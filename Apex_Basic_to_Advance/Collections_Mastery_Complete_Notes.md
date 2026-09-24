# Collections Mastery — Complete English Notes

> **Format:** Simple → Advanced → Super Advanced → Interview → Errors & Gotchas → Limits → Best Option → Real-World Use Case → AI Engineering → Summary → Original Source
> **Important:** Source ke all 5 topics aur original content preserve kiye gaye hain. The explanation is arranged in a clean learning order.

---

# 1. Simple

## Basic Idea of Collections

Apex has 3 main collection types:

| CollectionSimple MeaningBest Use |               |                           |
| -------------------------------- | ------------- | ------------------------- |
| `List<T>`                        | Ordered items | When order/index matters  |
| `Set<T>`                         | Unique items  | Membership/de-duplication |
| `Map<K,V>`                       | Key → Value   | Fast lookup/grouping      |

### List

List is an ordered collection:

```
List<String> names = new List<String>{'Amit', 'Ravi', 'Neha'};

names.add('Pooja');
names.remove(0);
Boolean found = names.contains('Ravi');
Integer index = names.indexOf('Neha');
names.set(0, 'Rahul');

List<String> copy = names.clone();
names.sort();
names.clear();
```

### Set

Set automatically removes duplicate values:

```
Set<String> ids = new Set<String>{'A', 'A', 'B'};
System.debug(ids.size()); // 2
```

When the question is **"does this value already exist?"**, Set is the natural choice.

### Map

Map looks up a value through its key:

```
Map<Id, Account> accountsById = new Map<Id, Account>();
accountsById.put(account.Id, account);

Account acc = accountsById.get(account.Id);
```

---

# 2. Advanced

## List Performance

Typical cost:

```
add/get/set       → O(1)
contains/indexOf  → O(n)
remove(value)     → O(n)
remove(index)     → O(n), because elements shift
```

Therefore, with large data:

```
for (String id : incomingIds) {
    if (existingList.contains(id)) {
        // ...
    }
}
```

This can become O(n²).
Better:

```
Set<String> existing = new Set<String>(existingList);

for (String id : incomingIds) {
    if (existing.contains(id)) {
        // fast membership check
    }
}
```

## `clone()` vs `deepClone()`

`clone()` is a shallow copy:

```
List<Account> copy = original.clone();
```

The List is copied, but contained sObject references can still be shared.
`deepClone()` creates independent sObject copies:

```
List<Account> copies = original.deepClone(false, false, false);
```

---

# 3. Set Methods

Important methods:

```
add()
addAll()
contains()
remove()
removeAll()
retainAll()
size()
isEmpty()
clear()
```

### Set Algebra

```
addAll     → Union
retainAll  → Intersection
removeAll  → Difference
```

Example:

```
Set<String> a = new Set<String>{'A', 'B', 'C'};
Set<String> b = new Set<String>{'B', 'C', 'D'};

Set<String> common = new Set<String>(a);
common.retainAll(b);
```

Result: `B, C`.

### De-duplication

```
List<String> values = new List<String>{'A', 'A', 'B', 'C', 'C'};
Set<String> unique = new Set<String>(values);
```

---

# 4. Map Methods

Important methods:

```
put()
get()
containsKey()
remove()
keySet()
values()
size()
isEmpty()
putAll()
```

Missing key:

```
Integer value = counts.get('missing');
```

The result can be `null`, so guard it:

```
if (counts.containsKey(key)) {
    Integer value = counts.get(key);
}
```

### `keySet()` + SOQL

```
Map<Id, Account> accountsById = new Map<Id, Account>(accounts);

List<Contact> contacts = [
    SELECT Id, AccountId
    FROM Contact
    WHERE AccountId IN :accountsById.keySet()
];
```

---

# 5. `Map<String, List<Account>>`

Ye is the classic **one-to-many grouping** pattern.
Example:

```
Technology → A1, A2, A3
Finance    → A4, A5
Retail     → A6
```

Code:

```
Map<String, List<Account>> byIndustry =
    new Map<String, List<Account>>();

for (Account acc : accounts) {
    if (!byIndustry.containsKey(acc.Industry)) {
        byIndustry.put(acc.Industry, new List<Account>());
    }

    byIndustry.get(acc.Industry).add(acc);
}
```

Ab ek industry ke all Accounts ek List mein mil jayenge.

---

# 6. `Map<Id, Set<Id>>`

This is useful for **unique many-to-many relationship tracking**.

```
Account Id
    ↓
Set<Contact Id>
```

Example:

```
Account A → Contact 1
Account A → Contact 2
Account A → Contact 2
Account B → Contact 2
```

Set removes duplicates:

```
Account A → {Contact 1, Contact 2}
Account B → {Contact 2}
```

Code:

```
Map<Id, Set<Id>> accountToContacts =
    new Map<Id, Set<Id>>();

for (SomeJunction__c row : rows) {
    if (!accountToContacts.containsKey(row.Account__c)) {
        accountToContacts.put(
            row.Account__c,
            new Set<Id>()
        );
    }

    accountToContacts.get(row.Account__c).add(row.Contact__c);
}
```

---

# 7. Super Advanced — Architect Thinking

## Avoiding O(n²)

If you repeatedly call `contains()` on the same large List inside a loop, you can introduce hidden O(n²) work.
Architect approach:

```
List
 ↓
Set
 ↓
Fast membership checks
```

## List vs Set vs Map

```
Need order/index?
       ↓
     List

Need uniqueness/membership?
       ↓
      Set

Need key → value lookup?
       ↓
      Map
```

## List + SaveResult Alignment

List order can be useful because `SaveResult[]` can align with the input order:

```
Database.SaveResult[] results =
    Database.insert(records, false);

for (Integer i = 0; i < results.size(); i++) {
    if (results[i].isSuccess()) {
        // results[i] corresponds to records[i]
    }
}
```

## Grouping vs Aggregate SOQL

If you only need counts/sums:

```
Aggregate SOQL / GROUP BY
```

If you need to process the actual records:

```
Map<Key, List<Record>>
```

## Parent → Children

```
Map<Id, List<Contact>> contactsByAccount =
    new Map<Id, List<Contact>>();

for (Contact c : contacts) {
    if (!contactsByAccount.containsKey(c.AccountId)) {
        contactsByAccount.put(c.AccountId, new List<Contact>());
    }

    contactsByAccount.get(c.AccountId).add(c);
}
```

You can convert the result of one bulk query into an in-memory parent→children structure.

## Many-to-Many Overlap

```
Set<Id> shared = new Set<Id>(map.get(accountA));
shared.retainAll(map.get(accountB));
```

`shared` contains the common IDs.

---

# 8. Interview

## Q1. `clone()` vs `deepClone()`?

**Answer:** `clone()` shallow copy hai — list copy hoti hai but sObject references shared ho sakte hain. `deepClone()` independent sObject copies banata hai, aur source ke according Id/timestamps/autonumber preservation options is availablen.

## Q2. `contains()` loop mein problem kyun?

**Answer:** List `contains()` performs an O(n) scan. Isko O(n) outer loop ke andar repeatedly use karne par O(n²) work ho sakta hai. Use Set for membership checks.

## Q3. Two collections ka intersection?

```
Set<Id> common = new Set<Id>(setA);
common.retainAll(setB);
```

`retainAll()` gives the intersection.

## Q4. Missing Map key?

`get()` returns `null` for a missing key. Use `containsKey()` to distinguish it.

## Q5. Trigger old vs new?

```
for (Account a : Trigger.new) {
    Account oldA = Trigger.oldMap.get(a.Id);

    if (a.Rating != oldA.Rating) {
        changed.add(a);
    }
}
```

`Trigger.oldMap` provides O(1) lookup by Id.

## Q6. `Map<Id, Set<Id>>` when?

Use it when IDs must be unique and you need fast membership/overlap checks, especially for many-to-many relationships.

---

# 9. Errors & Gotchas

### List

- `contains()` inside loop → O(n²) risk.
- `clone()` ko deep copy samajhna → shared-reference bug.
- Invalid index → `List index out of bounds`.
- Removing while iterating can cause skipped elements or errors.

### Set

- Set ordering is not guaranteed.
- String/Id equality is case-insensitive, so values can unexpectedly merge.
- Do not mutate while iterating.
- Equality/hash behavior matters for custom classes.

### Map

- Missing key → `null`.
- `containsKey()` guard is useful.
- String keys collision create kar sakte hain because source ke according case-insensitive behavior.
- Iteration ke time Map mutate karna unsafe hai.

### Nested Collections

Large:

```
Map → List → sObjects
```

ya:

```
Map → Set → IDs
```

can create heap pressure.

---

# 10. Limits

Limits/constraints stated by the source:

- `contains()` O(n).
- `clone()` shallow hai.
- `deepClone()` sObject Lists ke liye is available, custom-object Lists ke liye nahi.
- Set iteration order guaranteed nahi hai.
- Large Set operations can consume heap.
- `Map.get()` missing key par null deta hai.
- Large `putAll()` can temporarily increase peak heap.
- Nested grouping structures can be heap-heavy.
- Large `Map<Id,Set<Id>>` graphs mein per-entry hash overhead hota hai.
- Deeply nested Queueable state serialization can be expensive.
- Source ke according 15/18-character Id representations ko mix karna duplicate-entry issue create kar sakta hai.

---

# 11. Best Option + Why + Trade-offs

| RequirementCollectionWhy |                   |                        |
| ------------------------ | ----------------- | ---------------------- |
| Ordered records          | `List<T>`         | Order/index            |
| Unique values            | `Set<T>`          | Automatic de-duplication       |
| Fast key lookup          | `Map<K,V>`        | Key-based access       |
| Group records            | `Map<K,List<V>>`  | One-to-many            |
| Unique relationships     | `Map<Id,Set<Id>>` | De-duplication + membership    |
| Intersection             | `retainAll()`     | Set algebra            |
| Difference               | `removeAll()`     | Set algebra            |
| Union                    | `addAll()`        | Unique combined values |

### Trade-offs

**List**

- ✅ Order/index
- ❌ Membership scan

**Set**

- ✅ Unique/fast membership
- ❌ No guaranteed order/index

**Map**

- ✅ Fast lookup/grouping
- ❌ Extra key/value memory

---

# 12. Real-World Use Cases

## 10k Existing IDs

Problem:

```
Incoming records
       ↓
existingList.contains()
       ↓
CPU grows
```

Solution:

```
Set<String> existingIds = new Set<String>(existingList);

for (String id : incomingIds) {
    if (!existingIds.contains(id)) {
        // new record
    }
}
```

## Trigger Status Change

```
for (Account a : Trigger.new) {
    Account oldA = Trigger.oldMap.get(a.Id);

    if (a.Status__c != oldA.Status__c) {
        changed.add(a);
    }
}
```

## Industry Processing

```
Map<String, List<Account>> grouped =
    new Map<String, List<Account>>();
```

Query once → group in memory → apply industry-specific rules.

## Shared Contacts

```
Set<Id> shared =
    new Set<Id>(accountToContacts.get(accountA));

shared.retainAll(
    accountToContacts.get(accountB)
);
```

Common contacts are identified efficiently.

---

# 13. AI Engineering Connection

## Batch Inference

```
List<Input>
   ↓
Model inference
   ↓
List<Output>
```

List ordering is useful for input-output alignment.

## RAG Deduplication

```
Retrieved chunks
      ↓
Set<ChunkId>
      ↓
Duplicate removal
      ↓
Final context
```

## Fast Retrieval Mapping

```
docs_by_id = {
    "doc-1": {"title": "Apex"},
    "doc-2": {"title": "RAG"}
}
```

Dictionary/Map pattern provides fast lookup.

## Evaluation Grouping

```
Map<Model, List<Result>>
```

ya:

```
Map<Dataset, List<Prediction>>
```

Per-model/per-dataset metrics can be processed.

## Agent / Tool Relationships

```
Agent → Set<Tool IDs>
User → Set<Permission IDs>
Document → Set<Chunk IDs>
```

`Map<Id, Set<Id>>` style thinking is useful for unique relationships and membership checks.

---

# 14. Coding Examples — Python / JavaScript / Java / TypeScript

## Python

```
names = ["Amit", "Ravi", "Amit"]
names.append("Neha")

unique_names = set(names)

accounts_by_id = {
    "001": "Acme",
    "002": "Globex"
}

if "001" in accounts_by_id:
    print(accounts_by_id["001"])
```

## JavaScript

```
const names = ["Amit", "Ravi", "Amit"];
names.push("Neha");

const uniqueNames = new Set(names);

const accountsById = new Map();
accountsById.set("001", "Acme");

console.log(accountsById.get("001"));
```

## Java

```
List<String> names = new ArrayList<>();
names.add("Amit");
names.add("Ravi");

Set<String> uniqueNames = new HashSet<>(names);

Map<String, String> accountsById = new HashMap<>();
accountsById.put("001", "Acme");

System.out.println(accountsById.get("001"));
```

## TypeScript

```
const names: string[] = ["Amit", "Ravi", "Amit"];
names.push("Neha");

const uniqueNames = new Set<string>(names);

const accountsById = new Map<string, string>();
accountsById.set("001", "Acme");

console.log(accountsById.get("001"));
```

> These examples map collection concepts to other languages; they are not replacements for Apex-specific behavior.

---

# 15. One-Page Cheat Sheet

```
LIST
 ├─ Ordered
 ├─ Index based
 ├─ add/get/set → O(1)
 ├─ contains/indexOf/remove(value) → O(n)
 └─ clone = shallow

SET
 ├─ Unique
 ├─ Membership
 ├─ add/contains/remove → O(1)
 ├─ retainAll → intersection
 ├─ removeAll → difference
 ├─ addAll → union
 └─ No guaranteed order

MAP
 ├─ Key → Value
 ├─ get → O(1)
 ├─ containsKey
 ├─ keySet → query filters
 ├─ Id-keyed Map → trigger comparisons
 └─ Map<K,List<V>> → grouping

MAP<Id,SET<Id>>
 ├─ Unique relationships
 ├─ Many-to-many
 ├─ Membership
 └─ Intersection / overlap
```

---

# 16. Final Summary

More important than collection syntax is the **access pattern**:

- **Order/index → List**
- **Unique + membership → Set**
- **Key lookup → Map**
- **Grouping → `Map<K,List<V>>`**
- **Unique relationship graph → `Map<Id,Set<Id>>`**

Architect mindset:

```
Wrong structure
     ↓
Nested loops
     ↓
O(n²) + CPU/heap pressure

Right structure
     ↓
Set / Map
     ↓
Better lookup + bulkification
```

In production Apex:

```
Correct Collection
+ Bulkification
+ Heap Awareness
+ Governor Limit Awareness
= Scalable Apex
```

---

# Original Source — Preserved Completely

# Collections Mastery

5 topics · Series 7: Apex Fundamentals

#### Topics on this page

[List add/remove/contains/clone](#list-add-remove-contains-clone)[Set Methods](#set-methods)[Map Methods](#map-methods)[Map\<String,List\<Account>>](#map-string-list-account)[Map\<Id,Set\<Id>>](#map-id-set-id)

## List add/remove/contains/clone

Mastering List methods and their performance characteristics.
🌱

### Simple

Beginner - plain language
Core List methods: `add(v)`, `add(i,v)`, `remove(i)`, `contains(v)`, `indexOf(v)`, `set(i,v)`, `clone()`, `sort()`, `clear()`. They build and manipulate ordered collections.
📘

### Advanced

Working developer depth
Know the costs: `add`/`get(i)`/`set(i)` are O(1); `contains`/`indexOf`/`remove(value)` scan the list O(n). `remove(index)` shifts elements. `clone()` is a **shallow** copy (elements shared); for sObjects, `deepClone()` exists on List. `sort()` uses natural/Comparable order. For frequent membership tests, use a **Set** instead.
🧠

### Super Advanced

Senior / Architect depth
Architect depth: choosing List ops by cost avoids hidden O(n²). (1) **contains/indexOf are O(n)** — calling them inside a loop over the same list is O(n²); convert to a **Set** for O(1) membership or a **Map** for keyed lookups. (2) **Shallow vs deep clone** — `clone()` copies the list but shares element references (mutating an element affects both); `deepClone(preserveId, preserveTimestamps, preserveAutonumber)` on sObject lists makes independent copies — essential when duplicating records for insert. (3) **remove(index) cost** — shifts subsequent elements (O(n)); building a new filtered list is often clearer/cheaper than repeated removes. (4) **sort() with Comparable** — implement `compareTo` on a wrapper for custom ordering SOQL can't express. (5) **Index alignment** — Lists keep order, enabling alignment with `SaveResult[]`. (6) **Heap** — large lists cost heap; stream with SOQL for-loops. Architects pick the right collection/op for the access pattern, using Set/Map to avoid O(n²) and deepClone for true record copies.

```
List<Account> copies = original.deepClone(false, false, false);  // independent records (no Ids)
insert copies;
```

🎯

### Interview

Q&A + how to answer
**Q: Difference between `clone()` and `deepClone()` on a List of sObjects?**
A: `clone()` is **shallow** — copies the list but shares the same sObject references. `deepClone()` creates **independent** sObject copies (optionally preserving Id/timestamps/autonumber), so mutating or inserting one doesn't affect the originals.
🐞

### Errors & Gotchas

What breaks & why

- **O(n²)** from `contains` in a loop.
- **Shared mutations** from shallow `clone()`.
- **"List index out of bounds".**

📏

### Limits

Governor & platform limits

- `contains()` is O(n) - use a `Set` for membership tests inside loops.
- `clone()` is shallow; nested sObjects and collections are shared.
- `deepClone()` exists only on sObjects, not on Lists of custom objects.
- Removing while iterating throws or skips elements - iterate a copy.

✅

### Best Option (with reason)

Decision + trade-off
**Best option: Set/Map for membership/lookups, deepClone for independent record copies, Comparable for custom sort.**
**Why:** It matches operation cost to access patterns, avoiding O(n²) and shared-reference bugs.
🏢

### Real-World Use Case

Project scenario
**Scenario:** Code filters 10k records by checking `existing.contains(x)` in a loop and times out on CPU.
**Solution:** Load `existing` into a `Set` and test membership in O(1) — turning O(n²) into O(n) and eliminating the CPU-time error.

## Set Methods

Set operations for uniqueness, membership, and set algebra.
🌱

### Simple

Beginner - plain language
Core Set methods: `add`, `addAll`, `contains`, `remove`, `removeAll`, `retainAll`, `size`, `isEmpty`, `clear`. Sets keep unique values with O(1) operations.
📘

### Advanced

Working developer depth
Beyond basics, Sets do **set algebra**: `retainAll(other)` = intersection, `removeAll(other)` = difference, `addAll(other)` = union. `contains`/`add`/`remove` are O(1). Convert List↔Set to de-dup (`new Set<T>(list)`) or to sort (Set→List→sort). Remember String/Id equality is **case-insensitive**.
🧠

### Super Advanced

Senior / Architect depth
Architect depth: Set methods express comparisons concisely and efficiently. (1) **Intersection/difference** — `retainAll`/`removeAll` replace nested-loop comparisons (O(n·m)→O(n)); e.g., "which incoming ids already exist" = copy the incoming set and `retainAll(existingIds)`. (2) **De-dup pipeline** — `new Set<String>(list)` then back to a List removes duplicates in two lines. (3) **Membership guards** — O(1) `contains` for "seen?" checks and recursion guards (`Set<Id> processed`). (4) **Building query filters** — collect unique foreign keys for one `IN :set` query. (5) **Case-insensitivity gotcha** — String/Id sets treat 'A'/'a' as equal, merging entries unexpectedly; normalize case if exact distinction matters. (6) **No order/index** — convert to List for ordered output. (7) **Mutating during iteration** throws; collect changes separately. Architects use set algebra to replace nested loops, build clean query filters, and guard recursion, while accounting for case-insensitive equality.
🎯

### Interview

Q&A + how to answer
**Q: How do you find the records common to two collections efficiently?**
A: Put both in Sets and call `setA.retainAll(setB)` — that's the **intersection** in O(n), replacing an O(n·m) nested loop. `removeAll` gives the difference and `addAll` the union.
🐞

### Errors & Gotchas

What breaks & why

- **Case-insensitive merging** of String/Id values.
- **Mutating a Set during iteration.**
- **Expecting order** from a Set.

📏

### Limits

Governor & platform limits

- Iteration order is not guaranteed and must never be relied on.
- Custom classes need `equals()` and `hashCode()` or duplicates are stored.
- `retainAll`/`removeAll` allocate new internal structures - watch heap on large sets.
- Bounded by heap, not by a documented element count.

✅

### Best Option (with reason)

Decision + trade-off
**Best option: Set algebra (retainAll/removeAll/addAll) for comparisons, Sets for de-dup, membership guards, and query-filter building.**
**Why:** It turns nested-loop comparisons into O(n) operations and guarantees uniqueness.
🏢

### Real-World Use Case

Project scenario
**Scenario:** You must skip importing records whose external ids already exist among thousands.
**Solution:** Build a `Set<String>` of existing external ids and check `contains` per incoming record (or `removeAll` to compute the new ones) — fast and duplicate-free.

## Map Methods

Map operations for keyed access, grouping, and bulk patterns.
🌱

### Simple

Beginner - plain language
Core Map methods: `put(k,v)`, `get(k)`, `containsKey(k)`, `remove(k)`, `keySet()`, `values()`, `size()`, `isEmpty()`, `putAll()`. They give O(1) key→value access.
📘

### Advanced

Working developer depth
`get(missingKey)` returns **null** (guard or use `containsKey`). `keySet()` returns a Set (great for `IN :map.keySet()` queries); `values()` returns a List. The Id-keyed constructor `new Map<Id,SObject>(list)` auto-indexes records. `putAll` merges maps. Build `Map<Id,List<X>>` for grouping.
🧠

### Super Advanced

Senior / Architect depth
Architect depth: Map methods power many of the most important Apex patterns. (1) **keySet() for queries** — `WHERE Id IN :myMap.keySet()` reuses the map's keys as a query filter without building a separate Set. (2) **Id-keyed constructor** — `new Map<Id,Account>(accts)` and `Trigger.newMap/oldMap` instantly index by Id for O(1) old-vs-new comparisons in triggers. (3) **Grouping idiom** — `if(!m.containsKey(k)) m.put(k,new List<X>()); m.get(k).add(v);` builds parent→children maps in one pass (replacing sub-query gymnastics in code). (4) **get returns null** — always guard or `containsKey` to avoid NPEs; a missing key and a null value are indistinguishable via `get` alone. (5) **Composite keys** — concatenated String keys index by multiple dimensions. (6) **Case-insensitive String keys** — 'A'/'a' collide. (7) **values()/keySet() are live-ish** — iterate carefully; don't mutate during iteration. Architects build query filters, trigger comparisons, and groupings on Map methods as the backbone of bulkified logic.

```
for (Account a : Trigger.new)
  if (a.Rating != Trigger.oldMap.get(a.Id).Rating)   // O(1) old-vs-new compare
    changed.add(a);
```

🎯

### Interview

Q&A + how to answer
**Q: How do you compare old and new field values in an update trigger?**
A: Use `Trigger.oldMap.get(record.Id)` for an O(1) lookup of the prior version and compare fields against `Trigger.new`. The Id-keyed maps make per-record old-vs-new comparison clean and bulk-safe.
🐞

### Errors & Gotchas

What breaks & why

- **NPE** from `get()` on a missing key.
- **Case-insensitive String keys** colliding.
- **Mutating a Map during iteration.**

📏

### Limits

Governor & platform limits

- `get()` on a missing key returns null - use `containsKey()` to distinguish from a stored null.
- `keySet()` and `values()` return live views; modifying while iterating is unsafe.
- `putAll()` on a large map can double peak heap momentarily.
- Key types are restricted to primitives, sObjects, enums and classes with equals/hashCode.

✅

### Best Option (with reason)

Decision + trade-off
**Best option: keySet() for IN-queries, Id-keyed maps for trigger comparisons, and `Map<Id,List<X>>` grouping — with containsKey guards.**
**Why:** These idioms drive bulkification and relationship resolution at O(1).
🏢

### Real-World Use Case

Project scenario
**Scenario:** A trigger must act only on records whose Status changed.
**Solution:** Compare `Trigger.new` values to `Trigger.oldMap.get(id).Status` in a single loop — detecting changes in O(1) per record without extra queries.

## Map\<String,List\<Account>>

Grouping records by a key — the canonical one-to-many in-memory structure.
🌱

### Simple

Beginner - plain language
`Map<String, List<Account>>` groups accounts under a key (e.g., by Industry): each key maps to a **list** of matching records. Perfect for "group records by X" in memory.
📘

### Advanced

Working developer depth
Build it using the grouping idiom: for each record, if the key isn't present, put a new empty List, then add the record to `map.get(key)`. This avoids per-group queries and lets you process each group together (rollups, comparisons, batching by category) — all from a single query.
🧠

### Super Advanced

Senior / Architect depth
Architect depth: keyed lists are the in-memory equivalent of GROUP BY for record-level processing. (1) **One query, many groups** — query all relevant records once, then group in memory by parent id, type, or any field — replacing N queries or awkward sub-query handling. (2) **Parent→children** — `Map<Id, List<Contact>>` keyed by AccountId lets a parent loop access its children in O(1) for rollups/validation without re-querying. (3) **Grouping idiom** — the containsKey/put-empty-list/add pattern (or a helper) builds it in one pass. (4) **Composite keys** — `region+'|'+tier` groups by multiple dimensions. (5) **vs aggregate SOQL** — use SOQL GROUP BY when you only need counts/sums; use keyed lists when you need the *records* themselves for processing. (6) **Case-insensitive keys** — normalize String keys to avoid accidental merges. (7) **Memory** — large groupings cost heap; consider chunking. Architects use keyed lists to process records by category from a single bulk query, the in-memory complement to aggregate queries.
🎯

### Interview

Q&A + how to answer
**Q: How do you build a parent-to-children map from a single child query?**
A: Query children with their parent id, then group: `if(!m.containsKey(c.AccountId)) m.put(c.AccountId, new List<Contact>()); m.get(c.AccountId).add(c);` — yielding `Map<Id, List<Contact>>` for O(1) per-parent child access.
🐞

### Errors & Gotchas

What breaks & why

- **NPE** adding to a list before initializing it.
- **Case-insensitive String keys** merging groups.
- **Heap pressure** on huge groupings.

📏

### Limits

Governor & platform limits

- Nested collections multiply heap - a map of 10,000 keys each holding 20 Accounts is often the cause of a 6 MB failure.
- String keys are case-insensitive in Apex maps, which can silently merge entries.
- Build inside a SOQL for-loop to avoid materialising the full result set first.

✅

### Best Option (with reason)

Decision + trade-off
**Best option: keyed-list grouping from a single bulk query for record-level by-category processing; aggregate SOQL when only counts/sums are needed.**
**Why:** It resolves one-to-many relationships in memory from one query, avoiding query-in-loop.
🏢

### Real-World Use Case

Project scenario
**Scenario:** You must apply per-industry logic to thousands of accounts.
**Solution:** Query once and build `Map<String, List<Account>>` keyed by Industry, then iterate each group applying its rules — one query, clean per-category processing.

## Map\<Id,Set\<Id>>

Mapping a key to a set of unique related IDs — many-to-many in memory.
🌱

### Simple

Beginner - plain language
`Map<Id, Set<Id>>` maps each id to a **set of unique related ids** — e.g., each Account → the Set of its related Contact ids. Great for de-duplicated relationship tracking.
📘

### Advanced

Working developer depth
Like the keyed-list pattern but the value is a **Set**, guaranteeing **unique** related ids and O(1) membership. Ideal for many-to-many relationships (junction objects), tracking "which children belong to which parent" without duplicates, or accumulating unique references across a loop.
🧠

### Super Advanced

Senior / Architect depth
Architect depth: id→set-of-ids models de-duplicated relationships efficiently. (1) **Junction/many-to-many** — from a junction-object query, build `Map<Id, Set<Id>>` (e.g., Course→Set of Student ids) to answer membership/overlap questions in O(1) without re-querying. (2) **Uniqueness for free** — the Set value prevents duplicate related ids that a List would accumulate (e.g., when the same relationship appears multiple times in the source). (3) **Overlap analysis** — combine with set algebra: `map.get(a).retainAll(map.get(b))` finds shared related ids (common contacts between two accounts) in O(n). (4) **Grouping idiom** — init the inner Set on first use, then `add`. (5) **Membership guards** — `map.get(parent).contains(childId)` for fast "is related?" checks. (6) **Memory** — large fan-outs cost heap; chunk if needed. (7) **Composite/typed keys** for richer relationships. Architects use ID→set maps to model and query many-to-many relationships in memory, enabling overlap/membership logic from a single query.
🎯

### Interview

Q&A + how to answer
**Q: When would you choose `Map<Id, Set<Id>>` over `Map<Id, List<Id>>`?**
A: When related ids must be **unique** and you need fast membership/overlap checks — the Set value auto-dedupes and gives O(1) `contains`, ideal for many-to-many (junction) relationships where the same pair may appear multiple times.
🐞

### Errors & Gotchas

What breaks & why

- **NPE** adding before initializing the inner Set.
- **Using a List** and accumulating duplicate ids.
- **Heap pressure** on large fan-outs.

📏

### Limits

Governor & platform limits

- Sets inside maps carry per-entry hash overhead - measure heap on large graphs.
- Id keys are safest as 18-character; mixing 15 and 18 creates duplicate entries.
- Deeply nested structures are expensive to serialise for Queueable state - prefer a cursor.

✅

### Best Option (with reason)

Decision + trade-off
**Best option: `Map<Id, Set<Id>>` for unique many-to-many relationship tracking and O(1) membership/overlap (with set algebra).**
**Why:** The Set value guarantees uniqueness and fast relationship queries from one bulk query.
🏢

### Real-World Use Case

Project scenario
**Scenario:** You need to find contacts shared between two accounts via a junction object.
**Solution:** Build `Map<Id, Set<Id>>` (Account→Set of Contact ids) from the junction query, then `retainAll` the two sets to get the shared contacts in O(n) — no extra queries.