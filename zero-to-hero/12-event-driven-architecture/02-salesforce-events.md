[Home](../index.md) / [12 · Event Driven Architecture](index.md) / **Salesforce Events**

# Salesforce Events

9 topics · Series 12: Event Driven Architecture

**Topics on this page**

- [Publish Immediately](#publish-immediately)
- [Publish After Commit](#publish-after-commit)
- [Standard Platform Event](#standard-platform-event)
- [High Volume Platform Event](#high-volume-platform-event)
- [CDC Events](#cdc-events)
- [Replay Events](#replay-events)
- [Change Event Header](#change-event-header)
- [Generic Streaming](#generic-streaming)
- [PushTopic](#pushtopic)

## Publish Immediately

*Publish behavior that fires as soon as publish() runs.*

### 🌱 Simple

*Beginner - plain language*

**Publish Immediately** means a platform event is published the instant `EventBus.publish()` executes — even if the surrounding transaction later rolls back. It does *not* wait for commit.

### 📏 Limits

*Governor & platform limits*

- Fires even if the enclosing transaction later rolls back - consumers can act on work that never happened.
- Cannot be undone by a `Savepoint` rollback.
- Appropriate only for logging and telemetry, not for business events.
- Same 1 MB message and daily allocation limits.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Publish After Commit

*Publish behavior that fires only when the transaction commits.*

### 🌱 Simple

*Beginner - plain language*

**Publish After Commit** means a platform event is published only *after* the surrounding transaction successfully commits — so a rollback prevents the event from ever being sent.

### 📏 Limits

*Governor & platform limits*

- Only published if the transaction commits successfully.
- Adds latency equal to the remaining transaction time.
- The default and correct choice for business events.
- Still at-least-once delivery to consumers.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Standard Platform Event

*Transaction-coupled platform events (publish after commit).*

### 🌱 Simple

*Beginner - plain language*

A **Standard (-Volume) Platform Event** is a platform event that publishes *after commit* and participates in the transaction — good for moderate volumes where event-data consistency matters.

### 📏 Limits

*Governor & platform limits*

- Standard-volume events have lower daily allocations and shorter retention than high-volume.
- Message maximum 1 MB.
- Schema changes are additive only - removing a field breaks consumers.
- Cannot be updated or deleted once published.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## High Volume Platform Event

*Scalable platform events (publish immediately, 72h retention).*

### 🌱 Simple

*Beginner - plain language*

A **High-Volume Platform Event** is the scalable platform event type — it publishes *immediately*, retains events for **72 hours** for replay, and handles large event volumes. It's the recommended default.

### 📏 Limits

*Governor & platform limits*

- 72-hour retention; Replay Ids expire with it.
- Daily publish and delivery allocations vary by edition and add-on.
- Delivery to CometD/Pub-Sub clients is counted separately from publishes.
- Ordering guaranteed per publisher only.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## CDC Events

*Change Data Capture — auto-published record change events.*

### 🌱 Simple

*Beginner - plain language*

**Change Data Capture (CDC)** automatically publishes events when records are created, updated, deleted, or undeleted — so external systems can stay in sync with Salesforce data changes in near real time.

### 📏 Limits

*Governor & platform limits*

- Change Data Capture must be enabled per object and has a per-org object limit.
- 72-hour retention.
- Not all objects and field types are supported.
- Large transactions can produce header-only events with overflow - handle that path.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Replay Events

*Re-reading past events from the bus using replay ID.*

### 🌱 Simple

*Beginner - plain language*

**Replay** lets a subscriber re-read past events from the event bus (within the retention window) by specifying a **replay ID** — so a consumer that was down or restarted can catch up without losing events.

### 📏 Limits

*Governor & platform limits*

- Replay Ids are valid only within the 72-hour retention window.
- `-2` replays from earliest retained, `-1` from now.
- Replaying a large backlog can exceed delivery allocations.
- Replay must be idempotent on the consumer side.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Change Event Header

*Metadata block describing a CDC change.*

### 🌱 Simple

*Beginner - plain language*

The **Change Event Header** is a metadata section in every CDC event describing the change — what kind (create/update/delete/undelete), which records, when, and the originating transaction.

### 📏 Limits

*Governor & platform limits*

- Contains changed field names, entity, change type and transaction key.
- Large transactions emit a header with an overflow indicator instead of full detail.
- Header fields are fixed and cannot be extended.
- Transaction key lets you group related changes - use it for consistency.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Generic Streaming

*Custom user-defined streaming events (not tied to records).*

### 🌱 Simple

*Beginner - plain language*

**Generic Streaming** lets you push arbitrary, custom notifications (not tied to record changes) to subscribers via a user-defined **streaming channel** — useful for app-specific signals.

### 📏 Limits

*Governor & platform limits*

- Payload is limited (typically 3,000 characters) and untyped.
- Retention is shorter than for high-volume Platform Events.
- No schema, so consumers have no contract to rely on.
- Largely superseded by Platform Events.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## PushTopic

*Legacy SOQL-based streaming of record changes.*

### 🌱 Simple

*Beginner - plain language*

A **PushTopic** streams notifications when records matching a defined **SOQL query** change — a legacy way to get real-time record updates pushed to subscribers via CometD.

### 📏 Limits

*Governor & platform limits*

- Query syntax is restricted - no aggregates, limited relationship traversal.
- Maximum 100 PushTopics per org.
- Retention 24 hours.
- Legacy technology; use CDC or Platform Events instead.

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
