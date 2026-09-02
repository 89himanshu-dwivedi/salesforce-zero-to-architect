[Home](../index.md) / [20 · Real-Time Scenarios (CTA)](index.md) / **Event Driven Scenarios**

# Event Driven Scenarios

3 topics · Series 20: Real-Time Scenarios (CTA)

**Topics on this page**

- [Platform Events Design](#platform-events-design)
- [Kafka Integration](#kafka-integration)
- [CDC Architecture](#cdc-architecture)

## Platform Events Design

*Design a robust Salesforce event-driven backbone.*

### 🌱 Simple

*Beginner - plain language*

**Platform Events design** architects Salesforce's **event bus** — publishing and subscribing to events for decoupled, near-real-time integration and automation within and beyond the org.

### 📏 Limits

*Governor & platform limits*

- Event schemas as contract; publish after-commit vs immediate; Pub/Sub API.
- Replay-based durability (~72h); idempotent consumers; publish/delivery limits.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Kafka Integration

*Bridge Salesforce with an enterprise Kafka event streaming platform.*

### 🌱 Simple

*Beginner - plain language*

**Kafka integration** connects Salesforce with **Apache Kafka** — the enterprise event-streaming backbone — so Salesforce events flow into Kafka topics and external events flow into Salesforce, at high throughput.

### 📏 Limits

*Governor & platform limits*

- Bridge Platform Events/CDC via Pub/Sub API to partitioned Kafka topics.
- MuleSoft/Kafka Connect; idempotency; replay/offset durability; schema registry.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## CDC Architecture

*Stream Salesforce data changes in near real time.*

### 🌱 Simple

*Beginner - plain language*

**Change Data Capture (CDC)** streams **record change events** (create/update/delete/undelete) from Salesforce in near real time, so external systems and data stores stay synchronized without polling.

### 📏 Limits

*Governor & platform limits*

- Per-object change events (fields + header); consume via Pub/Sub API.
- Replay-durable (~72h); idempotent consumers; gap/overflow; vs Platform Events.

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
