[Home](../index.md) / [12 · Event Driven Architecture](index.md) / **Event Patterns**

# Event Patterns

5 topics · Series 12: Event Driven Architecture

**Topics on this page**

- [Event Notification](#event-notification)
- [Event Carried State Transfer](#event-carried-state-transfer)
- [CQRS](#cqrs)
- [Event Sourcing](#event-sourcing)
- [Saga Pattern](#saga-pattern)

## Event Notification

*Thin events announcing a fact; consumers fetch details.*

### 🌱 Simple

*Beginner - plain language*

**Event notification** sends a lightweight event saying "something happened" with minimal data (often just an ID). Interested consumers then *call back* to fetch the details they need.

### 📏 Limits

*Governor & platform limits*

- Thin events mean consumers must call back for detail, consuming API requests.
- Callback latency adds to end-to-end processing time.
- The record may have changed again by the time the consumer reads it.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Event Carried State Transfer

*Events carry the full data so consumers need no callback.*

### 🌱 Simple

*Beginner - plain language*

**Event-Carried State Transfer (ECST)** puts the relevant data *inside* the event, so consumers can act and keep their own local copy without calling back to the source.

### 📏 Limits

*Governor & platform limits*

- Fat events are bounded by the 1 MB message limit.
- Payload is a point-in-time snapshot and is stale on arrival.
- Larger payloads consume delivery allocation faster.
- Schema changes affect every consumer immediately.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## CQRS

*Command Query Responsibility Segregation.*

### 🌱 Simple

*Beginner - plain language*

**CQRS** separates the model that **writes** data (commands) from the model that **reads** it (queries) — letting each be optimized independently, often kept in sync via events.

### 📏 Limits

*Governor & platform limits*

- Read models must be rebuilt from events - bounded by the 72-hour retention unless you persist them.
- Eventual consistency must be visible in the UI.
- Read model storage consumes data storage.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Event Sourcing

*Store state as an immutable sequence of events.*

### 🌱 Simple

*Beginner - plain language*

**Event sourcing** stores all changes as an immutable, ordered sequence of events (the source of truth) instead of just the current state — you rebuild current state by replaying the events.

### 📏 Limits

*Governor & platform limits*

- Platform Events are not durable storage - 72 hours only.
- True event sourcing requires persisting events to a custom or Big Object.
- Rebuilding state from millions of events needs Batch Apex and time.
- Big Object indexes are immutable, so design the replay query first.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Saga Pattern

*Managing distributed transactions via compensations.*

### 🌱 Simple

*Beginner - plain language*

A **saga** coordinates a business transaction spanning multiple services as a series of local transactions, using **compensating actions** to undo prior steps if a later step fails — instead of a distributed ACID transaction.

### 📏 Limits

*Governor & platform limits*

- There are no distributed transactions - compensating actions must be written by hand.
- Each step consumes async executions and callouts.
- Compensation can itself fail and needs its own retry and dead-letter path.
- Saga state must be persisted; statics do not survive transactions.

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
