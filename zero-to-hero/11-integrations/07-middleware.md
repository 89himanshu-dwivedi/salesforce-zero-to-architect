[Home](../index.md) / [11 · Integrations](index.md) / **Middleware**

# Middleware

9 topics · Series 11: Integrations

**Topics on this page**

- [MuleSoft API Led Connectivity](#mulesoft-api-led-connectivity)
- [System API](#system-api)
- [Process API](#process-api)
- [Experience API](#experience-api)
- [Dell Boomi](#dell-boomi)
- [Informatica](#informatica)
- [Jitterbit](#jitterbit)
- [Azure Logic Apps](#azure-logic-apps)
- [AWS Integration](#aws-integration)

## MuleSoft API Led Connectivity

*Three-layer API architecture: System, Process, Experience.*

### 🌱 Simple

*Beginner - plain language*

**API-led connectivity** is MuleSoft's pattern of structuring integrations in three reusable layers — **System** (access data), **Process** (orchestrate/business logic), **Experience** (tailor for channels) — for reuse and decoupling.

### 📏 Limits

*Governor & platform limits*

- Adds a separate platform with its own limits, licensing and operational surface.
- Salesforce callout and API limits still apply to the Salesforce leg.
- Latency increases with each API layer - measure end to end.
- Requires its own monitoring, deployment pipeline and skills.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## System API

*The layer that unlocks raw backend system data.*

### 🌱 Simple

*Beginner - plain language*

A **System API** (bottom layer of API-led connectivity) provides direct, reusable access to a backend system's data/functions — abstracting its complexity behind a clean API.

### 📏 Limits

*Governor & platform limits*

- Should expose the source system's model, not the consumer's - resist business logic here.
- Salesforce-side callout limits still apply to whoever calls it.
- Versioning is mandatory; system APIs are the most stable and hardest to change.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Process API

*The layer orchestrating business logic across systems.*

### 🌱 Simple

*Beginner - plain language*

A **Process API** (middle layer) orchestrates and transforms data across multiple System APIs — implementing business logic independent of any specific backend or consumer.

### 📏 Limits

*Governor & platform limits*

- Orchestration across systems means the slowest dependency sets the latency.
- Transaction boundaries do not span systems - compensating actions are required.
- Retries must be idempotent at every downstream hop.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Experience API

*The layer shaping data for specific consumers/channels.*

### 🌱 Simple

*Beginner - plain language*

An **Experience API** (top layer) tailors data from Process APIs for a specific consumer or channel — mobile app, web, partner — returning exactly the shape each needs.

### 📏 Limits

*Governor & platform limits*

- Channel-specific shaping means more APIs to version and maintain.
- Payload size limits of the consuming channel (mobile, browser) apply.
- Caching at this layer is where most performance gains are realised.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Dell Boomi

*Cloud-native iPaaS for integration and data management.*

### 🌱 Simple

*Beginner - plain language*

**Dell Boomi** (Boomi) is a cloud integration platform (iPaaS) for connecting apps and data with low-code visual flows, connectors, and managed runtimes (Atoms).

### 📏 Limits

*Governor & platform limits*

- Separate licensing, connector limits and runtime capacity to manage.
- Salesforce API request limits still apply to the connector's traffic.
- Atom sizing determines throughput - it is not unlimited.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Informatica

*Enterprise data integration, ETL, and cloud iPaaS.*

### 🌱 Simple

*Beginner - plain language*

**Informatica** is an enterprise data-integration leader — known for **ETL/ELT** (PowerCenter), cloud integration (IICS/IDMC), data quality, and MDM — moving and transforming large data volumes.

### 📏 Limits

*Governor & platform limits*

- Bulk API limits apply to its Salesforce operations.
- Separate licensing and infrastructure with its own failure modes.
- Mapping changes require redeployment in its own pipeline.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Jitterbit

*Low-code iPaaS with strong Salesforce focus.*

### 🌱 Simple

*Beginner - plain language*

**Jitterbit** is an iPaaS with low-code integration, API creation, and a strong Salesforce footprint — popular for connecting Salesforce to ERPs and other apps quickly.

### 📏 Limits

*Governor & platform limits*

- Consumes the org's daily API allocation like any client.
- Agent capacity and licensing bound throughput.
- Error handling and retry must be configured explicitly - it is not automatic.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Azure Logic Apps

*Microsoft's serverless cloud integration/workflow service.*

### 🌱 Simple

*Beginner - plain language*

**Azure Logic Apps** is a serverless workflow/integration service on Azure — build automated flows with connectors (including Salesforce) via a visual designer, billed per execution.

### 📏 Limits

*Governor & platform limits*

- Salesforce connector uses standard API limits and counts against the daily allocation.
- Connector throttling and Azure consumption limits apply on top.
- Long-running workflows need durable patterns; HTTP actions have their own timeouts.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## AWS Integration

*Integrating Salesforce with AWS services.*

### 🌱 Simple

*Beginner - plain language*

**AWS integration** connects Salesforce with Amazon Web Services — using services like EventBridge, AppFlow, Lambda, SQS/SNS, and API Gateway to move data and events between Salesforce and AWS.

### 📏 Limits

*Governor & platform limits*

- Named Credentials support AWS Signature V4 natively - do not hand-roll signing.
- Callout body limits (6 MB / 12 MB) apply; use signed S3 URLs for large payloads.
- Lambda cold starts can exceed short callout timeouts.
- Data leaving the org has residency and DPA implications.

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
