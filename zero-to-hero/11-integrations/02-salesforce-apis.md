[Home](../index.md) / [11 · Integrations](index.md) / **Salesforce APIs**

# Salesforce APIs

14 topics · Series 11: Integrations

**Topics on this page**

- [REST API CRUD](#rest-api-crud)
- [Query API](#query-api)
- [Composite Requests](#composite-requests)
- [SOAP Enterprise WSDL](#soap-enterprise-wsdl)
- [SOAP Partner WSDL](#soap-partner-wsdl)
- [Bulk API 1.0](#bulk-api-1-0)
- [Bulk API 2.0](#bulk-api-2-0)
- [Parallel Mode](#parallel-mode)
- [Serial Mode](#serial-mode)
- [Metadata API](#metadata-api)
- [Tooling API](#tooling-api)
- [GraphQL API](#graphql-api)
- [Pub/Sub API](#pub-sub-api)
- [Streaming API](#streaming-api)

## REST API CRUD

*Create/read/update/delete records via the REST API.*

### 🌱 Simple

*Beginner - plain language*

The **REST API** exposes records over HTTP — POST to create, GET to read, PATCH to update, DELETE to delete — using JSON and the `/services/data/vXX.X/sobjects/` endpoints.

### 📏 Limits

*Governor & platform limits*

- Counts against the org-wide daily API request limit.
- Query results page at 2,000 records - follow `nextRecordsUrl`.
- Request body capped at 6 MB (higher for specific endpoints).
- Runs in the calling user's context - CRUD, FLS and sharing all apply.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Query API

*Retrieving records with SOQL over REST (query/queryMore).*

### 🌱 Simple

*Beginner - plain language*

The **Query API** runs SOQL via REST: `GET /query?q=SELECT...` returns matching records as JSON, with pagination through `nextRecordsUrl`.

### 📏 Limits

*Governor & platform limits*

- 2,000 records per page by default; use `queryMore` / `nextRecordsUrl`.
- Exposes your schema to the consumer - a versioning liability.
- Subject to query selectivity rules on large objects.
- Each page is a separate API request against the daily limit.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Composite Requests

*Bundling multiple API calls into one round-trip.*

### 🌱 Simple

*Beginner - plain language*

**Composite requests** bundle several REST operations into a single HTTP call — reducing round-trips and letting later sub-requests reference earlier results.

### 📏 Limits

*Governor & platform limits*

- Maximum 25 subrequests per composite call.
- Composite Graph handles up to 500 records with dependency ordering.
- Governor limits apply per subrequest, not per composite call.
- The whole composite counts as one API request - the main benefit.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## SOAP Enterprise WSDL

*Strongly-typed SOAP API bound to one org's schema.*

### 🌱 Simple

*Beginner - plain language*

The **Enterprise WSDL** is a strongly-typed SOAP contract generated for a *specific* org — its objects/fields become typed classes, ideal for a single-org integration.

### 📏 Limits

*Governor & platform limits*

- Strongly typed and org-specific - it must be regenerated whenever the schema changes.
- Generated Apex or client stubs count toward the 6 MB org Apex limit if stored in Salesforce.
- Same daily API request limit as REST.
- Large WSDLs frequently fail WSDL2Apex generation.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## SOAP Partner WSDL

*Loosely-typed SOAP API portable across any org.*

### 🌱 Simple

*Beginner - plain language*

The **Partner WSDL** is a loosely-typed SOAP contract that works with *any* Salesforce org — fields are accessed generically, so no regeneration is needed when schemas differ.

### 📏 Limits

*Governor & platform limits*

- Loosely typed - fields are name/value pairs, so there is no compile-time safety.
- Works across orgs without regeneration, which is its main advantage.
- Same API request limits as any other API.
- More verbose payloads mean more bandwidth and parsing cost.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Bulk API 1.0

*Asynchronous batch processing of large data via jobs/batches.*

### 🌱 Simple

*Beginner - plain language*

**Bulk API 1.0** processes large data volumes asynchronously — you create a job, add batches of records (CSV/XML/JSON), and poll for results — built for tens of thousands to millions of records.

### 📏 Limits

*Governor & platform limits*

- 10,000 records or 10 MB per batch; 10,000 batches per 24 hours.
- 10-minute processing timeout per batch.
- Parallel mode maximises throughput but causes lock contention on skewed data.
- You manage job and batch lifecycle manually.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Bulk API 2.0

*Simplified, auto-chunking async bulk loading.*

### 🌱 Simple

*Beginner - plain language*

**Bulk API 2.0** simplifies large loads: you submit one job with all your data (CSV) and Salesforce automatically splits it into batches, processes them, and provides success/failed results.

### 📏 Limits

*Governor & platform limits*

- 150 million records per 24-hour rolling window.
- Chunking, retries and job management are handled by the platform.
- CSV upload capped at 150 MB per job.
- Triggers still fire in 200-record chunks.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Parallel Mode

*Processing bulk batches concurrently for max throughput.*

### 🌱 Simple

*Beginner - plain language*

**Parallel mode** processes bulk batches *concurrently* for maximum throughput — the default for Bulk API — but can cause record-lock contention on shared parents.

### 📏 Limits

*Governor & platform limits*

- Default for Bulk API 1.0 and the fastest option.
- Causes `UNABLE_TO_LOCK_ROW` when batches touch the same parent.
- Sorting input by parent Id is the standard mitigation.
- Retry logic must handle partial batch failures.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Serial Mode

*Processing bulk batches sequentially to avoid locks.*

### 🌱 Simple

*Beginner - plain language*

**Serial mode** processes bulk batches *one at a time* — slower but avoids the lock contention that parallel processing can cause on shared parent records.

### 📏 Limits

*Governor & platform limits*

- Batches process one at a time - throughput drops significantly.
- Removes lock contention, which is why it is used for skewed data.
- Still bounded by the 10-minute per-batch timeout.
- Must be set at job creation; it cannot be changed mid-job.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Metadata API

*Programmatically deploy and retrieve org configuration.*

### 🌱 Simple

*Beginner - plain language*

The **Metadata API** manages *configuration* (not data) — deploying and retrieving components like objects, fields, flows, and profiles — powering CI/CD and migrations.

### 📏 Limits

*Governor & platform limits*

- Deploy limit: 10,000 files or 39 MB per deployment.
- Deploys per 24-hour rolling window are capped.
- Profile deployments are partial - only permissions for packaged components.
- Custom Metadata and Custom Setting *records* are data, not covered by default.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Tooling API

*Fine-grained metadata access for developer tooling.*

### 🌱 Simple

*Beginner - plain language*

The **Tooling API** gives fine-grained, programmatic access to metadata for building developer tools — editing Apex, querying symbol tables, running tests, managing debug logs — via REST or SOAP.

### 📏 Limits

*Governor & platform limits*

- Designed for development tooling, not bulk data operations.
- Counts toward the daily API request limit.
- Some objects are read-only or unavailable in production.
- Anonymous Apex execution via Tooling API commits - there is no automatic rollback.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## GraphQL API

*Query exactly the data you need in one shaped request.*

### 🌱 Simple

*Beginner - plain language*

The **GraphQL API** lets clients request exactly the fields and related records they need in a single, shaped query — reducing over-fetching and round-trips compared to REST.

### 📏 Limits

*Governor & platform limits*

- Supports a subset of objects and operations compared with REST.
- Query depth and complexity are limited to protect the platform.
- Counts toward the daily API request limit.
- Aggregations and some relationship traversals are not supported.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Pub/Sub API

*Efficient gRPC API to publish and subscribe to events.*

### 🌱 Simple

*Beginner - plain language*

The **Pub/Sub API** is Salesforce's modern gRPC-based API for publishing and subscribing to events (Platform Events, CDC) efficiently with binary Avro payloads and flow control.

### 📏 Limits

*Governor & platform limits*

- gRPC-based; requires HTTP/2 and a supported client library.
- Event retention 72 hours for high-volume events.
- Daily publish and delivery allocations are edition-based.
- Flow control means a slow consumer can fall behind and lose events past retention.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Streaming API

*CometD-based push of events to subscribers (legacy).*

### 🌱 Simple

*Beginner - plain language*

The **Streaming API** pushes events to subscribers over CometD (long-polling/Bayeux) — used by PushTopics, generic streaming, Platform Events, and CDC for near-real-time notifications.

### 📏 Limits

*Governor & platform limits*

- Concurrent client and daily delivery limits vary by edition.
- PushTopic queries are restricted (no aggregates, limited joins).
- Retention 24-72 hours depending on the channel type.
- Being superseded by the Pub/Sub API.

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
