[Home](../index.md) / [15 · Performance Optimization](index.md) / **Query Optimization**

# Query Optimization

6 topics · Series 15: Performance Optimization

**Topics on this page**

- [Query Selectivity](#query-selectivity)
- [Query Plan Tool](#query-plan-tool)
- [Cost Analysis](#cost-analysis)
- [Standard Index](#standard-index)
- [Custom Index](#custom-index)
- [External Id Index](#external-id-index)

## Query Selectivity

*Filtering on indexed fields that match few rows.*

### 🌱 Simple

*Beginner - plain language*

A query is **selective** when its WHERE filter uses an **indexed field** that returns a small fraction of the table's rows — letting the database use the index instead of a slow full scan.

### 📏 Limits

*Governor & platform limits*

- ~10% of the first million rows, 5% thereafter, capped at 333,333.
- Enforcement begins above 200,000 rows (100,000 in trigger context).
- Leading wildcards, negations and null filters defeat indexes.
- Formula fields are generally not indexable.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Query Plan Tool

*Developer Console tool showing query cost and indexes.*

### 🌱 Simple

*Beginner - plain language*

The **Query Plan tool** (Developer Console, with "Query Plan" enabled) shows how Salesforce will execute a SOQL query — including the **cost**, which index (if any) it uses, and rows scanned.

### 📏 Limits

*Governor & platform limits*

- Available only in the Developer Console with the feature enabled in preferences.
- Cost is an estimate based on statistics, not a guarantee.
- Does not evaluate sharing-related row filtering cost.
- Not available for SOSL or reports.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Cost Analysis

*Interpreting query cost to choose the best plan.*

### 🌱 Simple

*Beginner - plain language*

**Cost analysis** is interpreting the optimizer's **relative cost** numbers to judge whether a query is efficient — comparing index plans vs table scans and tuning filters to lower the cost.

### 📏 Limits

*Governor & platform limits*

- Cost below 1.0 indicates an index will be used.
- Statistics can be stale immediately after a large data load.
- Sharing evaluation cost is not reflected in the plan.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Standard Index

*Auto-indexed fields: Id, Name, lookups, audit dates.*

### 🌱 Simple

*Beginner - plain language*

**Standard indexes** are created automatically by Salesforce on certain fields — **Id, Name, OwnerId, lookup/master-detail relationships, CreatedDate, SystemModstamp**, and RecordType — so filtering on them can be selective without extra setup.

### 📏 Limits

*Governor & platform limits*

- Covers Id, Name, OwnerId, CreatedDate, SystemModstamp, RecordTypeId, lookups and master-detail.
- Not used for leading-wildcard, negation or null filters.
- Cannot be disabled or tuned.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Custom Index

*Salesforce-created index on a field you filter often.*

### 🌱 Simple

*Beginner - plain language*

A **custom index** is an index Salesforce creates (via a support request) on a field you frequently filter but that isn't standard-indexed — making those queries selective on large objects.

### 📏 Limits

*Governor & platform limits*

- Requires a Salesforce Support case; not self-service.
- Skewed data distributions may not benefit - the optimiser still decides.
- Two-column indexes exist but must be requested explicitly.
- Nulls are excluded unless the index is built to include them.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## External Id Index

*External Id fields are indexed and enable upsert.*

### 🌱 Simple

*Beginner - plain language*

An **External Id** field stores a record's identifier from an outside system. Marking a field as External Id **automatically creates a custom index** and lets you **upsert** on it — matching records without storing Salesforce Ids externally.

### 📏 Limits

*Governor & platform limits*

- Automatically indexed and case-insensitive by default.
- Maximum 7 External Id fields per object (3 on some editions).
- Upsert on a non-unique External Id throws when multiple records match.

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
