[Home](../index.md) / [05 · Developer Foundations](index.md) / **Platform Fundamentals**

# Platform Fundamentals

9 topics · Series 5: Developer Foundations

**Topics on this page**

- [Transaction Boundary](#transaction-boundary)
- [Commit](#commit)
- [Rollback](#rollback)
- [Order of Execution](#order-of-execution)
- [SOQL Limit](#soql-limit)
- [DML Limit](#dml-limit)
- [Heap Size](#heap-size)
- [CPU Time](#cpu-time)
- [Callout Limit](#callout-limit)

## Transaction Boundary

*The unit of work within which all DML is atomic and all governor limits are counted.*

### 🌱 Simple

*Beginner - plain language*

A **transaction** is one execution context — everything that happens from a single trigger/API call/flow run until it finishes. All database changes in it either **all commit** or **all roll back** together, and governor limits are counted per transaction.

### 📏 Limits

*Governor & platform limits*

- One governor-limit budget per transaction.
- Async = new transaction = fresh limits.
- Callouts forbidden after uncommitted DML.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Commit

*The point where the transaction's database changes become permanent.*

### 🌱 Simple

*Beginner - plain language*

A **commit** is when all the changes made during a transaction are **saved permanently** to the database. In Salesforce this happens **automatically** at the successful end of a transaction — there's no explicit "COMMIT" you call.

### 📏 Limits

*Governor & platform limits*

- No manual commit/savepoint-free control over commit timing.
- Callouts blocked with pending DML.
- Post-commit work runs in separate contexts.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Rollback

*Undoing a transaction's changes — automatic on error, or manual via savepoints.*

### 🌱 Simple

*Beginner - plain language*

A **rollback** undoes the database changes of a transaction. If an unhandled exception occurs, Salesforce **automatically rolls back everything** in that transaction. You can also roll back to a **savepoint** manually.

### 📏 Limits

*Governor & platform limits*

- Savepoints count as DML; only latest can be released.
- Caught exceptions don't auto-rollback.
- Partial-success DML returns results instead of rolling back.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Order of Execution

*The fixed sequence Salesforce follows when a record is saved — the master timeline.*

### 🌱 Simple

*Beginner - plain language*

The **Order of Execution** is the defined sequence of everything that happens when a record is saved: validation, before triggers, after triggers, flows, workflow, rollups, commit. Knowing it explains *why* automations fire in a certain order.

### 📏 Limits

*Governor & platform limits*

- All steps share the transaction's governor limits.
- Re-entrancy from field updates.
- Order is fixed — you design around it, not change it.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## SOQL Limit

*The cap on database queries per transaction — 100 synchronous, 200 async.*

### 🌱 Simple

*Beginner - plain language*

The **SOQL limit** caps how many queries you can run in one transaction: **100** in synchronous context, **200** in asynchronous. Hit it and you get "Too many SOQL queries: 101".

### 📏 Limits

*Governor & platform limits*

- **100** SOQL (sync) / **200** (async) per transaction.
- **50,000** rows retrieved per transaction.
- Cumulative across all code/automation.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## DML Limit

*The cap on database write statements (150) and rows (10,000) per transaction.*

### 🌱 Simple

*Beginner - plain language*

The **DML limit** caps database writes: **150 DML statements** per transaction and **10,000 rows** processed across them. "Too many DML statements: 151" means too many separate insert/update/delete calls.

### 📏 Limits

*Governor & platform limits*

- **150** DML statements per transaction.
- **10,000** rows across all DML.
- Savepoints count as statements; cumulative across automation.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Heap Size

*The in-memory limit for data your code holds — 6 MB sync, 12 MB async.*

### 🌱 Simple

*Beginner - plain language*

**Heap size** is how much data your code can hold in memory at once: **6 MB** synchronous, **12 MB** asynchronous. Loading too many records/large strings into memory causes "Apex heap size too large".

### 📏 Limits

*Governor & platform limits*

- **6 MB** sync / **12 MB** async heap.
- Blobs and big collections are heavy.
- Counts all live references.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## CPU Time

*The compute-time budget per transaction — 10s sync, 60s async.*

### 🌱 Simple

*Beginner - plain language*

**CPU time** limits how long your code's *computation* can run in one transaction: **10,000 ms (10s)** synchronous, **60,000 ms (60s)** asynchronous. "Apex CPU time limit exceeded" means too much in-memory processing.

### 📏 Limits

*Governor & platform limits*

- **10s** sync / **60s** async CPU.
- Excludes DB/callout wait time.
- Cumulative across all automation.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Callout Limit

*Caps on external HTTP callouts — 100 per transaction, 120s total timeout.*

### 🌱 Simple

*Beginner - plain language*

The **callout limit** caps external web service calls: **100 callouts** per transaction, each with a configurable timeout, and a **cumulative 120-second** callout time budget. You also can't make a callout after uncommitted DML.

### 📏 Limits

*Governor & platform limits*

- **100** callouts/transaction; **120s** cumulative timeout.
- No callouts after uncommitted DML.
- Per-callout timeout configurable.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

---

## Connect

These pages carry the **definitions and limits** only. The advanced depth, real-world
scenarios, error playbooks, best-option reasoning and interview questions are kept aside.

If you would like them, or you want to talk about anything on this page:

- **LinkedIn** - [in/himanshukumar-sf](https://www.linkedin.com/in/himanshukumar-sf/)
- **X** - [@kum60094](https://x.com/kum60094)
- **GitHub** - [89himanshu-dwivedi](https://github.com/89himanshu-dwivedi)
- **Email** - [himanshu.jee.1996@gmail.com](mailto:himanshu.jee.1996@gmail.com)

*- Himanshu Kumar*
