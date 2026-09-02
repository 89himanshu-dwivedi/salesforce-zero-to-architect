[Home](../index.md) / [12 · Event Driven Architecture](index.md) / **Enterprise Messaging**

# Enterprise Messaging

8 topics · Series 12: Event Driven Architecture

**Topics on this page**

- [Kafka Topics](#kafka-topics)
- [Kafka Partitions](#kafka-partitions)
- [Consumer Groups](#consumer-groups)
- [Offsets](#offsets)
- [RabbitMQ](#rabbitmq)
- [Azure Event Hub](#azure-event-hub)
- [AWS EventBridge](#aws-eventbridge)
- [Google Pub/Sub](#google-pub-sub)

## Kafka Topics

*Named, partitioned, durable event logs in Kafka.*

### 🌱 Simple

*Beginner - plain language*

A **Kafka topic** is a named category/stream of events — a durable, append-only log that producers write to and consumers read from. Topics are how Kafka organizes event streams.

### 📏 Limits

*Governor & platform limits*

- External to Salesforce - its own retention, partitioning and licensing.
- Salesforce cannot consume Kafka natively; middleware or a connector is required.
- Bridging Kafka to Salesforce still consumes API and callout limits.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Kafka Partitions

*Sub-logs enabling parallelism and ordering.*

### 🌱 Simple

*Beginner - plain language*

A **partition** is a sub-log of a Kafka topic. Splitting a topic into partitions enables parallel consumption and throughput, with ordering guaranteed *within* each partition.

### 📏 Limits

*Governor & platform limits*

- Ordering is guaranteed per partition only, mirroring Platform Event semantics.
- Partition count cannot be reduced after creation.
- Key choice determines skew - a hot key serialises throughput.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Consumer Groups

*Sets of consumers sharing a topic's partitions.*

### 🌱 Simple

*Beginner - plain language*

A **consumer group** is a set of consumers that cooperatively read a topic — Kafka assigns each partition to one consumer in the group, so the work is load-balanced and scalable.

### 📏 Limits

*Governor & platform limits*

- Parallelism is capped by partition count - more consumers than partitions idle.
- Rebalancing pauses consumption.
- Offset management is the consumer's responsibility.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Offsets

*Per-partition position markers for consumers.*

### 🌱 Simple

*Beginner - plain language*

An **offset** is a consumer's position in a partition — the index of the next record to read. Committing offsets lets a consumer resume where it left off after a restart.

### 📏 Limits

*Governor & platform limits*

- Committing offsets before processing risks data loss; after processing risks duplicates.
- Retention policy bounds how far back you can reset.
- Manual offset resets are operationally risky and rarely reversible.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## RabbitMQ

*Mature message broker (AMQP) for queuing and routing.*

### 🌱 Simple

*Beginner - plain language*

**RabbitMQ** is a popular open-source message broker implementing AMQP — it routes messages through exchanges to queues, supporting flexible routing, acknowledgments, and reliable delivery.

### 📏 Limits

*Governor & platform limits*

- External platform with its own queue depth, memory and disk limits.
- Salesforce integration requires middleware or a custom bridge.
- Message TTL and queue limits must be configured explicitly.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Azure Event Hub

*Azure's high-throughput event streaming/ingestion service.*

### 🌱 Simple

*Beginner - plain language*

**Azure Event Hubs** is a managed, high-throughput event ingestion/streaming service — like a cloud-native Kafka — for ingesting millions of events/sec into Azure for processing and analytics.

### 📏 Limits

*Governor & platform limits*

- Throughput units cap ingest and egress rates.
- Retention is configurable but capped (typically 1-7 days on standard tiers).
- Salesforce integration is via HTTP or middleware, consuming callout limits.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## AWS EventBridge

*AWS serverless event bus with routing rules.*

### 🌱 Simple

*Beginner - plain language*

**Amazon EventBridge** is a serverless event bus that ingests events from AWS services, SaaS apps (including Salesforce), and custom sources, then routes them to targets using **rules** — no infrastructure to manage.

### 📏 Limits

*Governor & platform limits*

- Event size limited to 256 KB - smaller than the Salesforce 1 MB limit.
- Rule and target quotas apply per account.
- Salesforce Event Relay has its own configuration and object support limits.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Google Pub/Sub

*GCP's global, serverless messaging service.*

### 🌱 Simple

*Beginner - plain language*

**Google Cloud Pub/Sub** is a fully managed, global messaging service — publishers send messages to **topics** and subscribers receive them via **subscriptions**, with automatic scaling and at-least-once delivery.

### 📏 Limits

*Governor & platform limits*

- Message size limit 10 MB; retention configurable up to 7 days.
- At-least-once delivery, so consumers must be idempotent.
- Salesforce integration requires middleware or custom callouts.

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
