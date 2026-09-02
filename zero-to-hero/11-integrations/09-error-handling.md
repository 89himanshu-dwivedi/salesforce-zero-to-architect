[Home](../index.md) / [11 · Integrations](index.md) / **Error Handling**

# Error Handling

5 topics · Series 11: Integrations

**Topics on this page**

- [Retry Strategy](#retry-strategy)
- [Exponential Backoff](#exponential-backoff)
- [Dead Letter Queue](#dead-letter-queue)
- [Circuit Breaker](#circuit-breaker)
- [Fallback Pattern](#fallback-pattern)

## Retry Strategy

*Re-attempting failed calls to handle transient errors.*

### 🌱 Simple

*Beginner - plain language*

A **retry strategy** automatically re-attempts a failed operation (timeout, 5xx, throttling) because many integration failures are *transient* and succeed on a second try.

### 📏 Limits

*Governor & platform limits*

- Retry only 408, 429 and 5xx; other 4xx will fail identically forever.
- Apex cannot sleep - delays require async re-enqueue with minute granularity.
- Each attempt consumes a callout and an async execution.
- Safe only when the receiver is idempotent.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Exponential Backoff

*Increasing delay between retries (plus jitter).*

### 🌱 Simple

*Beginner - plain language*

**Exponential backoff** increases the wait between retries exponentially (e.g., 1s, 2s, 4s, 8s) instead of retrying immediately — easing pressure on a struggling service and improving recovery odds.

### 📏 Limits

*Governor & platform limits*

- Queueable delay granularity is minutes, so sub-minute backoff is not possible.
- Add jitter or synchronised clients retry in lockstep and re-create the spike.
- Cap total attempts; unbounded retry consumes the async daily allocation.
- Honour `Retry-After` when the vendor supplies it.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Dead Letter Queue

*Holding pen for messages that can't be processed.*

### 🌱 Simple

*Beginner - plain language*

A **Dead Letter Queue (DLQ)** stores messages/events that repeatedly fail processing (after retries are exhausted) so they aren't lost — to be inspected, fixed, and reprocessed later.

### 📏 Limits

*Governor & platform limits*

- Salesforce has no native DLQ - you build the object and the replay UI.
- Stored payloads consume data storage and need a retention policy.
- Platform Event replay is limited to the 72-hour retention window.
- Replay must be idempotent or recovery creates duplicates.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Circuit Breaker

*Stop calling a failing service to let it recover.*

### 🌱 Simple

*Beginner - plain language*

The **circuit breaker** pattern stops sending requests to a downstream service that's failing — "tripping open" after repeated failures — to avoid wasting resources and to give the service time to recover.

### 📏 Limits

*Governor & platform limits*

- No native support - implement with Custom Metadata or a Custom Setting flag.
- State must survive across transactions, so statics cannot hold it.
- A tripped breaker is a silent outage unless it alerts.
- Half-open probing needs its own scheduled job.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Fallback Pattern

*Graceful degradation when a dependency fails.*

### 🌱 Simple

*Beginner - plain language*

The **fallback pattern** provides an alternative response when a primary operation fails — cached data, a default value, or a degraded experience — so the system stays useful instead of erroring out.

### 📏 Limits

*Governor & platform limits*

- Cached fallback data is bounded by Platform Cache size (100 KB per item) and TTL.
- Stale fallback data can be worse than an error - surface staleness in the UI.
- Fallback paths are rarely tested; include them in the regression suite.

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
