[Home](../index.md) / [22 · Limits & Alternate Solutions](index.md) / **Apex Governor Limits**

# Apex Governor Limits

14 topics · Series 22: Limits & Alternate Solutions

**Topics on this page**

- [SOQL 100 Queries Limit](#soql-100-queries-limit)
- [Query Rows 50000 Limit](#query-rows-50000-limit)
- [DML 150 Statements Limit](#dml-150-statements-limit)
- [DML 10000 Rows Limit](#dml-10000-rows-limit)
- [CPU Time Limit](#cpu-time-limit)
- [Heap Size Limit](#heap-size-limit)
- [100 Callouts Limit](#100-callouts-limit)
- [Callout Timeout 120s](#callout-timeout-120s)
- [50 Future Calls Limit](#50-future-calls-limit)
- [Queueable Limits](#queueable-limits)
- [Email Send Limits](#email-send-limits)
- [SOSL Query Limits](#sosl-query-limits)
- [Stack Depth Limit](#stack-depth-limit)
- [describeSObjects Limit](#describesobjects-limit)

## SOQL 100 Queries Limit

*100 SOQL per sync transaction (200 async) — and the fixes.*

### 🌱 Simple

*Beginner - plain language*

Apex lets you run **100 SOQL queries** in a synchronous transaction (**200** in async). The classic way to blow this is putting a `SELECT`*inside a loop* — 250 records = 250 queries = error. The fix is almost never "raise the limit" (you can't); it's **query once, outside the loop, and use Maps**.

### 📏 Limits

*Governor & platform limits*

- 100 sync / 200 async SOQL queries per transaction.
- SOSL is a separate 20-per-transaction limit.
- Custom Metadata and Custom Setting `getInstance()` are free.
- Managed package queries count against their own limit, not yours.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Query Rows 50000 Limit

*50,000 rows retrieved per transaction — stream with Batch instead.*

### 🌱 Simple

*Beginner - plain language*

A single Apex transaction can retrieve **50,000 records total** across all SOQL. Hit it and you get `Too many query rows: 50001`. You can't raise it — you **stream** the data with **Batch Apex**, which uses a `QueryLocator` able to process up to **50 million** rows in chunks.

### 📏 Limits

*Governor & platform limits*

- 50,000 query rows per transaction (sync and async).
- Batch `start()` QueryLocator: 50 million.
- Child rows in sub-queries count toward the total.
- `COUNT()` returns 1 row regardless of matches.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## DML 150 Statements Limit

*150 DML statements per transaction — collect and DML once.*

### 🌱 Simple

*Beginner - plain language*

You get **150 DML statements** per transaction (each `insert`/`update`/`delete` call = 1, regardless of how many records). DML inside a loop burns these fast. The fix: **add records to a List and DML the List once** after the loop.

### 📏 Limits

*Governor & platform limits*

- 150 DML statements per transaction.
- 10,000 rows across all DML.
- `Database.emptyRecycleBin` and `convertLead` count as DML.
- Approval process submissions count too.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## DML 10000 Rows Limit

*10,000 rows DML'd per transaction — chunk or go async.*

### 🌱 Simple

*Beginner - plain language*

Across all DML in a transaction you can modify **10,000 records total**. One `update` of a 12,000-row list fails. Fix: **chunk the list** into ≤10k slices, or process in **Batch Apex** where each scope gets a fresh 10k allowance.

### 📏 Limits

*Governor & platform limits*

- 10,000 DML rows per transaction (sync and async).
- Cascade-deleted children count.
- Trigger chunks are 200 records - each chunk is a separate transaction with its own budget.
- Bulk API batches of 10,000 map to 50 trigger invocations.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## CPU Time Limit

*10s sync / 60s async CPU — the #1 hard-to-fix limit.*

### 🌱 Simple

*Beginner - plain language*

The CPU limit is **10 seconds (sync)** / **60 seconds (async)** of *compute* time (loops, logic, formula/automation — NOT waiting on SOQL/callouts). It's the limit you "increase your way into": more automation, nested loops, and complex Flows quietly pile on CPU until saves fail.

### 📏 Limits

*Governor & platform limits*

- 10s sync / 60s async.
- Shared across Apex, Flow, workflow, validation rules and roll-ups.
- Excludes DB and callout wait.
- `LimitException` is not recoverable - the transaction rolls back.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Heap Size Limit

*6MB sync / 12MB async memory — don't hold everything at once.*

### 🌱 Simple

*Beginner - plain language*

Heap is your transaction's **working memory: 6MB sync / 12MB async**. You blow it by holding huge collections, big query results, or large strings/blobs all at once. Fix: **process in smaller chunks, null out references, use SOQL for-loops, and don't store data you don't need**.

### 📏 Limits

*Governor & platform limits*

- 6 MB sync / 12 MB async.
- SOQL for-loops retrieve 200 sObjects at a time.
- Strings are immutable - concatenation allocates each iteration.
- Stateful batch member state counts in every chunk.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## 100 Callouts Limit

*100 callouts per transaction — batch the API, not the loop.*

### 🌱 Simple

*Beginner - plain language*

You can make **100 HTTP callouts per transaction**. Calling an API inside a loop over records hits this instantly. Fix: **send one bulk/batch request** to the external API, or move per-record callouts to **async** (Queueable per record / Batch with `Database.AllowsCallouts`).

### 📏 Limits

*Governor & platform limits*

- 100 callouts per transaction.
- 120s total callout time; max 120s per callout.
- 6 MB request/response sync, 12 MB async.
- No callouts after uncommitted DML.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Callout Timeout 120s

*Max 120s total callout time — handle slow APIs gracefully.*

### 🌱 Simple

*Beginner - plain language*

A single callout can wait up to **120 seconds**, and total callout time per transaction is also capped (~120s). A slow third-party API can blow this and fail the whole transaction. Fix: **set a sane timeout, go async, retry with backoff, or use Continuations** for long synchronous UI calls.

### 📏 Limits

*Governor & platform limits*

- Default 10s, max 120s per callout.
- 120s cumulative per transaction.
- Concurrent long-running (>5s) requests capped at 10 per org.
- Timeout does not tell you whether the remote side committed.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## 50 Future Calls Limit

*50 @future per transaction — and why Queueable is better.*

### 🌱 Simple

*Beginner - plain language*

You can invoke **50 `@future` methods per transaction**. Worse, you can't call `@future` from `@future`, can't pass objects, and can't chain or monitor them. The modern fix is almost always: **use Queueable instead** — it chains, takes object params, and is monitorable.

### 📏 Limits

*Governor & platform limits*

- 50 future calls per transaction.
- Counts toward the shared async Apex daily limit.
- Must be `static void`.
- Cannot be invoked from future, batch or Queueable contexts.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Queueable Limits

*50 enqueues sync / 1 chained; depth and stack-depth caveats.*

### 🌱 Simple

*Beginner - plain language*

You can enqueue up to **50 Queueable jobs** from a synchronous context, but only **1 child job** from inside a running Queueable (so chains are single-file). There's no hard chain-depth limit in production, but each link is a separate transaction — design for idempotency and a stop condition.

### 📏 Limits

*Governor & platform limits*

- 50 enqueues per sync transaction; 1 from within a Queueable.
- Chain depth unlimited in production, 5 in Developer/Trial.
- Max 100 jobs in Holding.
- One Finalizer per Queueable; it may enqueue one job.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Email Send Limits

*Apex email caps (10 invocations / daily limits) — bulk & async.*

### 🌱 Simple

*Beginner - plain language*

Apex limits email: a transaction can call `Messaging.sendEmail` a limited number of times, and the org has a **daily mass-email cap** (varies by edition, e.g. 5,000 external recipients/day). Sending one email per record in a loop hits both. Fix: **build one List of messages and send once**; for volume, go async and respect the daily cap.

### 📏 Limits

*Governor & platform limits*

- 5,000 external addresses/day; internal users are exempt in some contexts.
- 10 sendEmail invocations per transaction; 100 recipients per message.
- 10 MB total attachments per email.
- Rolled-back transactions send nothing.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## SOSL Query Limits

*20 SOSL/transaction, 2,000 rows per SOSL — and when to use SOQL.*

### 🌱 Simple

*Beginner - plain language*

SOSL (full-text search across objects) allows **20 SOSL queries per transaction** and returns at most **2,000 rows** per search. If you need exact-field filtering or more rows, **SOQL is the right tool**; SOSL is for fuzzy text search across many objects.

### 📏 Limits

*Governor & platform limits*

- 20 SOSL queries per transaction.
- 2,000 records per SOSL query.
- Search index lag means very recent records may be missing.
- SOSL results count toward the 50,000 row limit.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Stack Depth Limit

*Recursion/trigger re-entry blows the stack — guard it.*

### 🌱 Simple

*Beginner - plain language*

Apex limits how deep calls can nest (recursion, and trigger→update→trigger re-entry). Uncontrolled recursion causes `Maximum stack depth reached` or `Maximum trigger depth exceeded`. The fix is a **static recursion guard** so logic runs once per transaction.

### 📏 Limits

*Governor & platform limits*

- 16 levels of recursive DML.
- Statics reset between transactions and batch chunks.
- Method recursion is bounded by heap and CPU, not by this limit.
- Order across multiple triggers on one object is undefined.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## describeSObjects Limit

*100 describe calls/txn — cache schema, don't re-describe.*

### 🌱 Simple

*Beginner - plain language*

Schema describe calls (e.g. `getGlobalDescribe()`, field describes) are limited (~**100 per transaction** for some, and they're expensive). Calling them inside loops is wasteful. Fix: **describe once, cache the result in a static Map**, and reuse.

### 📏 Limits

*Governor & platform limits*

- 100 describeSObjects calls per transaction.
- Describes consume CPU time.
- getGlobalDescribe is expensive on orgs with many objects.
- Describe results are transaction-scoped when cached in statics.

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
