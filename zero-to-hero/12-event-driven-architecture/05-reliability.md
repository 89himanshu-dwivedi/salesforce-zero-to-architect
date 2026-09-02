[Home](../index.md) / [12 · Event Driven Architecture](index.md) / **Reliability**

# Reliability

5 topics · Series 12: Event Driven Architecture

**Topics on this page**

- [Replay ID](#replay-id)
- [Event Ordering](#event-ordering)
- [Event Retention](#event-retention)
- [Idempotency](#idempotency)
- [Duplicate Handling](#duplicate-handling)

## Replay ID

*Per-event position marker for recovery.*

### 🌱 Simple

*Beginner - plain language*

A **Replay ID** is a unique, increasing marker assigned to each event on the bus, identifying its position. Subscribers store it to resume from the right place after a disconnect.

### 📏 Limits

*Governor & platform limits*

- Valid only within the 72-hour retention window.
- Opaque and not sequential across channels.
- Storing it durably is the consumer's responsibility.
- An expired Replay Id causes the subscription to fail, not to silently reset.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Event Ordering

*Guarantees (and limits) on the order events arrive.*

### 🌱 Simple

*Beginner - plain language*

**Event ordering** concerns whether subscribers receive events in the same order they were published. Salesforce preserves order *per channel* via replay ID, but not globally across channels.

### 📏 Limits

*Governor & platform limits*

- Guaranteed per publisher, not globally across publishers.
- Parallel consumers can process out of order.
- Use the CDC transaction key or your own sequence field where order matters.
- Retries break ordering by definition.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Event Retention

*How long events stay on the bus for replay.*

### 🌱 Simple

*Beginner - plain language*

**Event retention** is how long the event bus keeps events available for replay. For Salesforce high-volume platform events and CDC, retention is **72 hours**.

### 📏 Limits

*Governor & platform limits*

- 72 hours for high-volume Platform Events and CDC.
- 24 hours for PushTopic and generic streaming.
- Retention cannot be extended - archive to a custom object if you need longer.
- Consumers offline beyond the window lose data permanently.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Idempotency

*Same event processed twice yields the same result.*

### 🌱 Simple

*Beginner - plain language*

**Idempotency** means processing the same event more than once has the same effect as processing it once — essential because event delivery is **at-least-once** (duplicates happen).

### 📏 Limits

*Governor & platform limits*

- Requires a unique External Id on the consumer side to deduplicate.
- Deduplication state consumes data storage and needs purging.
- Upsert on a non-unique External Id throws when multiple matches exist.
- Idempotency is mandatory - delivery is at-least-once, not exactly-once.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Duplicate Handling

*Detecting and discarding repeated events.*

### 🌱 Simple

*Beginner - plain language*

**Duplicate handling** is the practical mechanism for detecting that an event has already been processed and discarding the repeat — the runtime side of idempotency.

### 📏 Limits

*Governor & platform limits*

- Deduplication keys must be indexed or lookups become non-selective at volume.
- Duplicate rules can silently block API inserts during recovery.
- Dedup windows must exceed the 72-hour retention to cover full replay.

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
