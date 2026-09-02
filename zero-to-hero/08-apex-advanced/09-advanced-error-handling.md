[Home](../index.md) / [08 · Apex Advanced](index.md) / **Advanced Error Handling**

# Advanced Error Handling

4 topics · Series 8: Apex Advanced

**Topics on this page**

- [Logging Framework](#logging-framework)
- [Error Monitoring](#error-monitoring)
- [Retry Logic](#retry-logic)
- [Dead Letter Strategy](#dead-letter-strategy)

## Logging Framework

*A structured, persistent way to capture errors and diagnostics.*

### 🌱 Simple

*Beginner - plain language*

A **logging framework** captures errors and key events into persistent records (a custom object or Platform Event) so you can diagnose issues after they happen — far better than relying on transient debug logs.

### 📏 Limits

*Governor & platform limits*

- Log rows written with normal DML are rolled back with the transaction - publish via Platform Event to survive.
- Log objects consume data storage and need scheduled purging or Big Object archiving.
- Long text fields cap at 131,072 characters and are not filterable.
- Logs easily become a PII store - mask before writing.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Error Monitoring

*Proactively detecting, alerting on, and triaging failures.*

### 🌱 Simple

*Beginner - plain language*

**Error monitoring** means actively watching for failures — querying log records, alerting on errors, and tracking async job failures — so issues are caught proactively rather than reported by users.

### 📏 Limits

*Governor & platform limits*

- Apex exception emails cover unhandled exceptions only - handled errors are invisible.
- No aggregation or rate limiting; one bad deploy floods the channel.
- Debug logs are unsuitable for monitoring: 20 MB cap, 7-day retention, 1,000 MB org allocation.
- Event Monitoring requires an add-on licence and is not real-time.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Retry Logic

*Recovering from transient failures with bounded, backed-off retries.*

### 🌱 Simple

*Beginner - plain language*

**Retry logic** re-attempts a failed operation (e.g., a callout) that may succeed on a second try — useful for **transient** errors like timeouts or 5xx, with a cap on attempts.

### 📏 Limits

*Governor & platform limits*

- Only 408, 429 and 5xx are worth retrying; 4xx will fail identically.
- A transaction cannot sleep - delays require async re-enqueue (minute granularity).
- Each attempt consumes a callout and an async execution.
- Retries are only safe when the receiver is idempotent.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Dead Letter Strategy

*Capturing messages that fail all retries for later handling.*

### 🌱 Simple

*Beginner - plain language*

A **dead-letter strategy** captures operations/messages that **fail even after retries** into a separate store ("dead-letter queue") so they aren't lost and can be reviewed or reprocessed.

### 📏 Limits

*Governor & platform limits*

- Salesforce provides no native dead-letter queue - you build the object.
- Stored payloads consume data storage; plan retention and purging.
- Platform Event replay is limited to the 72-hour retention window.
- Replay must be idempotent or you create duplicates during recovery.

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
