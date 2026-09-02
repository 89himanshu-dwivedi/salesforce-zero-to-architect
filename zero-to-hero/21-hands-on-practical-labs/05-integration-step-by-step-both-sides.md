[Home](../index.md) / [21 · Hands-On Practical Labs](index.md) / **Integration Step-by-Step (Both Sides)**

# Integration Step-by-Step (Both Sides)

5 topics · Series 21: Hands-On Practical Labs

**Topics on this page**

- [Inbound REST (External to SF)](#inbound-rest-external-to-sf)
- [Outbound Callout (SF to External)](#outbound-callout-sf-to-external)
- [OAuth 2.0 Setup Both Sides](#oauth-2-0-setup-both-sides)
- [Webhook Into Salesforce](#webhook-into-salesforce)
- [Platform Event to External](#platform-event-to-external)

## Inbound REST (External to SF)

*External system calls Salesforce to read/write data — full both-side recipe.*

### 🌱 Simple

*Beginner - plain language*

**Inbound** = the external system **initiates** the call into Salesforce. You either expose Salesforce's **standard REST API** (CRUD on objects) or build a **custom Apex REST endpoint** for a tailored contract. The caller authenticates via a Connected App and sends a bearer token.

### 📏 Limits

*Governor & platform limits*

- Request body max **6 MB**; response also limited by heap.
- Class and methods must be `global`; annotations are per HTTP verb, one method each.
- Runs in the calling user's context - CRUD/FLS/sharing all apply.
- Counts against the org's daily API request limit.
- Standard Apex governor limits apply per request - a bulk payload of 10,000 rows will hit DML row limits.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Outbound Callout (SF to External)

*Salesforce initiates a call to an external API — both-side recipe.*

### 🌱 Simple

*Beginner - plain language*

**Outbound** = Salesforce **initiates** the HTTP call to an external API (REST/SOAP). Because you can't call out synchronously during a trigger/DML, outbound is normally **asynchronous** (Queueable/@future/Platform Event) and uses a **Named Credential** for the endpoint + auth.

### 📏 Limits

*Governor & platform limits*

- **100 callouts** / **120s** total callout time per transaction.
- Async Apex daily limit shared across future, batch, queueable, scheduled.
- Only **1** Queueable can be enqueued from within a running Queueable.
- Concurrent long-running requests (>5s) capped at **10**.
- Platform Event publish: high-volume events have daily allocations by edition.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## OAuth 2.0 Setup Both Sides

*The handshake that lets two systems trust each other — step by step.*

### 🌱 Simple

*Beginner - plain language*

**OAuth 2.0** is how two systems authenticate **without sharing passwords**. One side registers an app (gets Client ID/Secret), proves identity, and receives an **access token** used as `Authorization: Bearer ...`. The right **flow** depends on whether a user is present.

### 📏 Limits

*Governor & platform limits*

- Access token lifetime = profile session timeout (default 2 hours).
- Max **5 active access tokens** per user per Connected App.
- Refresh token policy is per Connected App - "immediately expire" silently kills integrations.
- Connected App changes take up to **10 minutes** to propagate.
- JWT requires the certificate uploaded to the Connected App and the user pre-authorised.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Webhook Into Salesforce

*Let an external event push into Salesforce in near real-time.*

### 🌱 Simple

*Beginner - plain language*

A **webhook** is when an external system **pushes** an HTTP request to *you* the instant something happens (a payment succeeds, a shipment ships) — instead of you polling them. Salesforce receives it on an **Apex REST endpoint** (or a Site/Experience endpoint for unauthenticated webhooks).

### 📏 Limits

*Governor & platform limits*

- Inbound request body max **6 MB**.
- Guest user has no API access - the endpoint must be a Site-hosted Apex REST or Visualforce, not `/services/data`.
- Counts against the daily API request limit unless served through a Site guest context.
- Apex governor limits apply per inbound request - keep the synchronous path tiny.
- No built-in queue or replay in Salesforce; you must build the staging object yourself.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Platform Event to External

*Event-driven outbound: publish once, external subscribers react.*

### 🌱 Simple

*Beginner - plain language*

**Platform Events** let Salesforce **publish an event** ("OrderPlaced") and any number of subscribers — including external systems via the **Pub/Sub API** or **CometD** — react in near real-time. It decouples Salesforce from consumers: you publish, you don't care who's listening.

### 📏 Limits

*Governor & platform limits*

- Event retention: **72 hours** (High Volume Platform Events).
- Daily publish and delivery allocations vary by edition and licence - check "Platform Events" in Company Information.
- Event message max **1 MB**.
- Replay Id is only valid within the retention window; older resubscription fails.
- Ordering is guaranteed per publisher, not globally across publishers.

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
