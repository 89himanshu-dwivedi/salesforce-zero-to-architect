[Home](../index.md) / [22 · Limits & Alternate Solutions](index.md) / **Workaround Pattern Library**

# Workaround Pattern Library

8 topics · Series 22: Limits & Alternate Solutions

**Topics on this page**

- [Async Offload Pattern](#async-offload-pattern)
- [Chunking & Batching Pattern](#chunking-and-batching-pattern)
- [Caching with Platform Cache](#caching-with-platform-cache)
- [Skinny Table & Big Object Pattern](#skinny-table-and-big-object-pattern)
- [Bulkification Pattern](#bulkification-pattern)
- [Recursion Guard Pattern](#recursion-guard-pattern)
- [Selective Index Pattern](#selective-index-pattern)
- [External Compute Offload](#external-compute-offload)

## Async Offload Pattern

*Move heavy work out of the user transaction to beat limits.*

### 🌱 Simple

*Beginner - plain language*

The single most useful workaround: **don't do heavy work synchronously**. Offload it to async (Queueable/Batch/@future/Platform Events) so the user's save is fast *and* you get bigger limits (60s CPU, 12MB heap, fresh SOQL/DML budgets).

### 📏 Limits

*Governor & platform limits*

- Async limits: 60s CPU, 12 MB heap, 200 SOQL.
- Async daily execution allocation applies.
- 1 Queueable enqueue from within a Queueable.
- No ordering guarantee between independent async jobs.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Chunking & Batching Pattern

*Process volume in slices so each stays under limits.*

### 🌱 Simple

*Beginner - plain language*

When data is too big for one transaction, **process it in chunks** — Batch Apex (200/scope by default) or manual slicing. Each chunk gets fresh governor limits, so millions of records process safely.

### 📏 Limits

*Governor & platform limits*

- Batch scope max 2,000, default 200.
- 5 concurrent batch jobs; Flex Queue 100.
- Each chunk consumes an async execution.
- Only one chunk runs in a test context.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Caching with Platform Cache

*Cache hot, expensive data to cut queries & speed pages.*

### 🌱 Simple

*Beginner - plain language*

**Platform Cache** (Org and Session partitions) stores frequently-used, expensive-to-compute data in memory so you don't re-query it every time — cutting SOQL usage and speeding pages. Great for config, reference data, and aggregates.

### 📏 Limits

*Governor & platform limits*

- Org cache default TTL 24h (max 48h); session cache 8h.
- Max 100 KB per org-cache item.
- Capacity is purchased and entries can be evicted early.
- Cached data bypasses sharing - never cache row-level restricted data in org cache.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Skinny Table & Big Object Pattern

*Read fast on huge data via skinny tables / archive to Big Objects.*

### 🌱 Simple

*Beginner - plain language*

For read performance on large objects, Salesforce can create a **skinny table** (a copy of hot fields, kept in sync, that speeds queries/reports). For cold data, **archive to Big Objects**. Together they keep large-volume orgs fast.

### 📏 Limits

*Governor & platform limits*

- Skinny tables: max 10 fields (some sources 100 in specific cases); no formula fields; Support required.
- Big Object index is immutable; queries must use index fields left to right.
- Big Objects support no triggers or standard reports.
- Both require deliberate design up front.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Bulkification Pattern

*The universal fix: handle collections, never one record at a time.*

### 🌱 Simple

*Beginner - plain language*

**Bulkification** is the foundational pattern behind most Apex limit fixes: write code that processes **collections of records**, with **queries and DML outside loops**. Triggers get up to 200 records per invocation — your code must handle all of them in one pass.

### 📏 Limits

*Governor & platform limits*

- 100 SOQL / 150 DML statements / 10,000 DML rows per transaction.
- Triggers fire in chunks of 200.
- Limits are shared across all automation in the transaction.
- Heap often fails before row limits on wide objects.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Recursion Guard Pattern

*Stop trigger/automation re-entry from multiplying work.*

### 🌱 Simple

*Beginner - plain language*

When automation updates the same object, it can **re-fire itself**, multiplying CPU/SOQL/DML and risking infinite loops. A **static recursion guard** ensures the logic runs only once per transaction (or once per record).

### 📏 Limits

*Governor & platform limits*

- Max stack depth 16.
- Statics are transaction-scoped.
- Workflow field updates re-fire triggers once.
- Before-save flows run before before-triggers.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Selective Index Pattern

*Make any large-object query fast with the right index.*

### 🌱 Simple

*Beginner - plain language*

The cure for non-selective query errors and slow reports is the **selective index pattern**: filter on an **indexed field** returning a small fraction of rows, adding a **custom or composite index** when needed, and verifying with the **Query Plan** tool.

### 📏 Limits

*Governor & platform limits*

- ~10% of first 1M rows, 5% thereafter, capped at 333,333.
- Custom and two-column indexes require Support.
- Nulls, negations and leading wildcards defeat indexes.
- Formula fields are generally not indexable.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## External Compute Offload

*When the platform isn't the right tool, compute elsewhere.*

### 🌱 Simple

*Beginner - plain language*

Some workloads simply don't belong in Salesforce limits — massive aggregation, heavy transformation, ML, or huge file processing. The pattern: **offload to an external service** (Heroku, AWS Lambda, a data lake, CRM Analytics/Data Cloud) and integrate the results back.

### 📏 Limits

*Governor & platform limits*

- Callout limits still apply to the request and response.
- 6 MB / 12 MB payload caps - use storage links for large data.
- External systems add operational and compliance surface.
- Latency and availability become your dependency.

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
