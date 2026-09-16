# Apex Fundamentals — Variables, Types, Collections & Control Flow

## 1. Variables

Typed storage and immutable values — the foundation of Apex code.

A variable is a named, typed container used to store a value.

```apex
Integer count = 0;
```
- `Integer` → data type
- `count` → variable name
- `0` → stored value

Apex is a **strongly typed** language — every variable must have a declared data type.

### Key Characteristics
- Statically typed
- Variable names are **case-insensitive**
- Default value is `null` if not initialized

### Scope — 3 Types
| Scope | Meaning |
|---|---|
| Local | Inside a method/block |
| Instance | Per object |
| Static | Per transaction/class |

### Value vs Reference
- **Primitives** → value is fixed
- **Objects** → the reference is fixed, but the object's internal contents can change

### `final` Semantics
```apex
final List<String> L = new List<String>();
```
`L` cannot be reassigned to a different List. Note: `final` makes the **reference** immutable, not the object itself.

### Static = Transaction-Scoped
Static variables persist within a single transaction. Useful for:
- Recursion guards
- Transaction-level caching
- Trigger re-entry control

⚠️ Static variables reset across async transactions and Batch Apex chunks.

### No Type Inference
Unlike JavaScript/modern languages (`var x = 10;`), Apex requires explicit types:
```apex
Integer x = 10;
```
**Benefits:** better readability, better compiler checks, clearer code intention.

### Limits
- Local variable names must be unique within the same scope
- Apex is case-insensitive, so `x` and `X` collide
- `final` makes the reference immutable, not the object
- Static variables only live for the transaction lifetime
- Heap consumption: ~6 MB synchronous, ~12 MB asynchronous

### Best Practice
- Use explicit types
- Use `public static final` for named constants
- Use Custom Metadata/Custom Settings for business-configurable values
- Use static variables deliberately for transaction-scoped state

**Why?** Improves readability, makes configuration manageable, reduces null bugs, avoids recursion issues.

---

## 2. Operators

Arithmetic, comparison, logical, assignment, and safe-navigation operators.

| Category | Operators |
|---|---|
| Arithmetic | `+ - * /` |
| Comparison | `== != < >` |
| Logical | `&& \|\| !` |
| Assignment | `= += -=` |
| Ternary | `condition ? value1 : value2` |

### Safe Navigation Operator `?.`
Helps avoid null-related errors:
```apex
account?.Name
account?.Owner?.Name
```
If `account == null`, the expression returns `null` instead of throwing an exception.

### sObject Equality
- `==` on sObjects compares field-value equality
- For actual record identity, compare by `Id` — it's safer and clearer

### Architect Rule
Architects generally:
- Use `?.` for null-safe chains
- Put null guards first
- Use `Decimal` for financial math
- Deliberately select numeric types

### Limits
- sObject `==` does field-by-field comparison
- `String ==` is case-insensitive; use `.equals()` for case-sensitive comparison
- Integer division truncates
- `&&` and `||` short-circuit — right-hand side side effects may not execute

### Best Practice
- `?.` for null-safe chains
- Null guard first in conditions
- Explicit `Decimal` for financial calculations

**Why?** Reduces NPEs, avoids precision bugs, keeps conditions concise.

---

## 3. Primitive Types

Built-in scalar types: `Integer`, `Long`, `Decimal`, `Double`, `Boolean`, `String`, `Date`, `Datetime`, `Time`, `Id`, `Blob`.

Apex primitives are basic value types — each generally represents a single value (collections like List/Set/Map store multiple values).

| Type | Description |
|---|---|
| `Integer` | 32-bit integer; for counts and normal whole-number calculations |
| `Long` | 64-bit integer; for large counts/numbers |
| `Decimal` | Preferred for money; exact decimal arithmetic |
| `Double` | Floating-point; for scientific/approximate calculations — avoid for money |
| `Id` | Salesforce record key; 15-character or 18-character form |
| `Blob` | Binary data (e.g., files) |
| `Date` | Date only, e.g. `2026-09-09` |
| `Datetime` | Specific instant in time (UTC semantics important) |
| `Time` | Time-of-day |

### Important
- Primitives are passed **by value**
- Uninitialized primitive values default to `null`

### Best Practices
1. **Currency** → use `Decimal`, never `Double`. Use `setScale()` and rounding modes.
   ```apex
   amount.setScale(2, RoundingMode.HALF_UP)
   ```
2. **Salesforce IDs**:
   - 15-character → case-sensitive
   - 18-character → case-safe
   - For external integrations, always prefer the 18-character Id
3. **Blob** — for binary data. Use `Blob.valueOf()` and `EncodingUtil` for conversions. Large blobs consume heap.

### Limits
- `Integer` (32-bit): approximately ±2,147,483,647 — beyond that, use `Long`
- `String` heap usage bounded by heap limits; long text fields capped around 131,072 characters
- `Blob` is bounded by heap limits; a 4 MB file Base64-encoded becomes roughly 5.5 MB
- `Id`: 15-char is case-sensitive, 18-char is case-insensitive

### Best Practice Summary
| Need | Use |
|---|---|
| Money | `Decimal` |
| Date/time instant | `Datetime` |
| Integration IDs | 18-char `Id` |
| Large counts | `Long` |

**Why?** Choosing types by actual business semantics avoids rounding bugs, timezone bugs, ID mismatches, and overflow.

---

## 4. sObject

A Salesforce record's typed in-memory representation.

### 🌱 Simple
An sObject is an in-memory representation of a Salesforce record.

### Typed vs Generic

**Typed:**
```apex
Account a;
```
Benefits: compile-time safety, autocomplete, readable code.

**Generic:**
```apex
SObject s;
```
Useful for metadata-driven/reusable frameworks (e.g., trigger frameworks, generic selectors).

Typed access: `a.Name`
Dynamic access: `s.get('Name');` / `s.put('Name', value);`

### Key Facts
- sObjects follow **reference semantics**
- Only fields that were **queried or explicitly set** are populated
- Relationship fields can represent parent/child records

### Pass-by-Reference
sObject references can be shared. Modifying a record inside a method:
```apex
void updateAccount(Account a) {
    a.Name = 'New Name';
}
```
...can make the change visible in the caller's object too.

For an independent copy, use clone:
```apex
a.clone(false, true);
```

### Example — Dynamic Access
```apex
SObject s = [SELECT Id, Name FROM Account LIMIT 1];
String n = (String) s.get('Name');
s.put('Name', n + ' Inc');
```

### Errors & Gotchas
- `"SObject row retrieved without selecting the field"` → field missing from `SELECT`
- Pass-by-reference can cause unintended mutation
- Invalid field name in dynamic `put()` throws an exception

### Limits
- Only queried/set fields are available
- Assigning a query directly to a single sObject variable:
  - 0 rows → `QueryException`
  - multiple rows → `QueryException`
- sObjects can be heap-heavy — query only required fields
- Serialized sObjects may include an `attributes` node — a DTO can be a cleaner option for external integrations

### Best Practice
| Need | Use |
|---|---|
| Business logic | Typed sObjects |
| Generic frameworks | `SObject` + Schema describe |
| Selectors | Query complete required field sets |
| Independent copy | Explicit `clone()` |

**Why?** Balances compile-time safety with reusable dynamic logic, while avoiding field-not-queried bugs and reference-sharing bugs.

---

## 5. Enum

A fixed set of named constants — type-safe and readable state.

An enum defines a fixed set of named values:
```apex
enum Season {
    WINTER,
    SPRING,
    SUMMER,
    FALL
}
```
Instead of using arbitrary Strings, use valid enum values.

**Benefits:** type safety, readable code, avoids "magic strings".

Enums can be used in:
- Variables
- Method parameters
- Return types
- `switch` statements

**Useful methods:** `values()`, `name()`, `ordinal()`

Enums provide compile-time safety and are **case-sensitive**.

**Common use cases:** status, stage, processing type, flags, transaction modes.

### Enum vs String

**String:**
```apex
String status = 'APPROVED';
```
Typos are possible (e.g. `'APPROVVED'`).

**Enum:**
```apex
Status.APPROVED
```
The compiler rejects invalid enum members.

### Mapping to Data
Apex enums can't hold custom properties/methods like Java. To bridge with a record field/picklist, use a mapping:
```apex
Map<String, MyEnum>
```
or a helper approach.

### Serialization
JSON serialization generally represents an enum as its **name**. Keep enum names stable in integrations — renaming a member (e.g., `APPROVED`) can break an external contract.

### When NOT to Use Enum
If values can be changed by an admin, avoid enums — code deployment would be required for changes. Use **Picklist** or **Custom Metadata** instead.

### Architect Rule
| Ownership | Use |
|---|---|
| Code-owned fixed values | Enum |
| Business/admin-owned changeable values | Picklist / Custom Metadata |

**Why?** Enum gives compile-time safety for stable states; metadata gives runtime/admin flexibility for business-owned values.

### Errors & Gotchas
- Hard-coding an admin-changeable list as an enum
- Renaming an enum member breaks JSON/integration contracts
- Expecting Java-like custom properties/methods on Apex enums

### Limits
- Apex enums don't define constructors, methods, or fields like Java
- `valueOf()` expects exact matching — a mismatched string throws an exception
- Arbitrary JSON strings don't automatically deserialize into enum values
- Adding an enum value in a managed package can be a breaking change for subscribers

---

## 6. List

Ordered, duplicate-allowing collection — Apex's workhorse.

### 🌱 Simple
A List is an ordered collection. Duplicates are allowed. Access via index.

Lists:
- Preserve insertion order
- Allow duplicates
- Provide index access

```apex
names.get(0);
names.set(0, 'C');
```

**Useful methods:** `add`, `addAll`, `remove(index)`, `size`, `isEmpty`, `contains`, `sort`, `clear`, `clone`

### Notes
- Partial-success DML results (`Database.SaveResult[]`) can align by index with the input List
- Primitive Lists' `sort()` uses natural order; for custom order, use a `Comparable` wrapper
- `contains()` / `remove()` are **O(n)**

### Heap Awareness
Large Lists consume heap — for large result sets, a SOQL for-loop is often better.

### Clone
`List.clone()` is **shallow**.

### Errors & Gotchas
- Invalid index → `List index out of bounds`
- DML inside a loop
- Repeated `O(n)` `contains()` for membership checks

### Limits
- Ordered/indexed, duplicates allowed
- Literal initializer supports approximately 1,000 elements
- Overall List size is bounded by heap
- DML on a List is capped around 10,000 rows per transaction
- `clone()` is shallow — nested objects may be shared

### Best Practice
| Need | Use |
|---|---|
| Ordered bulk operations | List |
| Index-aligned result handling | List |
| Uniqueness | Set |
| Lookup | Map |

---

## 7. Set

Unordered collection of unique values with fast membership tests.

If you add the same Id multiple times, the Set retains only one value.

- Sets are unordered
- Great for de-duplication and fast `contains()`
- Membership operations (`add`, `contains`, `remove`) generally provide **O(1)-average** behavior

**Useful methods:** `add`, `addAll`, `contains`, `remove`, `size`, `retainAll`, `removeAll`

### De-duplication
```apex
Set<String> unique = new Set<String>(list);
```

### No Ordering
- Iteration order is **not guaranteed**
- No index access
- If sorting/index is needed, convert Set → List

### Errors & Gotchas
- Treating `'A'` and `'a'` as different (String Sets are case-sensitive by value)
- Expecting ordering/index from a Set
- Using a List when uniqueness is actually required

### Limits
- Unordered, no guaranteed iteration order, no index access
- Members can be primitives, sObjects, enums, or objects with proper equality/hash behavior
- Custom Apex classes need proper `equals()` / `hashCode()` implementations
- Overall size bounded by heap

### Best Practice
Use Set for:
- Unique IDs/keys
- O(1)-style membership testing
- De-duplication
- Set algebra

**Why?** Makes bulk query filters and comparison operations efficient and duplicate-free.

---

## 8. Map

Key → Value collection for fast lookups — the core tool of bulkified Apex.

### 🌱 Simple
A Map stores key/value pairs:
```apex
Map<Id, Account> byId = new Map<Id, Account>();
```

### Behavior
- Keys are unique
- `put()` can overwrite an existing key
- `get()` returns the value; a missing key returns `null`
- `containsKey()` checks presence
- `keySet()` returns keys
- `values()` returns values

### Composite Keys
For lookups on multiple dimensions, build a composite String key.

### Architect Rule
Maps are core to scalable Apex. Use them for:
- Relationship resolution
- Grouping
- De-duplication
- In-memory joins
- Bulkification

### Errors & Gotchas
- Missing key → `get()` returns `null`; dereferencing it directly causes an NPE
- Case-sensitivity of String keys can cause entries to merge unexpectedly
- Querying inside a loop instead of using a Map lookup

### Limits
- Keys must follow supported types/equality semantics
- A Map can consume more heap than a List (keys + values + hashing overhead)
- `Map<Id, SObject>` is the standard bulkification pattern
- Missing key on `get()` returns `null`, not an exception — use `containsKey()` to check presence explicitly

### Best Practice
- Query once into `Map<Id, Account>`, then do O(1)-style lookups
- Group with `Map<Id, List<Contact>>`

**Why?** Eliminates query-in-loop, resolves relationships in memory, and makes bulk processing scalable.

---

## 9. If / Else

Conditional branching — the basic decision structure.

### Deep Nesting — Avoid It
Nested conditions beyond 2–3 levels reduce readability. Alternatives:
- Guard clauses
- `switch`
- Strategy Map

Architects generally prefer guard clauses, named Boolean methods, `switch` for discrete values, and strategy maps for extensible behavior, instead of huge if/else chains.

### Limits
- Every branch consumes CPU time
- Approximate governor CPU budgets: 10 seconds synchronous, 60 seconds asynchronous
- Deep conditional logic can create unreachable branches without an obvious compiler warning
- `null == null` is `true`, but `null.method()` throws an exception

### Best Practice
| Case | Use |
|---|---|
| Guard conditions | Guard clauses |
| Named conditions | Named Boolean helpers |
| Many discrete cases | `switch` |
| Highly extensible cases | Strategy Map |

**Why?** Keeps branching flat, readable, and testable.

---

## 10. Switch

Multi-way branching on a value — cleaner than long if/else chains.

### 🌱 Simple
Apex `switch` branches based on a value.

Supported case types: `Integer`, `Long`, `String`, `Enum`, sObject type, `Id`.

Apex `switch` has **no fall-through** — `break` isn't needed, and only the matching `when` executes.

```apex
switch on status {
    when DRAFT {
        ...
    }
    when APPROVED {
        ...
    }
}
```

### Switch vs Strategy Map
| Scenario | Best Choice |
|---|---|
| Small, fixed set | `switch` (clearest) |
| Frequently extending/open set | Strategy Map (`Map<String, Handler>`) |

Adding a new handler avoids modifying a giant switch statement — this supports the Open/Closed Principle.

### Null Handling
Use `when null` explicitly to consciously handle the null case.

### Limitations
`when` doesn't support arbitrary ranges/expressions.

### Architect Rule
| Logic Type | Use |
|---|---|
| Fixed discrete logic | `switch` |
| Open/extensible logic | Strategy Map |
| Range logic | if/else |

### Errors & Gotchas
- Expecting C/Java-style fall-through
- Trying ranges/expressions in `when`
- No `when else` → an unmatched value silently no-ops

### Limits
- Supported types: `Integer`, `Long`, `String`, `Enum`, sObject types (not `Decimal`/`Boolean`)
- String matching is case-insensitive
- No fall-through
- `when else` is optional; without it, an unmatched value with no match executes nothing

### Best Practice
| Need | Use |
|---|---|
| Fixed discrete dispatch | Switch |
| Enum dispatch | Switch |
| sObject type dispatch | Switch |
| Frequently extended/open case set | Strategy Map |

**Why?** Switch is clear for stable cases.

---

## 11. For Loop

### Index Alignment
A classic counter loop is useful when an input List needs to align by index with something like `Database.SaveResult[]`.

### Early Exit
Use `break;` / `continue;` when further processing is unnecessary or the current item should be skipped. Guard clauses inside loops reduce nesting.

### CPU Time
Heavy per-iteration logic consumes CPU — especially nested `O(n²)` loops, which are dangerous with large datasets. Use a `Map` for O(1)-style inner lookups instead.

### Architect Rule
Treat loops as **collection builders/processors**. Do I/O (SOQL, DML, callouts) **in bulk, outside the loop**.

### Errors & Gotchas
- SOQL/DML inside a loop → "Too many SOQL/DML" errors
- Materializing a huge List → heap errors
- Nested O(n²) loops → CPU-time issues

### Limits
- A SOQL for-loop retrieves/processes in chunks of approximately 200 sObjects
- Avoid SOQL/DML inside a loop
- Common transaction limits: 100 SOQL queries, 150 DML statements
- Nested loops are a major source of CPU-limit issues
- No fixed platform iteration cap on loop counters — CPU/heap are the practical boundaries

### Best Practice
| Case | Use |
|---|---|
| Normal collection processing | Enhanced for-loop |
| Large query results | SOQL for-loop |
| I/O | Bulk, outside the loop |
| Nested matching | Map |

**Why?** Better handles governor limits, heap, CPU, and scalability.

---

## 12. While Loop

Condition-first looping — repeats while a Boolean stays `true`.

### 🌱 Simple
```apex
while (hasMore) {
    ...
}
```
If `hasMore = false` initially, the body never executes.

### Advanced
`while` is useful when the iteration count isn't known in advance. Examples: consuming a queue, walking a cursor, pagination, processing while data is available.

**Important:** the loop body must eventually change the condition, or the loop runs indefinitely — subject to CPU time and governor limits. Avoid SOQL/DML inside the loop.

### Architect Rule
Use `while` when:
- Iteration count is unknown
- Work is condition-driven
- Termination is clearly guaranteed

For large/unbounded workloads, consider an async architecture.

### Errors & Gotchas
- Infinite loop → CPU time exceeded
- SOQL/DML inside the loop
- Wrong exit condition → off-by-one bugs

### Limits
- No fixed platform-level iteration cap
- An unbounded loop will eventually hit the CPU limit
- A CPU exception can't be caught to recover normal execution — the transaction rolls back
- Querying inside the loop condition consumes a query each pass

### Best Practice
| Need | Use |
|---|---|
| Bounded condition-driven loop | `while` |
| Safety counter | Data-driven loops |
| Large/unbounded work | Queueable / Batch |

**Why?** Unknown iteration counts still need bounded, safe termination.

---

## 13. Do-While Loop

Bottom-tested loop — the body executes at least once.

### 🌱 Simple
In `do...while`, the body executes first, then the condition is checked.
```apex
do {
    ...
} while (condition);
```

**Important:** the body runs at least once, even if the condition is initially false.

This is a comparatively rare loop construct in Apex. Most cases are better served by:
- Known collection → `for`
- Condition-first → `while`

Same governor rules apply — avoid SOQL/DML inside the loop. Large iterative workloads are risky as a long synchronous `do/while`; consider Queueable or Batch Apex instead.

### Architect Rule
Reserve `do/while` for cases where the first iteration must execute unconditionally. Otherwise `for`/`while` are usually clearer.

### While vs Do-While

| | `while` | `do/while` |
|---|---|---|
| Condition check | Before body | After body |
| Body runs if condition initially false | Zero times | At least once |

### Errors & Gotchas
- Body executes even if the initial condition is false
- Condition never becoming false → infinite loop
- SOQL/DML inside the loop

### Limits
- Body always executes at least once
- Same CPU/governor exposure as `while`
- No fixed iteration cap
- Comparatively rare in Apex; `for` is usually clearer/safer for collection processing

### Best Practice
Use `do/while` only when the first iteration must run unconditionally. Otherwise prefer `for`/`while`.

**Why?** "Run at least once" is exactly what `do/while` is for, but overusing it makes code less familiar to most readers.

---

## 14. Master Reference — Requirement → Best Choice

| Requirement | Best Choice |
|---|---|
| Ordered values | List |
| Duplicate values allowed | List |
| Index required | List |
| Unique values | Set |
| Fast membership check | Set |
| Key → Value lookup | Map |
| Parent → children grouping | `Map<Id, List<X>>` |
| Query results by Id | `Map<Id, SObject>` |
| Fixed set of code-owned states | Enum |
| Many discrete branches | Switch |
| Range-based condition | If/Else |
| Simple value selection | Ternary |
| Known collection iteration | for-each |
| Large SOQL result set | SOQL for-loop |
| Unknown iteration count | while |
| First iteration guaranteed | do/while |
| Large/unbounded processing | Queueable / Batch |
| Money calculation | Decimal |
| Large integer/count | Long |
| Integration record ID | 18-char Id |
| Null-safe relationship access | `?.` |

---

## 15. Concept Map

```
Apex Fundamentals
│
├── Primitives
│   ├── Integer / Long
│   ├── Decimal / Double
│   ├── String / Boolean
│   ├── Date / Datetime / Time
│   ├── Id
│   └── Blob
│
├── sObject
│   ├── Typed
│   ├── Generic
│   ├── get()/put()
│   ├── Relationships
│   └── Queried fields
│
├── Enum
│   └── Fixed code-owned values
│
├── Collections
│   ├── List → Order / duplicates / index
│   ├── Set  → Unique / membership
│   └── Map  → Key → Value / lookup
│
└── Control Flow
    ├── If/Else → conditions/ranges
    ├── Switch  → discrete values
    ├── For     → known iteration
    ├── While   → condition-driven
    └── Do-While → at least once
```
