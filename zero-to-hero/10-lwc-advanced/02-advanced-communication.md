[Home](../index.md) / [10 · LWC Advanced](index.md) / **Advanced Communication**

# Advanced Communication

4 topics · Series 10: LWC Advanced

**Topics on this page**

- [LMS (Lightning Message Service)](#lms-lightning-message-service)
- [Message Channels](#message-channels)
- [Pub Sub](#pub-sub)
- [Cross DOM Communication](#cross-dom-communication)

## LMS (Lightning Message Service)

*Publish/subscribe communication across the DOM and tech stacks.*

### 🌱 Simple

*Beginner - plain language*

**Lightning Message Service (LMS)** enables communication between components **anywhere on the page** — even across LWC, Aura, and Visualforce — via a publish/subscribe message channel.

### 📏 Limits

*Governor & platform limits*

- No delivery guarantee, no ordering, no replay - messages are lost if nobody is listening.
- Message channels are metadata and require a deploy to change.
- Must unsubscribe in `disconnectedCallback` or handlers leak per console tab.
- Not supported on every mobile surface.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Message Channels

*The metadata defining an LMS communication contract.*

### 🌱 Simple

*Beginner - plain language*

A **Message Channel** (`.messageChannel-meta.xml`) is metadata defining a named LMS channel and its fields. Components import it (`@salesforce/messageChannel/MyChannel__c`) to publish/subscribe.

### 📏 Limits

*Governor & platform limits*

- `isExposed` must be true for Visualforce or managed package use.
- Fields are a published contract - removing one is a breaking change.
- Payloads should stay small; LMS is a UI bus, not a data transport.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Pub Sub

*The publish/subscribe pattern — and why LMS replaced the old utility.*

### 🌱 Simple

*Beginner - plain language*

**Pub/Sub** is a pattern where publishers emit messages and subscribers react, without direct references. In LWC, the supported implementation is **LMS**; the old singleton `pubsub` utility is deprecated for cross-DOM use.

### 📏 Limits

*Governor & platform limits*

- The legacy pubsub module works only within one page region and is unsupported.
- No cross-technology support - Visualforce and Aura cannot participate.
- Superseded entirely by LMS.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Cross DOM Communication

*Communicating across Shadow DOM and component boundaries reliably.*

### 🌱 Simple

*Beginner - plain language*

**Cross-DOM communication** means passing data between components separated by Shadow DOM boundaries or with no parent-child link. The reliable tools are **LMS** (client) and **Platform Events / empApi** (server-driven).

### 📏 Limits

*Governor & platform limits*

- Non-composed events cannot cross shadow boundaries.
- Composed bubbling events couple distant components and are hard to trace.
- LMS is the only supported cross-tree mechanism, and it is fire-and-forget.

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
