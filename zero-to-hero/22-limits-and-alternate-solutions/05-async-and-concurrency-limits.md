[Home](../index.md) / [22 · Limits & Alternate Solutions](index.md) / **Async & Concurrency Limits**

# Async & Concurrency Limits

10 topics · Series 22: Limits & Alternate Solutions

**Topics on this page**

- [Batch 5 Concurrent Limit](#batch-5-concurrent-limit)
- [Flex Queue 100 Limit](#flex-queue-100-limit)
- [Async Apex Daily Limit](#async-apex-daily-limit)
- [Queueable Chain Depth](#queueable-chain-depth)
- [Future Methods Daily Limit](#future-methods-daily-limit)
- [Scheduled Jobs 100 Limit](#scheduled-jobs-100-limit)
- [Long-Running Request Limit](#long-running-request-limit)
- [Platform Event Publish Limit](#platform-event-publish-limit)
- [Bulk API Batch Limit](#bulk-api-batch-limit)
- [Streaming Event Limit](#streaming-event-limit)

## Batch 5 Concurrent Limit

*Only 5 batch jobs run at once — the rest queue (Flex Queue).*

### 🌱 Simple

*Beginner - plain language*

Salesforce runs at most **5 Batch Apex jobs concurrently**. Additional jobs wait in the **Apex Flex Queue** (holds up to 100). So firing 20 batches doesn't parallelize 20 — only 5 run, the rest queue. Alternate: **fewer, larger batches; chain instead of flood; tune batch size**.

### 📏 Limits

*Governor & platform limits*

- 5 concurrent batch jobs.
- Flex Queue holds 100.
- Scheduled batches count toward the 5.
- Async daily limit applies on top.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Flex Queue 100 Limit

*Apex Flex Queue holds 100 pending batches — throttle intake.*

### 🌱 Simple

*Beginner - plain language*

The **Apex Flex Queue** can hold up to **100 batch jobs** waiting to run. Exceed it and new `executeBatch` calls fail. Alternate: **throttle how many you submit, reorder priorities in the queue, and consolidate jobs**.

### 📏 Limits

*Governor & platform limits*

- 100 jobs in Holding.
- 5 executing concurrently.
- Submitting when full throws `LimitException`.
- Order is editable but not priority-weighted.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Async Apex Daily Limit

*Daily cap on async invocations (≈250k or 200×licenses).*

### 🌱 Simple

*Beginner - plain language*

Async Apex (future, queueable, batch, scheduled) shares a **daily limit** of roughly **250,000 or 200 × user licenses**, whichever is higher. Heavy per-record async fired from triggers can exhaust it. Alternate: **batch the work, aggregate async calls, and avoid one-async-per-record patterns**.

### 📏 Limits

*Governor & platform limits*

- 250,000 or 200 x licences per 24h, whichever is greater.
- Each batch `execute()` chunk counts.
- Shared across all async types.
- Exceeding it blocks all async processing org-wide.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Queueable Chain Depth

*Chains are single-file (1 child) and need a stop condition.*

### 🌱 Simple

*Beginner - plain language*

A running Queueable can enqueue only **one child**, so chains are linear. There's no hard production depth cap, but an unbounded chain runs forever and risks consuming async limits. Alternate: **add an explicit stop condition and a max-iteration guard**; for parallelism use Batch.

### 📏 Limits

*Governor & platform limits*

- Depth 5 in Developer/Trial editions.
- 1 enqueue from within a Queueable.
- 100 jobs may be in Holding.
- Counts toward async daily limit.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Future Methods Daily Limit

*@future shares async limits and can't chain — aggregate.*

### 🌱 Simple

*Beginner - plain language*

`@future` calls count against the **async daily limit** and are capped at **50 per transaction**. Because they can't chain or take objects, heavy use is wasteful. Alternate: **aggregate work, pass Id lists, and prefer Queueable/Batch**.

### 📏 Limits

*Governor & platform limits*

- 50 futures per transaction.
- Shared daily async allocation.
- No job Id, no monitoring.
- Cannot be called from async contexts.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Scheduled Jobs 100 Limit

*Max 100 scheduled Apex jobs — consolidate schedulers.*

### 🌱 Simple

*Beginner - plain language*

You can have at most **100 scheduled Apex jobs** at once. Scheduling many small jobs hits this. Alternate: **one scheduler that dispatches multiple tasks, or fewer cron entries doing more work**.

### 📏 Limits

*Governor & platform limits*

- 100 scheduled Apex jobs per org.
- Standard granularity is hourly.
- Runs as the scheduling user.
- 5 concurrent scheduled/batch executions.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Long-Running Request Limit

*Max 10 concurrent requests over 5s — keep transactions short.*

### 🌱 Simple

*Beginner - plain language*

An org allows only **10 synchronous requests running longer than 5 seconds** at the same time. Slow synchronous Apex/pages eat these "long-running" slots and can **block other users**. Alternate: **make transactions fast (cacheable, selective queries) and move slow work async**.

### 📏 Limits

*Governor & platform limits*

- 10 concurrent sync requests over 5 seconds.
- 25 concurrent long-running API requests (>20s).
- Applies org-wide, not per user.
- Continuations do not count while waiting.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Platform Event Publish Limit

*Daily event publish caps & delivery limits — batch publishes.*

### 🌱 Simple

*Beginner - plain language*

Platform Events have **daily publish and delivery limits** (by edition/add-on). Publishing one event per record in a tight loop can exhaust them. Alternate: **publish coarser-grained events (batch many changes into one), and design consumers to be efficient**.

### 📏 Limits

*Governor & platform limits*

- Daily publish/delivery allocations vary by edition.
- 72-hour retention.
- 1 MB max event message size.
- Ordering guaranteed per publisher only.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Bulk API Batch Limit

*Bulk API daily batch caps & 10k records/batch — size right.*

### 🌱 Simple

*Beginner - plain language*

Bulk API processes data in batches with **daily batch limits** and up to **10,000 records per batch**. Too-large or too-many batches, or **parallel mode on skewed data**, cause failures/locks. Alternate: **right-size batches and use serial mode when locking**.

### 📏 Limits

*Governor & platform limits*

- Bulk 1.0: 10,000 records/batch, 10,000 batches per 24h.
- Bulk 2.0: 150 million records per 24h.
- Batch processing timeout 10 minutes per batch.
- Hard delete is irreversible.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Streaming Event Limit

*CometD/streaming concurrent client & event caps.*

### 🌱 Simple

*Beginner - plain language*

Streaming (PushTopic, CDC, Platform Events via CometD) has limits on **concurrent clients and events delivered**. Too many subscribers or high event volume hits caps. Alternate: **fewer shared subscriptions, coarser events, and a middleware fan-out** for many consumers.

### 📏 Limits

*Governor & platform limits*

- Concurrent client and daily delivery limits vary by edition.
- CDC/HVPE retention 72 hours.
- Max 1 MB event message.
- Ordering guaranteed per channel/publisher.

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
