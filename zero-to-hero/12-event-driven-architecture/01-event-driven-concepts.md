[Home](../index.md) / [12 · Event Driven Architecture](index.md) / **Event Driven Concepts**

# Event Driven Concepts

5 topics · Series 12: Event Driven Architecture

**Topics on this page**

- [Publisher](#publisher)
- [Subscriber](#subscriber)
- [Event Bus](#event-bus)
- [Event Consumer](#event-consumer)
- [Event Producer](#event-producer)

## Publisher

*The component that produces and emits events.*

### 🌱 Simple

*Beginner - plain language*

A **publisher** creates and sends events to an event bus/channel when something happens — without knowing or caring who (if anyone) consumes them. It announces "this happened" and moves on.

### 📏 Limits

*Governor & platform limits*

- Event message maximum 1 MB.
- Daily publish allocations vary by edition and licence.
- `EventBus.publish` returns `SaveResult` - publishes can and do fail.
- Publish Immediately fires even when the transaction rolls back.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Subscriber

*The component that listens for and reacts to events.*

### 🌱 Simple

*Beginner - plain language*

A **subscriber** listens to an event channel and reacts when relevant events arrive — running its own logic independently of the publisher and other subscribers.

### 📏 Limits

*Governor & platform limits*

- Apex Platform Event triggers run asynchronously with a default batch of 2,000 events.
- A subscriber trigger that keeps failing is disabled after repeated retries.
- Retention 72 hours caps how far a subscriber can fall behind.
- Delivery allocations are shared org-wide across all subscribers.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Event Bus

*The channel that transports events from publishers to subscribers.*

### 🌱 Simple

*Beginner - plain language*

The **event bus** is the pipeline/broker that receives events from publishers and delivers them to subscribers — decoupling the two sides and buffering events in between.

### 📏 Limits

*Governor & platform limits*

- Retention is 72 hours for high-volume events - it is not durable storage.
- Ordering is guaranteed per publisher, not globally.
- No native dead-letter queue or replay UI.
- Daily publish and delivery allocations are edition-based.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Event Consumer

*The endpoint that processes events to produce an outcome.*

### 🌱 Simple

*Beginner - plain language*

An **event consumer** is the system/process that receives events and acts on them — essentially a subscriber viewed from the "who does the work" angle (updates data, triggers a downstream action).

### 📏 Limits

*Governor & platform limits*

- Must store its Replay Id to resume; the window is 72 hours.
- Delivery is at-least-once, so consumers must be idempotent.
- Slow consumers fall behind and lose events past retention.
- Each subscribed client consumes delivery allocation.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Event Producer

*The originating system generating event-worthy facts.*

### 🌱 Simple

*Beginner - plain language*

An **event producer** is the system or component that generates the business facts which become events — the origin of "something happened" that the publisher emits.

### 📏 Limits

*Governor & platform limits*

- Publishing counts as DML-like work but not against the callout limit.
- Message size capped at 1 MB.
- Publish failures must be checked via `SaveResult`, not assumed.
- Producers cannot control consumer processing order.

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
