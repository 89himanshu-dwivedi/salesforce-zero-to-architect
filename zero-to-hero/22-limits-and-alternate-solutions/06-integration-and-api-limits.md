[Home](../index.md) / [22 · Limits & Alternate Solutions](index.md) / **Integration & API Limits**

# Integration & API Limits

12 topics · Series 22: Limits & Alternate Solutions

**Topics on this page**

- [Daily API Request Limit](#daily-api-request-limit)
- [Concurrent Long API Limit](#concurrent-long-api-limit)
- [Callout Size 6MB Limit](#callout-size-6mb-limit)
- [Callout After DML Error](#callout-after-dml-error)
- [Continuation 3 Parallel Limit](#continuation-3-parallel-limit)
- [Composite API Limits](#composite-api-limits)
- [Bulk API 10000 Limit](#bulk-api-10000-limit)
- [Named Credential Timeout](#named-credential-timeout)
- [Outbound Message Limits](#outbound-message-limits)
- [Webhook Burst Handling](#webhook-burst-handling)
- [Rate Limit 429 Handling](#rate-limit-429-handling)
- [Retry & Backoff Pattern](#retry-and-backoff-pattern)

## Daily API Request Limit

*Org-wide daily API call cap — batch & cache to conserve.*

### 🌱 Simple

*Beginner - plain language*

Every org has a **daily API request limit** (based on edition × licenses, e.g. tens of thousands to millions). Chatty integrations (one call per record) burn through it. Alternate: **use Bulk/Composite APIs, cache reads, and push changes via events instead of polling**.

### 📏 Limits

*Governor & platform limits*

- Edition and licence based; org-wide.
- Composite API: 25 subrequests per call.
- Bulk API operations count differently and far more efficiently.
- Exceeding it blocks all API traffic.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Concurrent Long API Limit

*Limited simultaneous long-running API requests (>20s).*

### 🌱 Simple

*Beginner - plain language*

Salesforce limits how many **API requests running longer than 20 seconds** can execute at once (e.g. 25 concurrent). Slow integration queries eat these slots and block others. Alternate: **make API queries selective/fast, paginate, and use Bulk API for big extracts**.

### 📏 Limits

*Governor & platform limits*

- 25 concurrent API requests over 20 seconds.
- 10 concurrent sync requests over 5 seconds.
- Org-wide, not per user.
- Rejections are immediate, not queued.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Callout Size 6MB Limit

*Request/response capped ~6MB sync / 12MB async — stream/paginate.*

### 🌱 Simple

*Beginner - plain language*

An Apex callout's **request and response are limited to ~6MB (sync) / 12MB (async)**. Sending or receiving a huge payload fails. Alternate: **paginate the API, compress, or stream the file to an external store** rather than passing it through Apex memory.

### 📏 Limits

*Governor & platform limits*

- 6 MB sync / 12 MB async request and response.
- Counts toward heap.
- Base64 inflates by ~33%.
- Exceeding it throws before any processing.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Callout After DML Error

*"You have uncommitted work pending" — order DML and callouts.*

### 🌱 Simple

*Beginner - plain language*

You **cannot make a callout after performing DML** in the same transaction — Salesforce throws `You have uncommitted work pending`. The alternate is to **call out first, then DML**, or move the callout to an **async** method that runs after the DML commits.

### 📏 Limits

*Governor & platform limits*

- No callouts after uncommitted DML in the same transaction.
- Applies to all DML, including from flows.
- Queueable needs `Database.AllowsCallouts`.
- Callout before DML in the same transaction is allowed.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Continuation 3 Parallel Limit

*Continuations allow 3 parallel callouts — for long UI calls.*

### 🌱 Simple

*Beginner - plain language*

A **Continuation** lets a Visualforce/LWC make long-running callouts *without* holding a server thread, but allows at most **3 parallel callouts** per continuation. Alternate for more: **chain continuations or batch the work server-side**.

### 📏 Limits

*Governor & platform limits*

- 3 parallel callouts per Continuation.
- 120 second total timeout.
- Supported in Visualforce and Aura, not LWC.
- Response size limits still apply.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Composite API Limits

*Composite bundles 25 subrequests — collapse many calls into one.*

### 🌱 Simple

*Beginner - plain language*

The **Composite API** runs up to **25 subrequests in one call** (and Composite Graph handles related records with rollback). It's the answer to "too many API calls." Alternate to chatty REST: **bundle dependent operations into one composite request**.

### 📏 Limits

*Governor & platform limits*

- 25 subrequests per composite call.
- Composite Graph: 500 records total.
- Governor limits apply per subrequest.
- Counts as one API request against the daily limit.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Bulk API 10000 Limit

*Bulk API 2.0 for big loads — 10k/batch, async, retry-friendly.*

### 🌱 Simple

*Beginner - plain language*

**Bulk API 2.0** is built for large data loads: it ingests data asynchronously in batches (historically **10,000 records/batch**) and is the right alternate to REST for big imports/exports. Use it for >a few thousand records; use serial mode to avoid locking on skew.

### 📏 Limits

*Governor & platform limits*

- Bulk 1.0: 10,000 records or 10 MB per batch; 10,000 batches per 24h.
- Bulk 2.0: 150 million records per 24h.
- 10-minute processing timeout per batch.
- Triggers fire in 200-record chunks regardless.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Named Credential Timeout

*Named Credentials centralize auth/endpoint + timeout config.*

### 🌱 Simple

*Beginner - plain language*

**Named Credentials** store the endpoint and auth so Apex callouts use `callout:MyApi` without hardcoding secrets — and they let you manage **timeouts and certificates** centrally. The alternate to scattered HttpRequest config and Remote Site Settings is one Named Credential.

### 📏 Limits

*Governor & platform limits*

- Default 10s, max 120s.
- 120s cumulative per transaction.
- Principal access must be granted via permission set.
- Named Credential removes the need for a Remote Site Setting.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Outbound Message Limits

*Workflow outbound messages: queueing, retries, ordering caveats.*

### 🌱 Simple

*Beginner - plain language*

**Outbound Messages** (declarative SOAP notifications) are simple but limited: they **queue with retries**, can arrive **out of order** or duplicated, and have batch-size caps. For reliable modern integration, the alternate is **Platform Events or a custom callout**.

### 📏 Limits

*Governor & platform limits*

- Retries for 24 hours then discards.
- Pending message cap per org.
- SOAP only; sends a session Id.
- No response processing beyond an ack.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Webhook Burst Handling

*Inbound webhook spikes can exceed limits — buffer & async.*

### 🌱 Simple

*Beginner - plain language*

When an external system fires **many webhooks at once** into Salesforce (an Apex REST endpoint or Site), a burst can exceed **concurrent request / API limits** and time out. Alternate: **ack fast, enqueue the work async, and process from a buffer** (Platform Events / a staging object).

### 📏 Limits

*Governor & platform limits*

- 6 MB inbound body limit.
- Guest users have no standard API access.
- Counts toward daily API requests.
- No native queue or replay - you build the staging layer.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Rate Limit 429 Handling

*External API returns 429 — back off, don't hammer.*

### 🌱 Simple

*Beginner - plain language*

When you call an external API too fast, it returns **HTTP 429 Too Many Requests**. Retrying immediately makes it worse. The alternate is **exponential backoff with jitter**, honoring any `Retry-After` header, ideally in an async retry job.

### 📏 Limits

*Governor & platform limits*

- Retry delay is capped by the 120s transaction callout budget - use async delays instead.
- Queueable delay is specified in minutes.
- 429s still consume a callout from your 100.
- Vendor limits are outside your control.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Retry & Backoff Pattern

*Reliable integration retries without storms or duplicates.*

### 🌱 Simple

*Beginner - plain language*

Transient failures (timeouts, 5xx, 429) are normal in integration. A robust **retry pattern** uses **exponential backoff + jitter, idempotency keys, a max-attempt cap, and a dead-letter queue** for permanent failures — implemented async so it never blocks a transaction.

### 📏 Limits

*Governor & platform limits*

- Delays must be async - a transaction cannot sleep.
- Queueable delay granularity is minutes.
- Each attempt consumes a callout and async execution.
- Dead-letter storage consumes data storage.

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
