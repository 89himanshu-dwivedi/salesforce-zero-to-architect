[Home](../index.md) / [08 · Apex Advanced](index.md) / **Platform Events**

# Platform Events

3 topics · Series 8: Apex Advanced

**Topics on this page**

- [Publish Event](#publish-event)
- [Subscribe Event](#subscribe-event)
- [Event Driven Design](#event-driven-design)

## Publish Event

*Emitting Platform Events to broadcast changes asynchronously.*

### 🌱 Simple

*Beginner - plain language*

**Publishing a Platform Event** broadcasts a message that subscribers react to — decoupling producers from consumers. In Apex: `EventBus.publish(new MyEvent__e(Field__c=val))`.

### 📏 Limits

*Governor & platform limits*

- Event message maximum 1 MB.
- Daily publish allocations vary by edition and licence.
- Publish Immediately fires even if the transaction rolls back; Publish After Commit does not.
- `EventBus.publish` returns `SaveResult` - check it, publishes can fail.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Subscribe Event

*Consuming Platform Events via Apex triggers, Flows, or external clients.*

### 🌱 Simple

*Beginner - plain language*

**Subscribing** means reacting to published events. Options: an **Apex trigger** on the event (`after insert`), a Flow, or external clients via CometD/Pub-Sub API. The subscriber processes the event payload.

### 📏 Limits

*Governor & platform limits*

- Apex triggers on Platform Events run asynchronously with a default batch size of 2,000.
- Retention is 72 hours for high-volume events; Replay Ids expire with it.
- `EventBus.TriggerContext.setResumeCheckpoint()` is the only retry mechanism.
- A subscriber trigger that throws repeatedly is disabled after retry limits are reached.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Event Driven Design

*Architecting loosely-coupled systems around asynchronous events.*

### 🌱 Simple

*Beginner - plain language*

**Event-driven design** structures systems so components communicate by **publishing and reacting to events** rather than direct calls — producers and consumers are decoupled and evolve independently.

### 📏 Limits

*Governor & platform limits*

- Delivery is at-least-once - consumers must be idempotent.
- Ordering is guaranteed per publisher, not globally.
- 72-hour retention caps how far back you can replay.
- Daily publish and delivery allocations are edition-based and shared org-wide.

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
