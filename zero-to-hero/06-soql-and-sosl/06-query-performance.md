[Home](../index.md) / [06 · SOQL & SOSL](index.md) / **Query Performance**

# Query Performance

7 topics · Series 6: SOQL & SOSL

**Topics on this page**

- [Governor Limits](#governor-limits)
- [SOQL For Loops](#soql-for-loops)
- [Bulk Queries](#bulk-queries)
- [Pagination](#pagination)
- [OFFSET Limitation](#offset-limitation)
- [Keyset Pagination](#keyset-pagination)
- [LDV Search Strategies](#ldv-search-strategies)

## Governor Limits

*The per-transaction query caps that force efficient, bulk-safe SOQL/SOSL.*

### 🌱 Simple

*Beginner - plain language*

**Governor limits** cap queries per transaction: **100 SOQL** (200 async), **50,000 rows** retrieved, **20 SOSL**, and 2,000 SOSL records. Exceed them and the transaction fails — so queries must be efficient and bulkified.

### 📏 Limits

*Governor & platform limits*

- 100/200 SOQL; 50k rows; 20 SOSL; 2,000 SOSL rows.
- Cumulative across the transaction.
- Async/Batch reset budgets.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## SOQL For Loops

*Iterating query results in chunks to keep heap low on large datasets.*

### 🌱 Simple

*Beginner - plain language*

A **SOQL for-loop** — `for (Account a : [SELECT ... ]) {...}` — processes records in **batches of 200** automatically, keeping memory (heap) low even for large result sets.

### 📏 Limits

*Governor & platform limits*

- 200-record chunks; still ≤ 50k rows total.
- List form for chunked DML.
- Beyond 50k → Batch Apex.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Bulk Queries

*Querying for whole collections at once to keep code bulk-safe.*

### 🌱 Simple

*Beginner - plain language*

**Bulk queries** retrieve data for an **entire set of records in one query** (e.g., `WHERE Id IN :ids`) instead of querying per record — the foundation of bulk-safe Apex.

### 📏 Limits

*Governor & platform limits*

- Large IN-lists can be non-selective.
- Still bound by 50k rows.
- Assume 200-record batches.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Pagination

*Returning results in pages to keep UIs responsive and within limits.*

### 🌱 Simple

*Beginner - plain language*

**Pagination** returns results a **page at a time** (e.g., 25 rows) rather than all at once — keeping list UIs fast and queries within row/heap limits.

### 📏 Limits

*Governor & platform limits*

- OFFSET capped at 2,000, slow at depth.
- Keyset needs a stable sort key.
- Exact totals costly on LDV.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## OFFSET Limitation

*Why OFFSET-based paging breaks down — the 2,000 cap and deep-page cost.*

### 🌱 Simple

*Beginner - plain language*

`OFFSET` has hard problems: it's **capped at 2,000** and gets **slower the deeper you page**, because the database still processes and throws away all the skipped rows.

### 📏 Limits

*Governor & platform limits*

- Maximum OFFSET is 2,000 - page 101 of a 20-per-page list is unreachable.
- Cost grows linearly with depth; the database still reads the skipped rows.
- Not supported in some sub-query contexts.
- Keyset pagination is the only design that stays fast at depth.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Keyset Pagination

*Efficient 'seek' paging using the last row's key — the LDV-scalable approach.*

### 🌱 Simple

*Beginner - plain language*

**Keyset pagination** (a.k.a. seek method) pages by remembering the **last row's key** and querying `WHERE Id > :lastId ORDER BY Id LIMIT n` — fast and unbounded, with no OFFSET cap.

### 📏 Limits

*Governor & platform limits*

- Sequential (no random page jump).
- Needs a unique indexed sort key.
- Composite key for non-unique sorts.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## LDV Search Strategies

*Combining indexing, selectivity, archiving, and search tools for large data volumes.*

### 🌱 Simple

*Beginner - plain language*

**LDV (Large Data Volume) search strategies** are the combined techniques for querying/searching objects with millions of rows: selective indexed filters, custom indexes, skinny tables, SOSL for text, keyset paging, archiving, and async processing.

### 📏 Limits

*Governor & platform limits*

- Selectivity thresholds + skew.
- Skinny tables/indexes via Salesforce.
- Archiving needed for true scale.

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
