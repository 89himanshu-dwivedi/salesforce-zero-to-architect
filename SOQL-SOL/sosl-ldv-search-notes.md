# SOSL & LDV Search Strategies

## 1. FIND + RETURNING + LIMIT

### 🌱 Simple
SOSL = Salesforce mein **text search**.

Jab tumhe pata nahi ki text kis object/field mein hai, SOSL useful hai.

| Requirement | Best |
|---|---|
| Multiple objects | SOSL |
| Text search | SOSL |
| `contains` type search | SOSL |
| Exact field filtering | SOQL |
| Relationships | SOQL |
| `WHERE Field = value` | SOQL |

### Advanced
SOSL search **index** use karta hai aur ek query se multiple objects search kar sakta hai.

```sql
FIND 'Acme' RETURNING Account(Id, Name), Contact(Id, LastName) LIMIT 20
```

### Limits
- **2,000 records** per SOSL query
- **20 SOSL** per transaction
- Search index **asynchronously** update hota hai

### 🏆 Best
- Multi-object text search → **SOSL**
- Precise structured query → **SOQL**

---

## 2. ALL FIELDS

### 🌱 Simple
```sql
IN ALL FIELDS
```
Matlab: Object ke saare searchable fields mein search karo.

| Approach | Trade-off |
|---|---|
| `ALL FIELDS` | Maximum recall, more results/noise, best for general search |
| Specific field group | Better precision, less unnecessary searching |

---

## 3. Wildcards `*` and `?`

### 🌱 Simple
- `*` → zero or more characters
- `?` → single character

---

## 4. Fuzzy Matching

### 🌱 Simple
Fuzzy ka matlab: exact spelling na hone ke baad bhi useful match mil jaye.

### Best Fit
| Requirement | Best |
|---|---|
| Search UI | SOSL + wildcard |
| Dedup | Matching Rules |

---

## 5. Exact Search

### 🌱 Simple
Jab exact value chahiye — fuzzy/broad search nahi.

---

## 6. Multi-Object Search ⭐

### 🌱 Simple
SOSL ka biggest advantage: **Ek query → multiple objects**

```
                Search Box
                    ↓
              One SOSL Query
        ┌───────────┼───────────┐
        ↓           ↓           ↓
     Account     Contact       Case
        ↓           ↓           ↓
       List        List        List
        └───────────┼───────────┘
                    ↓
             Merge / Rank
                    ↓
              Unified UI
```

| Requirement | Use |
|---|---|
| Search text | SOSL |
| Multiple objects | SOSL |
| Global search | SOSL |
| Name autocomplete | SOSL + `NAME FIELDS` |
| Email lookup | SOSL + `EMAIL FIELDS` |
| Phone lookup | SOSL + `PHONE FIELDS` |
| Partial text | SOSL + wildcard |
| Duplicate fuzzy matching | Matching Rules |
| Exact External ID | SOQL `=` |
| Exact field filtering | SOQL |
| Relationship query | SOQL |

---

## 7. SOQL vs SOSL — One-Line Summary

**SOQL** = "Mujhe pata hai data kahan hai."
```
FIND
  ↓
Search Group
  ↓
Search Index
  ↓
RETURNING
  ↓
Object-wise Lists
  ↓
WHERE / ORDER / LIMIT
```

**SOSL** = "Mujhe bas text pata hai, dhoondh ke batao."

---

## 8. Pagination Choice by Dataset Size

- Large dataset → **Keyset pagination**
- Small dataset → **OFFSET**

---

## 9. LDV Search Strategies 🔥🔥

**LDV** = Large Data Volume

### 🌱 Simple
Jab object mein millions/hundreds of millions records ho, normal query strategy enough nahi hoti.

**Need combination of:**
- Indexes
- Selective filters
- SOSL
- Keyset pagination
- Batch Apex
- Archiving
- Skinny Tables

### Decision Flow

```
Large Data?
      ↓
Do you need exact structured filtering?
      ↓
    YES
      ↓
Selective indexed SOQL

Text Search?
      ↓
SOSL

Huge UI list?
      ↓
Keyset Pagination

Mass Processing?
      ↓
Batch Apex

Huge Data Extraction?
      ↓
Bulk API + PK Chunking

Old/Cold Data?
      ↓
Archive / Big Objects

Performance doubt?
      ↓
Query Plan / Explain
```

---

## 10. Problem → Best Solution Cheat Sheet

| Problem | Best Solution |
|---|---|
| SOQL 101 | Bulkify |
| Query in loop | `IN :ids` |
| Fast lookup | Map |
| Large result + heap | SOQL For Loop |
| >50k processing | Batch Apex |
| Small pagination | OFFSET |
| Deep pagination | Keyset |
| Millions of records | Keyset + indexes |
| Text search | SOSL |
| Huge export | Bulk API + PK Chunking |
| Old data | Archive |
| Query performance check | Query Plan / Explain |
