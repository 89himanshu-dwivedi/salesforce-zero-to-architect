[Home](../index.md) / [05 · Developer Foundations](index.md) / **APIs**

# APIs

6 topics · Series 5: Developer Foundations

**Topics on this page**

- [REST API](#rest-api)
- [SOAP API](#soap-api)
- [Bulk API](#bulk-api)
- [Metadata API](#metadata-api)
- [Tooling API](#tooling-api)
- [Composite API](#composite-api)

## REST API

*The lightweight, JSON-based API for record CRUD and most app integrations.*

### 🌱 Simple

*Beginner - plain language*

The **REST API** lets external apps create, read, update, and delete Salesforce records over HTTP using **JSON**. It's the most common API for web/mobile integrations because it's simple and stateless.

### 📏 Limits

*Governor & platform limits*

- Counts toward 24-hour API request limit.
- Per-call payload/record limits (use Bulk for large volume).
- Token expiry / session limits.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## SOAP API

*The XML/WSDL-based API for strongly-typed, enterprise integrations and legacy systems.*

### 🌱 Simple

*Beginner - plain language*

The **SOAP API** is the older, **XML-based** API with a formal **WSDL** contract. It does the same CRUD as REST but with strict typing — common in enterprise/middleware and legacy integrations.

### 📏 Limits

*Governor & platform limits*

- Shares 24h API limits.
- Enterprise WSDL coupled to org schema.
- Verbose XML; not ideal for high volume (use Bulk).

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Bulk API

*The asynchronous, high-throughput API for loading/extracting large data volumes.*

### 🌱 Simple

*Beginner - plain language*

The **Bulk API** is built for **large data volumes** — thousands to millions of records. You submit data in **batches**, Salesforce processes them **asynchronously** in parallel, and you poll for results.

### 📏 Limits

*Governor & platform limits*

- Async, batch/job-based; separate large daily batch limits.
- Parallel processing risks lock contention.
- Automation still fires per record unless managed.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Metadata API

*The programmatic API for deploying/retrieving metadata — the engine behind real CI/CD.*

### 🌱 Simple

*Beginner - plain language*

The **Metadata API** lets tools (the CLI, ANT, CI systems) **retrieve** and **deploy** metadata programmatically — no clicking. It's what powers automated, repeatable deployments.

### 📏 Limits

*Governor & platform limits*

- Asynchronous (poll job status).
- Some metadata types unsupported (use Tooling API).
- File/component size and concurrency limits.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Tooling API

*The API for building developer tools — manipulating code, metadata, and debug artifacts.*

### 🌱 Simple

*Beginner - plain language*

The **Tooling API** is for **developer tooling**: it can read/write Apex classes, triggers, components, run tests, fetch **code coverage**, manage debug logs, and access metadata at a fine-grained level — the API behind IDEs and CI tools.

### 📏 Limits

*Governor & platform limits*

- Granular, not bulk-deploy oriented.
- Shares API request limits.
- Some objects read-only or context-specific.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Composite API

*A REST resource that bundles multiple calls into one request to cut round-trips.*

### 🌱 Simple

*Beginner - plain language*

The **Composite API** lets you send **multiple REST operations in a single HTTP request** — and even reference the output of one sub-request in the next. It reduces network round-trips and API call counts.

### 📏 Limits

*Governor & platform limits*

- 25 sub-requests (Composite); 500 records (Graph).
- Payload size limits.
- Counts as fewer API calls than separate requests.

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
