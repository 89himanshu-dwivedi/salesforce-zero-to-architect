[Home](../index.md) / [08 · Apex Advanced](index.md) / **Dynamic Apex**

# Dynamic Apex

5 topics · Series 8: Apex Advanced

**Topics on this page**

- [Schema Namespace](#schema-namespace)
- [Describe Calls](#describe-calls)
- [Dynamic SOQL](#dynamic-soql)
- [Dynamic SOSL](#dynamic-sosl)
- [Dynamic DML](#dynamic-dml)

## Schema Namespace

*The Schema class hierarchy for runtime metadata about objects and fields.*

### 🌱 Simple

*Beginner - plain language*

The **Schema namespace** exposes metadata at runtime — `Schema.getGlobalDescribe()`, `Schema.SObjectType`, `Schema.DescribeFieldResult` — so code can discover objects/fields dynamically instead of hard-coding them.

### 📏 Limits

*Governor & platform limits*

- 100 `describeSObjects` calls per transaction.
- `getGlobalDescribe()` is CPU-expensive on large orgs - cache it in a static.
- Describe results respect the running user's object and field access.
- Describe data counts toward heap on orgs with thousands of fields.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Describe Calls

*Runtime introspection of object/field/picklist metadata and accessibility.*

### 🌱 Simple

*Beginner - plain language*

**Describe calls** return metadata about objects and fields at runtime — labels, data types, picklist values, relationships, and whether the current user can access them (FLS/CRUD).

### 📏 Limits

*Governor & platform limits*

- 100 describe calls per transaction; each also costs measurable CPU.
- Field describes are lazily loaded but still count once triggered.
- Picklist value describes on large picklists consume noticeable heap.
- Always cache in a static rather than describing inside a loop.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Dynamic SOQL

*Building and running SOQL from strings at runtime — safely.*

### 🌱 Simple

*Beginner - plain language*

**Dynamic SOQL** builds a query string at runtime and runs it with `Database.query(str)` (or `getQueryLocator`). Used when fields/objects/filters aren't known until runtime.

### 📏 Limits

*Governor & platform limits*

- `Database.query()` counts against the 100-query and 50,000-row limits identically to static SOQL.
- String concatenation of user input causes SOQL injection - use bind variables or `String.escapeSingleQuotes()`.
- Dynamic queries cannot be validated at compile time; typos fail at runtime.
- `Database.queryWithBinds` enforces user mode and safe binding.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Dynamic SOSL

*Runtime-built SOSL searches with Search.query().*

### 🌱 Simple

*Beginner - plain language*

**Dynamic SOSL** builds a SOSL search string at runtime and runs it via `Search.query(str)` — for configurable text search across objects when the search terms/objects vary.

### 📏 Limits

*Governor & platform limits*

- 20 SOSL queries per transaction; 2,000 rows per query.
- `Search.query()` is vulnerable to SOSL injection - escape or bind user input.
- Search index lag means very recent records may not be returned.
- Results count toward the 50,000 query-row limit.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Dynamic DML

*Performing insert/update/delete on generic SObjects at runtime.*

### 🌱 Simple

*Beginner - plain language*

**Dynamic DML** operates on generic `SObject`/`List<SObject>` determined at runtime — e.g., `insert genericRecords;` — with fields set via `put()`. Used by object-agnostic frameworks.

### 📏 Limits

*Governor & platform limits*

- Generic sObject DML still counts against 150 statements / 10,000 rows.
- Field access via `put()`/`get()` fails at runtime for a wrong API name - no compile check.
- Dynamic DML bypasses no security: CRUD and FLS must still be enforced explicitly.
- Mixed DML rules apply the same way.

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
