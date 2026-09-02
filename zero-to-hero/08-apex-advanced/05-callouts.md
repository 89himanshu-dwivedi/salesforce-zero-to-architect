[Home](../index.md) / [08 · Apex Advanced](index.md) / **Callouts**

# Callouts

5 topics · Series 8: Apex Advanced

**Topics on this page**

- [REST Callouts](#rest-callouts)
- [SOAP Callouts](#soap-callouts)
- [Named Credentials](#named-credentials)
- [External Credentials](#external-credentials)
- [Continuation](#continuation)

## REST Callouts

*Calling external REST APIs with HttpRequest/HttpResponse.*

### 🌱 Simple

*Beginner - plain language*

**REST callouts** use `Http`, `HttpRequest`, and `HttpResponse` to call external REST APIs. Set the endpoint, method (GET/POST/...), headers, and body, then `new Http().send(req)` and read the response.

### 📏 Limits

*Governor & platform limits*

- 100 callouts per transaction; 120s cumulative; max 120s per callout (10s default).
- Request and response bodies capped at 6 MB sync / 12 MB async.
- Blocked entirely after uncommitted DML in the same transaction.
- Endpoint must be a Named Credential or covered by a Remote Site Setting.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## SOAP Callouts

*Consuming SOAP web services via WSDL-generated Apex classes.*

### 🌱 Simple

*Beginner - plain language*

**SOAP callouts** consume SOAP web services. Import the **WSDL** in Setup to auto-generate Apex stub classes, then call the generated methods like normal Apex.

### 📏 Limits

*Governor & platform limits*

- WSDL2Apex-generated classes count toward the 6 MB org Apex character limit.
- Large or complex WSDLs frequently fail to generate and need manual editing.
- Same 100-callout, 120s and 6 MB body limits as REST.
- Generated stubs are brittle - a vendor schema change requires regeneration and redeploy.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Named Credentials

*Declarative endpoint + auth management for callouts.*

### 🌱 Simple

*Beginner - plain language*

**Named Credentials** store an external endpoint **and its authentication** declaratively. Reference one in Apex as `callout:MyCred/path` — no hardcoded URLs or secrets.

### 📏 Limits

*Governor & platform limits*

- Callers need External Credential Principal access via a permission set or every callout throws.
- Timeout is still set per request - the credential does not control it.
- Secrets never deploy between orgs; they are entered per org by an admin.
- Removes the need for a Remote Site Setting.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## External Credentials

*The auth half of the modern credential model, with permission-set principals.*

### 🌱 Simple

*Beginner - plain language*

**External Credentials** hold the **authentication** details (protocol, principals) for the new Named Credential model. A Named Credential references an External Credential for its auth.

### 📏 Limits

*Governor & platform limits*

- Principal access must be granted through a permission set - profiles alone are not enough.
- Custom headers and formula-injected values are evaluated per callout.
- OAuth refresh is handled by the platform; your Apex never sees the token.
- Legacy Named Credentials keep working but new auth features ship only here.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Continuation

*Long-running async callouts for synchronous user-facing requests.*

### 🌱 Simple

*Beginner - plain language*

**Continuation** enables **long-running callouts** (up to 120s) from a synchronous Visualforce/Aura/LWC context without blocking the user's request thread — the response is processed in a callback.

### 📏 Limits

*Governor & platform limits*

- Maximum 3 parallel callouts per Continuation; 120s total timeout.
- Supported in Visualforce and Aura only - not in LWC.
- Does not count toward the concurrent long-running request limit while waiting.
- Response size limits still apply per callout.

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
