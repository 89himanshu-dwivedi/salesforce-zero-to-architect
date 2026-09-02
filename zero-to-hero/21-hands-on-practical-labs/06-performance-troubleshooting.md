[Home](../index.md) / [21 · Hands-On Practical Labs](index.md) / **Performance Troubleshooting**

# Performance Troubleshooting

7 topics · Series 21: Hands-On Practical Labs

**Topics on this page**

- [Why Org Is Slow](#why-org-is-slow)
- [Why LWC Is Slow](#why-lwc-is-slow)
- [Large Data Volume Errors](#large-data-volume-errors)
- [Non-Selective SOQL](#non-selective-soql)
- [Apex CPU Timeout](#apex-cpu-timeout)
- [Heap Size Errors](#heap-size-errors)
- [Slow List Views & Reports](#slow-list-views-and-reports)

## Why Org Is Slow

*Diagnose a sluggish org: automation, queries, data volume, UI.*

### 🌱 Simple

*Beginner - plain language*

A "slow org" is almost never the platform — it's **something you built** piling up work on every save or page load. The usual culprits: too much **automation** firing on save, **unselective SOQL** over large tables, **too many components** on a page, and **data skew**. You diagnose with real evidence, not guesses.

### 📏 Limits

*Governor & platform limits*

- CPU time **10s** sync / **60s** async - the most common hard wall.
- Concurrent long-running requests (>5s): **10** per org. Exceeding it queues everyone.
- Query timeout ~120s for SOQL; reports have their own timeout.
- Group membership locks during sharing recalculation are org-wide.
- Lightning Usage App data is aggregated daily - it is not real-time diagnostics.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Why LWC Is Slow

*Fix laggy components: too many server calls, big DOM, no caching.*

### 🌱 Simple

*Beginner - plain language*

A slow LWC usually means: **too many server round-trips**, **fetching too much data**, a **huge DOM** (thousands of rows), heavy work on every render, or **no caching**. The browser DevTools Performance/Network tabs tell you which.

### 📏 Limits

*Governor & platform limits*

- `lightning-datatable` practical ceiling is a few thousand rows before the browser struggles.
- Apex response is bounded by the transaction heap (6 MB sync).
- Each imperative call is a separate server request counting toward org request limits.
- Shadow DOM adds per-component overhead - hundreds of tiny components is slower than a few well-sized ones.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Large Data Volume Errors

*What breaks at millions of rows (LDV) and how to fix each.*

### 🌱 Simple

*Beginner - plain language*

**Large Data Volumes (LDV)** = objects with millions of records. At that scale things that worked in a demo start failing: queries time out, locks collide, sharing recalcs crawl, and you hit row limits. The fixes are **indexing, selective queries, skew avoidance, and async processing**.

### 📏 Limits

*Governor & platform limits*

- **50,000** query rows per transaction (batch `start()` exempt, up to 50M).
- Selectivity thresholds: ~**10%** of first 1M rows, **5%** thereafter, max **333,333**.
- Data skew thresholds: >**10,000** children per parent, >**10,000** records per owner.
- Heap **6 MB** sync / **12 MB** async.
- Skinny tables and custom indexes require a Salesforce Support case.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Non-Selective SOQL

*The most common cause of slow/failing queries — and how to make them selective.*

### 🌱 Simple

*Beginner - plain language*

A query is **selective** when its `WHERE` clause uses an **indexed field** that returns a small fraction of the table. On large objects, a **non-selective** query (no usable index, or matching too many rows) is slow — and Salesforce will **auto-abort** it with a "non-selective query" error.

### 📏 Limits

*Governor & platform limits*

- Threshold: **200,000** rows (**100,000** in trigger context) before selectivity is enforced.
- Custom indexes require Salesforce Support.
- Index is skipped for leading wildcards, `!=`, `NOT IN`, null filters, and most formula fields.
- Two-column custom indexes exist but must be requested.
- `Database.getQueryLocator` in batch is exempt from the 50k row limit but still benefits from selectivity.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Apex CPU Timeout

*Fix 'Apex CPU time limit exceeded' — too much in-memory compute.*

### 🌱 Simple

*Beginner - plain language*

The **CPU time limit** (10s sync / 60s async) counts **computation** — loops, processing, automation — *not* waiting on the database or callouts. Hitting it means your transaction does too much work, usually **nested loops**, heavy automation stacking, or processing huge collections.

### 📏 Limits

*Governor & platform limits*

- CPU time: **10,000 ms** sync, **60,000 ms** async.
- Shared across all Apex, Flow, workflow, validation rules and roll-ups in the transaction.
- Excludes SOQL/DML database time and callout wait time.
- Not catchable in a way you can recover from - `LimitException` rolls the transaction back.
- Batch chunk size directly controls how much work lands in one CPU budget.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Heap Size Errors

*Fix 'Apex heap size too large' — too much held in memory at once.*

### 🌱 Simple

*Beginner - plain language*

The **heap** is the memory your transaction uses (6MB sync / 12MB async). **"Apex heap size too large"** means you're holding too much in memory — usually **querying too many rows/fields** into a big collection, or building giant strings/JSON.

### 📏 Limits

*Governor & platform limits*

- Heap: **6 MB** synchronous, **12 MB** asynchronous.
- SOQL for-loops retrieve in chunks of **200** sObjects.
- Base64 encoding inflates binary data by ~33%.
- `Database.Stateful` instance state persists across batch chunks and counts every time.
- Strings are immutable - concatenation in a loop allocates a new string each iteration.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Slow List Views & Reports

*Speed up sluggish list views, reports, and dashboards.*

### 🌱 Simple

*Beginner - plain language*

Slow **list views, reports, and dashboards** almost always come down to **non-selective filters** over large objects, **no date bounds**, too many **cross-object/formula columns**, or running heavy analytics on the live transactional object. The fix is selectivity, scoping, and offloading.

### 📏 Limits

*Governor & platform limits*

- Reports display **2,000** rows; exports up to **50,000** (Formatted) / higher for details-only.
- Report types can span at most **4** objects.
- Dashboards: **20** components each; dynamic dashboards limited per edition (3/5/10).
- List views display up to **2,000** records.
- Reports have their own execution timeout, separate from Apex.

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
