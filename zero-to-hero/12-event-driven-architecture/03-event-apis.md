[Home](../index.md) / [12 · Event Driven Architecture](index.md) / **Event APIs**

# Event APIs

4 topics · Series 12: Event Driven Architecture

**Topics on this page**

- [Streaming API](#streaming-api)
- [EMP API](#emp-api)
- [Pub/Sub API](#pub-sub-api)
- [CometD](#cometd)

## Streaming API

*Push-based API for subscribing to Salesforce events.*

### 🌱 Simple

*Beginner - plain language*

The **Streaming API** is Salesforce's push-based API for subscribing to events — PushTopics, generic streaming, platform events, and CDC — delivering them to clients over a persistent connection (CometD/Bayeux) instead of polling.

### 📏 Limits

*Governor & platform limits*

- Concurrent client and daily delivery limits vary by edition.
- PushTopic queries are restricted - no aggregates, limited relationship traversal.
- Retention is 24-72 hours depending on channel type.
- Superseded by the Pub/Sub API for new work.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## EMP API

*Lightning component API for subscribing to streaming events.*

### 🌱 Simple

*Beginner - plain language*

The **emp API** (`lightning/empApi`) is a Lightning Web Component / Aura module that lets client-side components subscribe to streaming channels (platform events, CDC) directly in the browser.

### 📏 Limits

*Governor & platform limits*

- Java-only client library.
- Subject to the same streaming client and delivery limits.
- Requires managing Replay Id persistence yourself.
- Superseded by the Pub/Sub API for new work.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Pub/Sub API

*Modern gRPC API to publish and subscribe with schema.*

### 🌱 Simple

*Beginner - plain language*

The **Pub/Sub API** is Salesforce's modern, high-performance event API — using **gRPC/HTTP2 with binary Avro** — to publish, subscribe, and retrieve event schemas through a single efficient interface.

### 📏 Limits

*Governor & platform limits*

- gRPC over HTTP/2 - requires a supported client library.
- 72-hour retention for high-volume events; Replay Ids expire with it.
- Daily publish and delivery allocations are edition-based.
- A slow consumer falls behind and loses events past retention.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## CometD

*The Bayeux long-polling library behind streaming.*

### 🌱 Simple

*Beginner - plain language*

**CometD** is the JavaScript/Java library implementing the **Bayeux protocol** (long-polling pub/sub) that powers Salesforce's classic Streaming API delivery — maintaining a persistent channel for server-pushed events.

### 📏 Limits

*Governor & platform limits*

- Long-polling over HTTP - less efficient than gRPC.
- Concurrent client limits vary by edition.
- Reconnect storms consume delivery allocation rapidly.
- Replay Id handling is the client's responsibility.

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
