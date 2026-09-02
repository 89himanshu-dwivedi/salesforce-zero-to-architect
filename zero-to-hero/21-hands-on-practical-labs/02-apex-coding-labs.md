[Home](../index.md) / [21 · Hands-On Practical Labs](index.md) / **Apex Coding Labs**

# Apex Coding Labs

7 topics · Series 21: Hands-On Practical Labs

**Topics on this page**

- [Trigger + Handler](#trigger-plus-handler)
- [Service Class Pattern](#service-class-pattern)
- [Batch Apex Job](#batch-apex-job)
- [Queueable Chain](#queueable-chain)
- [Future Callout](#future-callout)
- [Test Class Writing](#test-class-writing)
- [Bulkified DML](#bulkified-dml)

## Trigger + Handler

*The correct one-trigger-per-object + handler class pattern with real code.*

### 🌱 Simple

*Beginner - plain language*

A **trigger** is Apex that runs automatically when records are inserted/updated/deleted. The mistake beginners make is writing all logic *inside* the trigger. The professional pattern: keep the trigger tiny (just routing) and put logic in a separate **handler class** — easy to test, reuse, and control.

### 📏 Limits

*Governor & platform limits*

- Trigger context is bulk: up to **200 records** per invocation, and the whole transaction shares 100 SOQL / 150 DML.
- Max trigger/stack depth **16** for recursive DML.
- `Trigger.old` is not available on insert; `Trigger.new` is not available on delete.
- Governor limits are per transaction, not per trigger - three "cheap" triggers can still blow the limit together.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Service Class Pattern

*Put reusable business logic in a service layer, not in triggers/controllers.*

### 🌱 Simple

*Beginner - plain language*

A **service class** holds your real business logic (e.g., "close an opportunity and create an order") so it can be called from a trigger, an LWC controller, a batch job, or a Flow — **without duplicating code**. Triggers and controllers stay thin; the service does the work.

### 📏 Limits

*Governor & platform limits*

- Static-only services keep no state, so they survive batch chunk boundaries safely.
- All standard governor limits apply per transaction, shared across every layer.
- Avoid `@AuraEnabled` directly on a service - keep controllers thin so security checks are explicit.
- Deep layering costs CPU time; on very hot paths measure before adding another hop.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Batch Apex Job

*Process millions of records in chunks, beyond normal limits.*

### 🌱 Simple

*Beginner - plain language*

**Batch Apex** lets you process **huge volumes** (millions of records) by splitting the work into chunks (default 200). Each chunk gets its own fresh governor limits, so you can do what a single transaction never could.

Use it for: mass data cleanup, recalculations, archiving, nightly syncs.

### 📏 Limits

*Governor & platform limits*

- **5 concurrent** batch jobs; further submissions go to the Flex Queue (max **100** holding).
- `start()` QueryLocator: up to **50 million** records.
- Scope size max **2,000** (default 200).
- **100 callouts** per `execute()`, and only with `Database.AllowsCallouts`.
- Async Apex daily limit: 250,000 or 200 x licences, whichever is greater - shared with future/queueable/scheduled.
- In tests only **one**`execute()` chunk runs between `startTest/stopTest`.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Queueable Chain

*Async jobs you can chain, with callouts and complex types.*

### 🌱 Simple

*Beginner - plain language*

**Queueable Apex** is the modern async tool: run work in the background, pass **rich objects** (not just IDs), make **callouts**, and **chain** one job to the next. It's like `@future` but far more powerful.

### 📏 Limits

*Governor & platform limits*

- **50** jobs added per transaction from synchronous Apex; only **1** from inside a running Queueable.
- Chain depth: unlimited in production, **5** in Developer/Trial editions.
- Only **1** Finalizer per Queueable, and a Finalizer may enqueue only 1 job.
- Max **100** jobs in the queue awaiting execution at once (holding status).
- Counts against the shared async Apex daily limit.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Future Callout

*The simplest async callout — and why Queueable is usually better.*

### 🌱 Simple

*Beginner - plain language*

A **@future** method runs **asynchronously** in the background. Its classic use is making an **HTTP callout from a trigger** — because you *can't* call out synchronously during a trigger/DML transaction. `@future(callout=true)` defers the callout to a separate context.

### 📏 Limits

*Governor & platform limits*

- **50** future calls per transaction; counts toward the shared async daily limit.
- Cannot be called from a future, a batch, or a Queueable.
- Parameters: primitives and collections of primitives only - no sObjects.
- Must be `static void`; no return value.
- Callout limits still apply inside: 100 callouts, 120s total.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Test Class Writing

*Write tests that actually verify behavior and mock callouts.*

### 🌱 Simple

*Beginner - plain language*

Salesforce **requires 75% code coverage** to deploy, but good tests do more than cover lines — they **assert** the right thing happened. You create your own test data, run the logic inside `Test.startTest()/stopTest()`, and **mock** external callouts (you can't hit real endpoints in tests).

### 📏 Limits

*Governor & platform limits*

- Deployment gate: **75%** org-wide coverage and every trigger must have some coverage.
- `Test.startTest()/stopTest()` can be used **once** per test method.
- Only **one** batch `execute()` chunk runs in a test context.
- `@testSetup` is not allowed in a class with `SeeAllData=true`; its data is rolled back between methods.
- Tests get their own governor limits but still fail on CPU time if the code under test is slow.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Bulkified DML

*Never do SOQL/DML inside loops — collect then operate once.*

### 🌱 Simple

*Beginner - plain language*

**Bulkification** means writing Apex that handles **many records at once**. Salesforce fires triggers in batches of up to 200 and enforces hard limits (100 SOQL, 150 DML per transaction). The cardinal sin is putting a query or `insert/update`*inside a loop* — do them **once**, on collections.

### 📏 Limits

*Governor & platform limits*

- **150** DML statements and **10,000** rows per transaction.
- **100** SOQL queries and **50,000** query rows per transaction.
- Heap **6 MB** sync / **12 MB** async - large maps of fat sObjects hit this before the row limit.
- CPU time **10s** sync / **60s** async.
- Trigger batches arrive in chunks of 200 - a 10,000-row load fires the trigger 50 times, each with fresh limits.

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
