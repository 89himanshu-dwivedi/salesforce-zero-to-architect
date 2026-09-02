[Home](../index.md) / [11 · Integrations](index.md) / **Integration Fundamentals**

# Integration Fundamentals

11 topics · Series 11: Integrations

**Topics on this page**

- [System Connectivity](#system-connectivity)
- [Data Synchronization](#data-synchronization)
- [Real Time Integration](#real-time-integration)
- [Near Real Time](#near-real-time)
- [Batch Integration](#batch-integration)
- [Request Reply Pattern](#request-reply-pattern)
- [Fire and Forget](#fire-and-forget)
- [Remote Process Invocation](#remote-process-invocation)
- [Batch Data Synchronization](#batch-data-synchronization)
- [Event Driven Integration](#event-driven-integration)
- [Data Virtualization](#data-virtualization)

## System Connectivity

*How Salesforce connects to external systems — the foundation.*

### 🌱 Simple

*Beginner - plain language*

**System connectivity** is how Salesforce talks to other systems — outbound (Salesforce calls out) or inbound (others call Salesforce) — over protocols like HTTP/REST, SOAP, and streaming.

### 📏 Limits

*Governor & platform limits*

- Daily API request limit is org-wide and shared by every connected system.
- 100 callouts and 120s cumulative callout time per Apex transaction.
- No callouts after uncommitted DML in the same transaction.
- Concurrent long-running (>5s) synchronous requests capped at 10 per org.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Data Synchronization

*Keeping data consistent across systems over time.*

### 🌱 Simple

*Beginner - plain language*

**Data synchronization** keeps records consistent between Salesforce and other systems — one-way or bidirectional — so both reflect the same state.

### 📏 Limits

*Governor & platform limits*

- Bidirectional sync has no native conflict resolution - last write wins.
- Bulk API 2.0 allows 150 million records per 24-hour rolling window.
- Platform Event retention is 72 hours, which caps replay-based recovery.
- Field-level merge is not provided; design ownership per field.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Real Time Integration

*Synchronous, immediate data exchange on demand.*

### 🌱 Simple

*Beginner - plain language*

**Real-time integration** exchanges data immediately and synchronously — e.g., Salesforce calls an external API during a user action and waits for the response right away.

### 📏 Limits

*Governor & platform limits*

- Synchronous callouts must complete inside the 120s transaction budget.
- Only 10 concurrent synchronous requests may exceed 5 seconds org-wide.
- User-facing saves block on the callout - vendor latency becomes your latency.
- True real-time bidirectional sync is rarely achievable within governor limits.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Near Real Time

*Low-latency async exchange without blocking the user.*

### 🌱 Simple

*Beginner - plain language*

**Near-real-time** integration processes data within seconds/minutes asynchronously — fast but not instant — so the user isn't blocked waiting for the external system.

### 📏 Limits

*Governor & platform limits*

- Async Apex daily allocation: 250,000 or 200 x licences, whichever is greater.
- Queueable delay granularity is minutes, not seconds.
- Platform Event delivery allocations are edition-based.
- Eventual consistency must be surfaced in the UI or users assume failure.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Batch Integration

*Scheduled bulk transfer of large data volumes.*

### 🌱 Simple

*Beginner - plain language*

**Batch integration** moves large volumes of data on a schedule (e.g., nightly) in bulk, rather than record-by-record in real time — efficient for big, non-urgent datasets.

### 📏 Limits

*Governor & platform limits*

- Bulk API 1.0: 10,000 records per batch, 10,000 batches per 24 hours.
- Bulk API 2.0: 150 million records per 24 hours, chunking handled automatically.
- Batch processing timeout is 10 minutes per batch in Bulk 1.0.
- Triggers still fire in chunks of 200 inside every bulk batch.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Request Reply Pattern

*Synchronous call-and-wait integration pattern.*

### 🌱 Simple

*Beginner - plain language*

The **request-reply** pattern is synchronous: the caller sends a request and waits for the response before continuing — a two-way, blocking exchange.

### 📏 Limits

*Governor & platform limits*

- The caller blocks for the full round trip - bounded by the 120s callout budget.
- Counts toward the 10 concurrent long-running request limit.
- Timeout is ambiguous: the remote side may have committed.
- Not suitable when the remote operation can exceed a few seconds.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Fire and Forget

*Asynchronous send-and-don't-wait integration pattern.*

### 🌱 Simple

*Beginner - plain language*

**Fire-and-forget** sends a message/request without waiting for a response — the caller continues immediately, trusting the message will be processed (or queued/retried).

### 📏 Limits

*Governor & platform limits*

- Delivery is at-least-once at best; there is no acknowledgement to act on.
- Platform Event retention 72 hours limits recovery.
- Outbound Messages retry for 24 hours then discard silently.
- Requires an idempotent receiver or duplicates are guaranteed.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Remote Process Invocation

*Invoking a process in another system, with or without reply.*

### 🌱 Simple

*Beginner - plain language*

**Remote process invocation (RPI)** means triggering a process/operation in an external system from Salesforce — either request-reply (wait for result) or fire-and-forget (trigger and continue).

### 📏 Limits

*Governor & platform limits*

- Subject to 100 callouts / 120s per transaction.
- Blocked after uncommitted DML - must be async from a trigger.
- Request and response bodies capped at 6 MB sync / 12 MB async.
- Long remote processes need a submit-and-poll or callback design.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Batch Data Synchronization

*Bulk, scheduled reconciliation of datasets between systems.*

### 🌱 Simple

*Beginner - plain language*

**Batch data synchronization** reconciles large datasets between systems on a schedule — exporting/importing changes in bulk to bring both into agreement.

### 📏 Limits

*Governor & platform limits*

- Bulk API limits apply; hard delete bypasses the Recycle Bin irreversibly.
- Large loads trigger sharing recalculation - defer it and recalculate once.
- Unsorted child loads cause `UNABLE_TO_LOCK_ROW` on skewed parents.
- Data storage must have headroom before the load, not after.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Event Driven Integration

*Integrating via events on a bus rather than direct calls.*

### 🌱 Simple

*Beginner - plain language*

**Event-driven integration** connects systems through events on a bus: producers publish events, consumers subscribe — instead of systems calling each other directly.

### 📏 Limits

*Governor & platform limits*

- Event message maximum 1 MB; 72-hour retention for high-volume events.
- Daily publish and delivery allocations vary by edition.
- Ordering is guaranteed per publisher only, not globally.
- Consumers must be idempotent - delivery is at-least-once.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Data Virtualization

*Accessing external data live without copying it into Salesforce.*

### 🌱 Simple

*Beginner - plain language*

**Data virtualization** exposes external data inside Salesforce *without storing it* — via Salesforce Connect external objects that query the source in real time on demand.

### 📏 Limits

*Governor & platform limits*

- External Objects support a limited SOQL subset - no aggregates, limited joins.
- Maximum 100-200 external objects per org depending on edition.
- OData callouts count toward API and callout limits and add page-load latency.
- External objects cannot be used in roll-ups, and reporting on them is restricted.

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
