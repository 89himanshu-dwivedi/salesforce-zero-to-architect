[Home](../index.md) / [12 · Event Driven Architecture](index.md) / **Event Monitoring**

# Event Monitoring

4 topics · Series 12: Event Driven Architecture

**Topics on this page**

- [Dead Letter Queue](#dead-letter-queue)
- [Poison Messages](#poison-messages)
- [Event Replay](#event-replay)
- [Event Recovery](#event-recovery)

## Dead Letter Queue

*Sidelined store for events that can't be processed.*

### 🌱 Simple

*Beginner - plain language*

In event monitoring, a **Dead Letter Queue (DLQ)** captures events that repeatedly fail processing (after retries) so the main stream keeps flowing and failed events can be inspected and replayed.

### 📏 Limits

*Governor & platform limits*

- No native DLQ in Salesforce - build a custom object with the payload and error.
- Stored payloads consume data storage and need retention rules.
- Replay is bounded by the 72-hour event retention if you rely on Replay Ids.
- Replay must be idempotent.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Poison Messages

*Events that always fail and block processing.*

### 🌱 Simple

*Beginner - plain language*

A **poison message** is an event that can never be processed successfully (e.g., malformed or triggering a bug) — if not handled, it's retried forever and blocks the consumer.

### 📏 Limits

*Governor & platform limits*

- A repeatedly failing subscriber trigger is disabled by the platform after retry limits.
- There is no automatic quarantine - detect and divert them yourself.
- `setResumeCheckpoint()` is the only in-trigger retry control.
- One poison message can block an entire event stream.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Event Replay

*Re-processing past events for recovery or new consumers.*

### 🌱 Simple

*Beginner - plain language*

**Event replay** re-delivers past events from the bus/log so a consumer can reprocess them — to recover after failures, rebuild state, or onboard a new consumer that needs history.

### 📏 Limits

*Governor & platform limits*

- Limited to the 72-hour retention window.
- Replaying a large backlog can exceed daily delivery allocations.
- Consumers must be idempotent or replay creates duplicates.
- Replay Ids are not portable between channels.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Event Recovery

*Restoring correct state after event failures/gaps.*

### 🌱 Simple

*Beginner - plain language*

**Event recovery** is the overall strategy for getting back to a correct, consistent state after failures, gaps, or downtime — combining replay, reconciliation, DLQ reprocessing, and idempotency.

### 📏 Limits

*Governor & platform limits*

- Beyond 72 hours, recovery requires a separate durable store - events are gone.
- Bulk recovery consumes API and async allocations.
- Recovery must be idempotent and rate-limited to avoid a second incident.
- Test the recovery path; an untested recovery plan is a wish.

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
