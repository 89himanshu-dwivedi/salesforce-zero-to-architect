[Home](../index.md) / [07 · Apex Fundamentals](index.md) / **Apex Basics**

# Apex Basics

13 topics · Series 7: Apex Fundamentals

**Topics on this page**

- [Variables & Constants](#variables-and-constants)
- [Operators](#operators)
- [Primitive Types](#primitive-types)
- [sObject](#sobject)
- [Enum](#enum)
- [List](#list)
- [Set](#set)
- [Map](#map)
- [If Else](#if-else)
- [Switch](#switch)
- [For Loop](#for-loop)
- [While](#while)
- [Do While](#do-while)

## Variables & Constants

*Declaring typed storage and immutable values — the foundation of Apex code.*

### 🌱 Simple

*Beginner - plain language*

A **variable** is a named, typed container: `Integer count = 0;`. A **constant** is a value that never changes, declared with `final` (and usually `static`): `static final Integer MAX = 100;`. Apex is **strongly typed** — every variable has a declared type.

### 📏 Limits

*Governor & platform limits*

- Local variable names must be unique in scope; Apex is case-insensitive so `x` and `X` collide.
- `final` makes a reference immutable, not the object it points to - a `final List` can still be added to.
- Static variables live for the transaction only and reset between async transactions and batch chunks.
- All variables count toward the 6 MB sync / 12 MB async heap.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Operators

*Arithmetic, comparison, logical, assignment, and safe-navigation operators.*

### 🌱 Simple

*Beginner - plain language*

**Operators** act on values: arithmetic (`+ - * /`), comparison (`== != < >`), logical (`&& || !`), assignment (`= += -=`), and the ternary (`cond ? a : b`). Apex also has the **safe-navigation** operator `?.` to avoid null errors.

### 📏 Limits

*Governor & platform limits*

- `==` on sObjects compares field-by-field, not reference - unlike Java.
- String comparison with `==` is case-insensitive; use `equals()` for case-sensitive.
- Integer division truncates; use Decimal or Double for fractional results.
- `&&` and `||` short-circuit, so side effects on the right may not run.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Primitive Types

*The built-in scalar types: Integer, Long, Decimal, Double, Boolean, String, Date, Datetime, Id, Blob.*

### 🌱 Simple

*Beginner - plain language*

Apex **primitives** are the basic value types: `Integer`, `Long`, `Decimal`, `Double`, `Boolean`, `String`, `Date`, `Datetime`, `Time`, `Id`, and `Blob`. Each holds a single value (vs collections/objects).

### 📏 Limits

*Governor & platform limits*

- Integer is 32-bit (±2,147,483,647); use Long beyond that.
- Decimal keeps scale and is the correct type for currency - Double introduces floating-point error.
- String has no documented hard cap in memory but long text fields cap at 131,072 characters.
- Blob is limited by heap; a 4 MB file base64-encoded is ~5.5 MB.
- Id is 15-char case-sensitive or 18-char case-insensitive - mixing them causes silent mismatches.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## sObject

*The typed in-memory representation of a Salesforce record.*

### 🌱 Simple

*Beginner - plain language*

An **sObject** is an in-memory record — e.g., `Account a = new Account(Name='Acme');`. It maps to a database object and its fields. You read/write fields (`a.Name`) and pass it to DML to persist.

### 📏 Limits

*Governor & platform limits*

- Fields not included in the SELECT are null, with no "not loaded" error.
- Assigning a query directly to an sObject throws `QueryException` on 0 or >1 rows.
- sObjects are heap-heavy - query only the fields you use.
- Serialising an sObject includes the `attributes` node; use a DTO for integrations.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Enum

*A fixed set of named constants for type-safe, readable state.*

### 🌱 Simple

*Beginner - plain language*

An **enum** defines a fixed set of named values: `enum Season { WINTER, SPRING, SUMMER, FALL }`. Use it instead of magic strings/numbers for type-safe, self-documenting choices.

### 📏 Limits

*Governor & platform limits*

- Enums cannot have constructors, methods or fields as in Java.
- `valueOf()` throws if the string does not match a constant exactly (case-sensitive).
- Enum values are not deserialised automatically from arbitrary JSON strings.
- Adding a value to an enum used in a managed package is a breaking change for subscribers.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## List

*An ordered, indexed, duplicate-allowing collection — the workhorse of Apex.*

### 🌱 Simple

*Beginner - plain language*

A **List** is an ordered collection allowing duplicates, accessed by index: `List<String> names = new List<String>{'A','B'};`. It's the most common collection — query results and DML both use Lists.

### 📏 Limits

*Governor & platform limits*

- Indexed and ordered, allows duplicates; max ~1,000 elements in a literal initialiser.
- Bounded by heap, not by a documented element cap.
- DML on a List is capped at 10,000 rows per transaction.
- `clone()` is shallow - nested objects are shared.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Set

*An unordered collection of unique values with fast membership tests.*

### 🌱 Simple

*Beginner - plain language*

A **Set** holds **unique** values (no duplicates), unordered: `Set<Id> ids = new Set<Id>();`. Great for de-duplicating and fast `contains()` checks.

### 📏 Limits

*Governor & platform limits*

- Unordered and unique; iteration order is not guaranteed and must not be relied on.
- Only primitives, sObjects, enums and objects with proper `equals`/`hashCode` can be members.
- Custom Apex classes need `equals()` and `hashCode()` overridden or duplicates slip in.
- Bounded by heap.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Map

*A key→value collection for O(1) lookups — central to bulkified Apex.*

### 🌱 Simple

*Beginner - plain language*

A **Map** stores **key→value** pairs: `Map<Id, Account> byId = new Map<Id, Account>();`. Look up a value by its key in O(1) — perfect for relating records without re-querying.

### 📏 Limits

*Governor & platform limits*

- Keys must be primitives, sObjects, enums or classes with `equals`/`hashCode`.
- Costs more heap than a List - keys plus values plus hash overhead.
- `Map<Id, sObject>` built from a query is the standard bulkification tool.
- `get()` on a missing key returns null, not an exception - guard with `containsKey`.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## If Else

*Conditional branching — the basic decision structure.*

### 🌱 Simple

*Beginner - plain language*

`if/else` runs code based on a Boolean condition: `if (amount > 1000) {...} else if (amount > 0) {...} else {...}`. It's the fundamental decision-maker.

### 📏 Limits

*Governor & platform limits*

- Every branch consumes CPU time from the shared 10s sync / 60s async budget.
- Deeply nested conditionals are a common source of unreachable branches with no compiler warning.
- Null-safe comparison matters: `null == null` is true, but `null.method()` throws.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Switch

*Multi-way branching on a value — cleaner than long if-else chains.*

### 🌱 Simple

*Beginner - plain language*

`switch on x { when 1 {...} when 2,3 {...} when else {...} }` branches on a value. Apex switch works on Integer, Long, String, Enum, sObject type, and Id.

### 📏 Limits

*Governor & platform limits*

- Supports Integer, Long, String, Enum and sObject types only - no Decimal or Boolean.
- String matching is case-insensitive.
- No fall-through between `when` blocks - each is independent.
- `when else` is optional; without it an unmatched value silently does nothing.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## For Loop

*Iterating over ranges, lists, and query results — three for-loop forms.*

### 🌱 Simple

*Beginner - plain language*

Apex has three `for` loops: classic counter (`for (Integer i=0; i<n; i++)`), list iteration (`for (Account a : accts)`), and the SOQL for-loop (`for (Account a : [SELECT ...])`).

### 📏 Limits

*Governor & platform limits*

- SOQL for-loops retrieve 200 sObjects per chunk, which is the standard heap-safety mechanism.
- Never place SOQL or DML inside a loop - 100 queries / 150 DML statements per transaction.
- Nested loops are the leading cause of CPU time limit exceptions.
- Loop counters are not bounded by the platform - only by CPU and heap.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## While

*Condition-first looping — repeats while a Boolean stays true.*

### 🌱 Simple

*Beginner - plain language*

A `while` loop repeats **as long as** its condition is true, checked **before** each iteration: `while (hasMore) {...}`. If the condition starts false, the body never runs.

### 📏 Limits

*Governor & platform limits*

- No platform-level iteration cap - an unbounded loop simply hits the 10s CPU limit.
- The CPU limit exception cannot be caught and recovered from; the transaction rolls back.
- Avoid querying inside the condition - it counts against the 100-query limit each pass.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Do While

*Bottom-tested loop — always runs at least once.*

### 🌱 Simple

*Beginner - plain language*

A `do...while` runs the body **first**, then checks the condition: `do {...} while (cond);`. It always executes **at least once**, even if the condition is false.

### 📏 Limits

*Governor & platform limits*

- Body always executes at least once, even when the condition is false.
- Same CPU and governor exposure as `while` - no iteration cap.
- Rarely the right construct in Apex; a for-loop over a collection is usually clearer and safer.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

---

## Connect

These pages carry the **definitions and limits** only. The advanced depth, real-world
scenarios, error playbooks, best-option reasoning and interview questions are kept aside.

If you would like them, or you want to talk about the topics on this page, connect with me
on **LinkedIn**, **X (Twitter)** or **GitHub** - all links are on the
[home page](../index.md).

*- Himanshu Kumar*
