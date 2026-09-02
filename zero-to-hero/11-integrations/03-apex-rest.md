[Home](../index.md) / [11 · Integrations](index.md) / **Apex REST**

# Apex REST

5 topics · Series 11: Integrations

**Topics on this page**

- [@RestResource](#restresource)
- [@HttpGet @HttpPost @HttpPut @HttpDelete @HttpPatch](#httpget-httppost-httpput-httpdelete-httppatch)
- [JSON.serialize / deserialize](#json-serialize-deserialize)
- [HTTP Status Codes](#http-status-codes)
- [Custom Error Responses](#custom-error-responses)

## @RestResource

*Exposing a custom REST endpoint via an Apex class.*

### 🌱 Simple

*Beginner - plain language*

**@RestResource** marks an Apex class as a custom REST web service mapped to a URL — e.g., `@RestResource(urlMapping='/Orders/*')` exposes `/services/apexrest/Orders/`.

### 📏 Limits

*Governor & platform limits*

- Class and all exposed methods must be `global`.
- One method per HTTP verb per class.
- Request body maximum 6 MB; response bounded by heap.
- Runs in the calling user's context and counts toward daily API requests.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## @HttpGet @HttpPost @HttpPut @HttpDelete @HttpPatch

*Mapping Apex REST methods to HTTP verbs.*

### 🌱 Simple

*Beginner - plain language*

These annotations map Apex methods to HTTP verbs in an `@RestResource` class: **@HttpGet** (read), **@HttpPost** (create), **@HttpPut** (replace), **@HttpPatch** (partial update), **@HttpDelete** (delete).

### 📏 Limits

*Governor & platform limits*

- Only one method per annotation per class.
- Methods must be `global static`.
- URL path parameters come from `RestContext.request.requestURI` - there is no routing framework.
- Standard Apex governor limits apply per request.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## JSON.serialize / deserialize

*Converting between Apex objects and JSON.*

### 🌱 Simple

*Beginner - plain language*

**JSON.serialize** turns an Apex object into a JSON string; **JSON.deserialize** parses JSON back into typed Apex objects — the core of REST request/response handling.

### 📏 Limits

*Governor & platform limits*

- Serialisation is CPU-intensive on large structures and counts toward the 10s budget.
- Deserialised objects count toward the 6 MB / 12 MB heap.
- `deserializeStrict` throws on unknown fields - vendor additions break you.
- Reserved Apex words cannot be used as field names; use untyped deserialisation or a mapping layer.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## HTTP Status Codes

*Using correct status codes in API responses.*

### 🌱 Simple

*Beginner - plain language*

**HTTP status codes** tell callers the outcome: 2xx success, 4xx client error, 5xx server error. Returning the right code is essential for correct client behavior and retries.

### 📏 Limits

*Governor & platform limits*

- Set explicitly via `RestContext.response.statusCode`; the default is 200 even on logical failure.
- Uncaught Apex exceptions return 500 with a stack trace unless handled.
- Only 408, 429 and 5xx are worth retrying.
- Salesforce imposes no cap on which codes you may return.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Custom Error Responses

*Returning structured, actionable error payloads.*

### 🌱 Simple

*Beginner - plain language*

**Custom error responses** return a structured body (code, message, details) alongside the HTTP status — so clients can programmatically understand and handle failures.

### 📏 Limits

*Governor & platform limits*

- Response size is bounded by heap (6 MB sync).
- Error bodies must be built manually - there is no standard error envelope.
- Stack traces in responses leak implementation detail; strip them in production.
- Partial-success semantics require a per-item result array you design yourself.

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
