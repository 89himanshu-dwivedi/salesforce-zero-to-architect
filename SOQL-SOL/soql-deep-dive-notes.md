# SOQL Deep Dive Notes

## 1. SELECT / FROM / WHERE — Basics

- `SELECT` → kaunse fields
- `FROM` → kaunsa object
- `WHERE` → kaunse records
- `SELECT *` nahi hota → fields explicitly likhne padte hain
- `FROM` mein sirf **one object** allowed
- Apex variable ke liye **bind variable** `:variable` use hota hai

### Performance yahin se decide hoti hai
- Sirf required fields select karo → heap/serialization kam hoga
- Large Data Volume (LDV) mein selective indexed `WHERE` use karo
- `Id`, `External Id`, audit fields, custom indexes useful hote hain
- `WHERE Id IN :ids` → bulk-safe + injection-safe
- 100/200 SOQL limit aur 50k rows limit ka dhyan rakho
- Large org mein queries ko **Selector Layer** mein centralize karna useful hai

> **Large data hai?** → Indexed + selective `WHERE`

---

## 2. ORDER BY + LIMIT + OFFSET

- `OFFSET` maximum **2,000**
- Deep pagination mein `OFFSET` bad choice hai
- Database skipped rows ko bhi scan/discard karta hai (performance cost)
- Indexed field par `ORDER BY` better hota hai
- `LIMIT` top-N queries ke liye useful
- Stable pagination ke liye tie-breaker `Id` add karo

### Pagination Strategy
- **Huge data / infinite scroll?** → Keyset pagination
- **Top-N** → indexed `ORDER BY` + `LIMIT`
- **Large pagination** → keyset pagination

### NULLS Ordering
- `ASC` → NULLS FIRST
- `DESC` → NULLS LAST

---

## 3. Selective vs Non-Selective WHERE Clauses

- `!=`, `NOT`, `NOT IN`, `!= null` → generally **non-selective** ho sakte hain
- Range conditions:
  - Narrow range → selective
  - Huge range → non-selective

---

## 4. LIKE + IN + NOT IN

### 🌱 Simple
```apex
Name LIKE 'Acme%'
```
- `%` → any number of characters
- `_` → one character
- `IN` → list mein koi value match
- `NOT IN` → list ko exclude

### Performance
- `LIKE 'prefix%'` → good (selective, can use index)
- `LIKE '%term%'` → bad for LDV (non-selective, full scan)

### Trade-off
`LIKE` simple hai; SOSL actual text search ke liye more suitable hai.

### ✅ Best Practice
| Need | Use |
|---|---|
| Prefix match | `LIKE 'x%'` |
| Contains match | SOSL |
| Collection match | `IN :collection` |

---

## 5. INCLUDES / EXCLUDES

- Existing small multi-select picklist field → `INCLUDES` / `EXCLUDES`
- New scalable relationship requirement → Junction Object use karo (not multi-select picklist)

---

## 6. AND / OR / NOT Logic

- `AND` → dono condition true honi chahiye
- `OR` → koi ek condition true honi chahiye
- `NOT` → reverse/negate

### Selectivity Impact
| Operator | Effect | Selectivity |
|---|---|---|
| `AND` | Narrows result | Selective hone ka chance |
| `OR` | Widens result | Index usage difficult |
| `NOT` | Negative condition | Usually non-selective |

### Tips
- Complex `AND`/`OR` combos? → Use parentheses to group clearly
- LDV + `OR` slow? → Check Query Plan, aim for selective queries

---

## 7. Date Literals

`TODAY`, `YESTERDAY`, `THIS_WEEK`, `LAST_WEEK`, `NEXT_WEEK`, `THIS_MONTH`, `THIS_QUARTER`, `THIS_YEAR`, `LAST_N_DAYS:30`, `NEXT_N_DAYS:30`, `THIS_FISCAL_QUARTER`

---

## 8. Child → Parent (Dot Notation) & Multi-Level Parent Query

### Super Advanced — Parent Traversal
- Up to **5 parent levels**
- Parent ke fields same SOQL query mein aa jaate hain
- N+1 queries avoid ho jaati hain (single query instead of many)
- Bahut saare parent fields select karoge → heap increase hoga
- Null parent ho to related fields **null** milenge

### Limits & Cautions
- 5-level cap on parent traversal
- Deep filtering LDV mein non-selective ho sakti hai
- Chain mein null aa sakta hai (broken lookup)
- Very deep / frequently accessed hierarchy mein **denormalization** better ho sakta hai

---

## 9. Parent → Child Sub-Query

### Important Rules
- Maximum **20 child sub-queries**
- Only **1 level down**
- Child query ka `WHERE` / `ORDER BY` / `LIMIT` alag ho sakta hai (independent of parent)
- Child rows **50k total row limit** mein count hote hain
- Returned children heap consume karte hain

### Limits Summary
- Max 20 sub-queries
- Only 1 level down
- Child rows → count towards 50k row limit + heap

---

## 10. Nested Relationship Query (Parent → Child + Child → Parent)

- **Down** (child) = only 1 level
- **Up** (parent) = max 5 levels
- Max 20 child sub-queries
- Total rows = 50k limit applies
- Heap is shared across the whole query
- Outer query **selective** hona chahiye

---

## 11. Polymorphic `WhoId` / `WhatId`

- `WhoId` → Contact OR Lead
- `WhatId` → Account OR Opportunity etc.

### Super Advanced — `TYPEOF`
Main benefit: Different object types ke liye **one single SOQL** query.

- `.Type` se type identify/filter kiya ja sakta hai
- `TYPEOF` se type-specific fields select ho sakte hain
- Multiple possible types hone ki wajah se Apex/reporting/integration logic **type-aware** hona chahiye
- Type filter karke query ko narrow karna useful ho sakta hai

| Use | Purpose |
|---|---|
| `.Type` | Filtering/branching |
| `TYPEOF` | Type-specific field selection |

---

## 12. Relationship Query — Quick Reference

| Requirement | Immediately Think |
|---|---|
| Contact ka Account Name | Child → Parent dot notation |
| Contact → Account → Owner | Multi-level parent |
| Account ke Contacts | Parent → Child sub-query |
| Account → Opp → Opp Owner | Nested relationship |
| WhatId Account/Opp dono ho sakta | Polymorphic + `TYPEOF` |
| Custom parent | `__r` |
| Deep parent | Max 5 levels up |
| Bahut saare children | Max 1 level down |
| Children ko further filter karna | `WHERE` + `LIMIT` / separate query |
| Multiple object types | `.Type` / `TYPEOF` |

---

## 13. COUNT() — Two Types

| Function | Kya Count Karta Hai |
|---|---|
| `COUNT()` | Total matching rows |
| `COUNT(Email)` | Sirf non-null Email rows |

---

## 14. COUNT_DISTINCT()

**Use cases:**
- Unique Customers
- Unique Products
- Unique Sales Reps

---

## 15. SUM()

Return type = `AggregateResult`

```apex
AggregateResult ar = [
    SELECT SUM(Amount) total
    FROM Opportunity
];

Decimal revenue = (Decimal)ar.get('total');
```

### Super Advanced
- Database-level aggregation → CPU save hoti hai
- Multi-currency org → `convertCurrency()` ka dhyan rakho
- `GROUP BY` se subtotals mil sakte hain

---

## 16. AVG()

- Average **skew** ho sakta hai (outliers ka effect)
- Null ko 0 treat karna hai to `SUM()/COUNT()` use karo
- **Median SOQL mein available nahi hai**

---

## 17. MAX() / MIN()

Largest/Smallest value nikalne ke liye — standard aggregate functions.

---

## 18. GROUP BY

- Non-aggregate fields ko `GROUP BY` mein hona **hi** chahiye
- Return type = `List<AggregateResult>`

---

## 19. HAVING

### 🌱 Simple
Groups ko filter karta hai (rows ko nahi).

```apex
SELECT AccountId, COUNT(Id)
FROM Case
GROUP BY AccountId
HAVING COUNT(Id) > 5
```

### WHERE vs HAVING

| | WHERE | HAVING |
|---|---|---|
| Filters | Rows | Groups |
| Timing | Before Group | After Group |

---

## 20. ROLLUP

### 🌱 Simple
Subtotal + Grand Total deta hai.

```apex
SELECT StageName, SUM(Amount)
FROM Opportunity
GROUP BY ROLLUP(StageName)
```

---

## 21. GROUPING()

```apex
GROUPING(StageName)
```

`GROUPING()` identify karta hai ki kaunsi rows **subtotal rows** hain.

---

## 22. ROLLUP vs CUBE

| | ROLLUP | CUBE |
|---|---|---|
| Structure | Hierarchy | Every Combination |
| Region Total | ✅ | ✅ |
| Stage Total | ❌ | ✅ |
| Grand Total | ✅ | ✅ |

| Requirement | Use |
|---|---|
| Hierarchical totals | ROLLUP |
| Cross-tab totals | CUBE |

---

## 23. CUBE

### 🌱 Simple
ROLLUP ka advanced version — har combination ka subtotal deta hai.

```apex
GROUP BY CUBE(Region__c, StageName)
```

---

## 24. Aggregate Functions — Business Requirement Cheat Sheet

| Business Requirement | Best Choice |
|---|---|
| Total records | `COUNT` |
| Unique records | `COUNT_DISTINCT` |
| Total revenue | `SUM` |
| Average deal | `AVG` |
| Largest deal | `MAX` |
| Earliest date | `MIN` |
| Stage-wise totals | `GROUP BY` |
| Groups with >100 records | `HAVING` |
| Subtotal + Grand Total | `ROLLUP` |
| Matrix totals | `CUBE` |
