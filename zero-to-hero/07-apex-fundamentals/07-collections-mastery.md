[Home](../index.md) / [07 · Apex Fundamentals](index.md) / **Collections Mastery**

# Collections Mastery

5 topics · Series 7: Apex Fundamentals

**Topics on this page**

- [List add/remove/contains/clone](#list-add-remove-contains-clone)
- [Set Methods](#set-methods)
- [Map Methods](#map-methods)
- [Map<String,List<Account>>](#map-string-list-account)
- [Map<Id,Set<Id>>](#map-id-set-id)

## List add/remove/contains/clone

*Mastering List methods and their performance characteristics.*

### 🌱 Simple

*Beginner - plain language*

Core List methods: `add(v)`, `add(i,v)`, `remove(i)`, `contains(v)`, `indexOf(v)`, `set(i,v)`, `clone()`, `sort()`, `clear()`. They build and manipulate ordered collections.

### 📏 Limits

*Governor & platform limits*

- `contains()` is O(n) - use a `Set` for membership tests inside loops.
- `clone()` is shallow; nested sObjects and collections are shared.
- `deepClone()` exists only on sObjects, not on Lists of custom objects.
- Removing while iterating throws or skips elements - iterate a copy.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Set Methods

*Set operations for uniqueness, membership, and set algebra.*

### 🌱 Simple

*Beginner - plain language*

Core Set methods: `add`, `addAll`, `contains`, `remove`, `removeAll`, `retainAll`, `size`, `isEmpty`, `clear`. Sets keep unique values with O(1) operations.

### 📏 Limits

*Governor & platform limits*

- Iteration order is not guaranteed and must never be relied on.
- Custom classes need `equals()` and `hashCode()` or duplicates are stored.
- `retainAll`/`removeAll` allocate new internal structures - watch heap on large sets.
- Bounded by heap, not by a documented element count.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Map Methods

*Map operations for keyed access, grouping, and bulk patterns.*

### 🌱 Simple

*Beginner - plain language*

Core Map methods: `put(k,v)`, `get(k)`, `containsKey(k)`, `remove(k)`, `keySet()`, `values()`, `size()`, `isEmpty()`, `putAll()`. They give O(1) key→value access.

### 📏 Limits

*Governor & platform limits*

- `get()` on a missing key returns null - use `containsKey()` to distinguish from a stored null.
- `keySet()` and `values()` return live views; modifying while iterating is unsafe.
- `putAll()` on a large map can double peak heap momentarily.
- Key types are restricted to primitives, sObjects, enums and classes with equals/hashCode.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Map<String,List<Account>>

*Grouping records by a key — the canonical one-to-many in-memory structure.*

### 🌱 Simple

*Beginner - plain language*

`Map<String, List<Account>>` groups accounts under a key (e.g., by Industry): each key maps to a **list** of matching records. Perfect for "group records by X" in memory.

### 📏 Limits

*Governor & platform limits*

- Nested collections multiply heap - a map of 10,000 keys each holding 20 Accounts is often the cause of a 6 MB failure.
- String keys are case-insensitive in Apex maps, which can silently merge entries.
- Build inside a SOQL for-loop to avoid materialising the full result set first.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Map<Id,Set<Id>>

*Mapping a key to a set of unique related ids — many-to-many in memory.*

### 🌱 Simple

*Beginner - plain language*

`Map<Id, Set<Id>>` maps each id to a **set of unique related ids** — e.g., each Account → the Set of its related Contact ids. Great for de-duplicated relationship tracking.

### 📏 Limits

*Governor & platform limits*

- Sets inside maps carry per-entry hash overhead - measure heap on large graphs.
- Id keys are safest as 18-character; mixing 15 and 18 creates duplicate entries.
- Deeply nested structures are expensive to serialise for Queueable state - prefer a cursor.

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
