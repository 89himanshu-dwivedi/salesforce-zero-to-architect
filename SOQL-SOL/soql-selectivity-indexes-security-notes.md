# SOQL — Selectivity, Indexes, Security & Dynamic SOQL

## 1. Query Selectivity

### 🌱 Simple
Selective query = `WHERE` condition records ko itna narrow kare ki Salesforce **index** use kar sake.
- Selective = fast
- Non-selective = large data par slow/fail

### Common Indexed Fields
- `Id`
- `Name`
- `CreatedDate`
- `SystemModstamp`
- Lookup / Master-Detail
- External Id
- Custom indexed fields

### Problem Solving — LDV + Slow SOQL + Trigger Error
1. `WHERE` dekho
2. Indexed field hai?
3. Filter selective hai?
4. Query Plan check karo
5. Index / filter redesign karo

> Optimizer roughly **10% / 5% thresholds** ko consider karta hai, aur indexed fields important hain.

---

## 2. Query Cost

- **Cost < 1** → generally good, index plan use hoga
- **Cost ≥ 1** → TableScan likely

```
Index Plan   TableScan   Sharing
        ↓
   Lowest cost wins
```

**Workflow:** Slow LDV query? → Query Plan → Cost dekho → Index/filter improve karo → Recheck

---

## 3. Query Plan Tool

### 🌱 Simple
Developer Console ka tool jo batata hai:
- Cost
- Cardinality
- Index used
- Leading operation

---

## 4. Explain Plan

### 🌱 Simple
Query Plan ki information ko API/programmatically dekhne ka way.

### 📘 Advanced
REST **Query Explain** candidate plans deta hai:
- Cost
- Cardinality
- Leading operation
- Fields/indexes used

> **Query Plan Tool** = interactive (Developer Console)
> **Explain API** = automation/CI

Query execute nahi hoti — sirf estimate milta hai.

**🎯 Use:** Critical LDV queries ke automated performance checks (CI pipeline mein).

---

## 5. Standard Index

### 🌱 Simple
Salesforce kuch fields ko automatically index karta hai:
- `Id`
- `Name`
- `OwnerId`
- `CreatedDate`
- `LastModifiedDate`
- `SystemModstamp`
- `RecordTypeId`
- Lookup / Master-Detail fields

> `SystemModstamp` especially **delta integrations** mein useful hota hai.

⚠️ **Indexed ≠ always selective.**

---

## 6. Custom Index

### 🌱 Simple
Jo field automatically indexed nahi hai, uske liye custom index banwaya ja sakta hai.

### 📘 Advanced
- External Id / Unique fields automatically indexed ho sakte hain
- Other fields ke liye Salesforce Support se request karni padti hai

---

## 7. External Id Index

### 🌱 Simple
- Auto-indexed
- Integration key ke roop mein use hota hai (upsert operations)

---

## 8. Unique Index

### 🌱 Simple
Unique field = duplicate values allowed nahi (hard database constraint).

---

## 9. Compound Index

### 🌱 Simple
Do fields ka combined index — jab do fields **together** selective hain but individually nahi.

---

## 10. Skinny Tables

### 🌱 Simple
Salesforce internally ek small denormalized copy maintain karta hai jisme frequently used fields hote hain.

### 📘 Advanced — Benefits
- Less data
- Less join overhead
- Faster LDV reads

### Modern Alternatives
- Indexing
- Sharing
- Record Types
- Business Units
- Archiving
- Big Objects

---

## 11. WITH SECURITY_ENFORCED

### 🌱 Simple
SOQL ko user's **FLS + CRUD** permissions respect karwata hai.

### ⚖️ Trade-off
- `SECURITY_ENFORCED` inaccessible field access hone par **throw** karta hai (exception)
- `stripInaccessible` inaccessible field ko silently **remove** karta hai

```
SECURITY_ENFORCED
        ↓
   FLS + CRUD
        ❌ Record Sharing (not enforced)
```

---

## 12. Feature Comparison — Security Options

| Feature | FLS/CRUD | Sharing | DML |
|---|---|---|---|
| `SECURITY_ENFORCED` | ✅ | ❌ | ❌ |
| `USER_MODE` | ✅ | ✅ | ✅ |
| `stripInaccessible` | ✅ | ❌ | Read/write sanitization |

---

## 13. stripInaccessible

### 🌱 Simple
Inaccessible fields ko **remove** karta hai, instead of throwing an exception.

---

## 14. Database.query() — Dynamic SOQL

### 📘 Advanced — Useful When
- Fields dynamic hain
- Filters optional hain
- Generic framework banana ho
- Metadata-driven UI banana ho

### 🧠 Super Advanced
**Biggest risk:** SOQL Injection

> Known query → Static SOQL
> Dynamic requirement → Dynamic SOQL + binds + allow-list + `USER_MODE`

---

## 15. Bind Variables

### 🌱 Simple
Apex variable ko SOQL mein `:` ke saath use karna.

### 📘 Advanced — Can Bind
- Primitive values
- List/Set
- Dates
- sObjects/field values

---

## 16. Dynamic Filters

### 🌱 Simple
User ne jo filters select kiye hain, unke basis par `WHERE` conditions runtime par build karna.

### 🧠 Super Advanced — 2 Major Risks

**1️⃣ Security — SOQL Injection**
Solution:
```
Bind values
+
Allow-list fields/operators
```

**2️⃣ Performance**
Agar user koi filter na de:
```sql
SELECT ... FROM Case
```
Millions records scan ho sakte hain. Isliye LDV par **mandatory selective/indexed filter** rakho.

### 🎯 Problem-Solving — Search Screen Flow
```
Optional filters
      ↓
Safe Query Builder
      ↓
Mandatory indexed filter
      ↓
Bind values
      ↓
Database.query()
```

### ⚠️ Best Architecture
Centralized **Query/Selector Builder** banao — har controller mein string concatenation mat karo.

---

## 17. 🔥 Advanced SOQL — Master Decision Map

```
Slow Query?
   ↓
Is it LDV?
   ↓
YES
   ↓
Check Selectivity
   ↓
Query Plan
   ↓
Cost < 1?
   ├── YES → Good
   └── NO
        ↓
   Indexed field?
        ├── NO → Custom Index
        └── YES
             ↓
          Data Skew?
             ↓
       Rewrite filter /
       Compound Index /
       Skinny Table
```

### 🔐 Security Decision

```
User-facing SOQL/DML
        ↓
     USER_MODE
        ↓
 FLS + CRUD + Sharing
```

- Gracefully restricted fields remove karne hain → `stripInaccessible`
- Exception chahiye for FLS/CRUD violations → `WITH SECURITY_ENFORCED`

### ⚡ Dynamic SOQL Decision

```
Query shape fixed?
   ↓
YES → Static SOQL

NO
 ↓
Dynamic SOQL
 ↓
Bind Variables
+
Allow-list identifiers
+
USER_MODE
+
Selective filter
```

---

## 18. 🎯 Interview Q&A — Most Important

1. **Selective query kya hai?**
   Indexed + sufficiently narrow filter.

2. **Query Cost < 1 ka kya meaning hai?**
   Optimizer ke according index plan better hai.

3. **Slow SOQL troubleshoot kaise karein?**
   Query Plan Tool se.

4. **Standard vs Custom Index?**
   Standard Salesforce automatically maintain karta hai; custom additional indexing hai (Support request se).

5. **External Id ka benefit?**
   Auto-index + integration upsert key.

6. **Unique vs Duplicate Rule?**
   Unique = hard database constraint; Duplicate Rule = fuzzy/user-facing dedup.

7. **Compound index kab use karein?**
   Jab two fields together selective hain, individually nahi.

8. **Skinny Table kab consider karein?**
   Proven LDV bottleneck ke baad, normal indexing/selectivity tuning ke baad bhi.

9. **USER_MODE vs SECURITY_ENFORCED?**
   `USER_MODE` = FLS + CRUD + sharing; `SECURITY_ENFORCED` = sirf FLS/CRUD.

10. **stripInaccessible kab use karein?**
    Jab inaccessible fields ko gracefully strip karna ho (no exception).

11. **Dynamic SOQL ka biggest risk kya hai?**
    SOQL Injection.

12. **Injection kaise prevent karein?**
    Bind variables + allow-listed field/operator names.

13. **WHERE vs HAVING?**
    `WHERE` rows ko filter karta hai; `HAVING` groups ko filter karta hai.

14. **LDV mein most important performance concept?**
    Selectivity + Index + Query Plan.

---

## 19. 🚀 LinkedIn Post Draft

> 🔥 **Salesforce Advanced SOQL — What Separates a Developer from a Scalable Developer?**
>
> Basic SOQL is about fetching records.
> Advanced SOQL is about fetching them securely and at scale.
>
> Here are the concepts every Salesforce developer should understand:
>
> 🔹 **Query Selectivity**
> Indexed + selective filters are critical for Large Data Volume (LDV).
>
> 🔹 **Query Plan & Cost**
> Don't guess why a query is slow. Check the plan, cardinality, leading operation and cost.
>
> 🔹 **Indexes**
> - Standard Index
> - Custom Index
> - External Id Index
> - Unique Index
> - Compound Index
>
> 🔹 **Skinny Tables**
> A last-resort optimization for proven LDV bottlenecks when indexing alone isn't enough.
>
> 🔹 **Security**
> - `WITH SECURITY_ENFORCED`
> - `WITH USER_MODE`
> - `Security.stripInaccessible()`
>
> Know exactly what each one enforces.
>
> 🔹 **Dynamic SOQL**
> Flexibility comes with responsibility:
> ❌ String concatenation
> ✅ Bind variables
> ✅ Allow-list dynamic field/object names
> ✅ USER_MODE where appropriate
>
> 🔹 **Bind Variables**
> ```
> WHERE AccountId IN :accountIds
> ```
> One of the most important patterns for both bulkification and SOQL injection prevention.
>
> 🧠 **My biggest takeaway:**
> At scale, SOQL is not just about getting the right records — it's about getting them selectively, securely, and efficiently.
>
> `#Salesforce #SalesforceDeveloper #Apex #SOQL #SalesforceArchitecture #LDV #CRM #CloudComputing #SoftwareEngineering`
