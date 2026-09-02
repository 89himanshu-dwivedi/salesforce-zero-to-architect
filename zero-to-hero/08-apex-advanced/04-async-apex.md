[Home](../index.md) / [08 · Apex Advanced](index.md) / **Async Apex**

# Async Apex

8 topics · Series 8: Apex Advanced

**Topics on this page**

- [Future Methods](#future-methods)
- [Callouts in Future](#callouts-in-future)
- [Queueable Apex](#queueable-apex)
- [Queueable Chaining](#queueable-chaining)
- [Batch start() execute() finish()](#batch-start-execute-finish)
- [Database.Stateful](#database-stateful)
- [Schedulable Apex](#schedulable-apex)
- [Transaction Finalizers](#transaction-finalizers)

## Future Methods

*Fire-and-forget async methods for offloading work to a separate transaction.*

### 🌱 Simple

*Beginner - plain language*

**Future methods** (`@future`) run **asynchronously** in a separate transaction with fresh limits. Use them to offload work (like callouts) from a synchronous context: `@future static void doWork(Set<Id> ids) {...}`.

### 📏 Limits

*Governor & platform limits*

- 50 per transaction; shares the async daily allocation (250,000 or 200 x licences).
- Primitive parameters only; must be `static void`.
- Cannot be called from a future, batch or Queueable.
- No job Id, no ordering guarantee, no retry mechanism.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Callouts in Future

*Making HTTP callouts asynchronously to satisfy the no-callout-after-DML rule.*

### 🌱 Simple

*Beginner - plain language*

`@future(callout=true)` lets a future method make **HTTP callouts** asynchronously — useful because you **can't call out after DML** in the same synchronous transaction.

### 📏 Limits

*Governor & platform limits*

- Requires `@future(callout=true)`.
- 100 callouts and 120s cumulative callout time still apply inside the future.
- A failed callout leaves no trace beyond the Apex exception email.
- Queueable with `Database.AllowsCallouts` plus a Finalizer is strictly better.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Queueable Apex

*The modern async framework supporting object state, chaining, and monitoring.*

### 🌱 Simple

*Beginner - plain language*

**Queueable Apex** is the modern async option: implement `Queueable` with an `execute(QueueableContext)` method and enqueue with `System.enqueueJob(job)`. It returns a **job Id** you can monitor.

### 📏 Limits

*Governor & platform limits*

- 50 enqueues per synchronous transaction; only 1 from within a running Queueable.
- Max 100 jobs in Holding status.
- Member variables are serialised - non-serialisable types throw at enqueue time.
- Counts toward the shared async Apex daily allocation.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Queueable Chaining

*Sequencing async jobs by enqueuing the next from execute().*

### 🌱 Simple

*Beginner - plain language*

**Queueable chaining** means a job enqueues the **next** job from its `execute()` method, creating a sequence of async transactions — each with fresh governor limits.

### 📏 Limits

*Governor & platform limits*

- Chain depth unlimited in production but capped at 5 in Developer and Trial editions.
- Only one job may be enqueued per Queueable execution.
- Chained jobs do not chain further inside a test context.
- Every link consumes an async execution from the daily allocation.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Batch start() execute() finish()

*Batch Apex's three-phase lifecycle for processing large data volumes.*

### 🌱 Simple

*Beginner - plain language*

**Batch Apex** processes large datasets in chunks via three methods: `start()` (returns the records), `execute()` (processes each chunk), and `finish()` (post-processing). Run with `Database.executeBatch(job, scopeSize)`.

### 📏 Limits

*Governor & platform limits*

- `start()` QueryLocator supports up to 50 million rows; a custom Iterable does not.
- Scope max 2,000, default 200; 5 concurrent batch jobs, Flex Queue holds 100.
- Each `execute()` chunk is its own transaction with fresh limits and its own async execution.
- Only one chunk runs between `Test.startTest()` and `stopTest()`.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Database.Stateful

*Preserving instance state across Batch Apex chunks.*

### 🌱 Simple

*Beginner - plain language*

By default, Batch Apex **resets member variables** between chunks. Implementing `Database.Stateful` makes instance state **persist** across `execute()` calls — for running totals, counters, or collected errors.

### 📏 Limits

*Governor & platform limits*

- State persists across chunks and counts toward heap in every chunk - keep it small.
- Only instance member variables are preserved; statics are not.
- Large stateful collections are a common cause of late-running batch failures.
- Prefer a cursor or a persisted counter object over accumulating data in memory.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Schedulable Apex

*Running Apex on a schedule via the System.schedule cron.*

### 🌱 Simple

*Beginner - plain language*

**Schedulable Apex** runs code on a schedule. Implement `Schedulable` with `execute(SchedulableContext)` and schedule it via `System.schedule(name, cron, job)` or the Setup UI.

### 📏 Limits

*Governor & platform limits*

- Maximum 100 scheduled Apex jobs per org.
- Standard scheduling granularity is hourly; sub-hourly needs stacked jobs.
- The job runs as the scheduling user - it stops if that user is deactivated.
- Deploying a scheduled class may require unscheduling it first.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Transaction Finalizers

*Attaching post-completion logic to a Queueable, even after failure.*

### 🌱 Simple

*Beginner - plain language*

**Transaction Finalizers** let you run logic **after a Queueable job completes** — whether it succeeded or failed. Implement `Finalizer` and attach it with `System.attachFinalizer()` in the Queueable.

### 📏 Limits

*Governor & platform limits*

- Only one Finalizer per Queueable, attached with `System.attachFinalizer`.
- A Finalizer may enqueue at most one job.
- Runs even after uncatchable governor exceptions - the only reliable failure hook.
- Available for Queueable only, not for future, batch or scheduled Apex.

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
