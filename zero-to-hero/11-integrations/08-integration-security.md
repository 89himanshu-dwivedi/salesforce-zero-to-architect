[Home](../index.md) / [11 · Integrations](index.md) / **Integration Security**

# Integration Security

7 topics · Series 11: Integrations

**Topics on this page**

- [TLS](#tls)
- [mTLS](#mtls)
- [Certificates](#certificates)
- [Encryption](#encryption)
- [JWT](#jwt)
- [API Keys](#api-keys)
- [Secrets](#secrets)

## TLS

*Transport Layer Security — encryption in transit.*

### 🌱 Simple

*Beginner - plain language*

**TLS** (Transport Layer Security, the successor to SSL) encrypts data in transit between client and server (HTTPS), protecting it from eavesdropping and tampering, and authenticates the server via its certificate.

### 📏 Limits

*Governor & platform limits*

- Salesforce requires TLS 1.2 or higher for inbound and outbound traffic.
- Endpoints with expired or untrusted certificates fail the callout outright.
- Self-signed remote certificates must be uploaded to the org's certificate store.
- Cipher suite support changes with Salesforce releases - watch release notes.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## mTLS

*Mutual TLS — both client and server authenticate.*

### 🌱 Simple

*Beginner - plain language*

**mTLS** (mutual TLS) extends TLS so *both* sides present certificates — the client authenticates to the server *and* the server to the client — providing strong two-way authentication.

### 📏 Limits

*Governor & platform limits*

- Requires a client certificate configured on the External Credential.
- Certificate expiry breaks the integration with no warning.
- Not supported by all authentication protocols or by legacy Named Credentials.
- Certificate rotation needs coordination on both sides.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Certificates

*Using certificates for app authentication and signing.*

### 🌱 Simple

*Beginner - plain language*

Connected Apps use **certificates** for stronger auth — e.g., the public certificate uploaded for JWT Bearer flow, so Salesforce verifies JWTs signed with the matching private key.

### 📏 Limits

*Governor & platform limits*

- Self-signed certificates have a maximum validity (typically 1-2 years) and must be rotated.
- Expiry breaks JWT flows, SSO and mTLS simultaneously and without warning.
- Maximum key sizes and algorithms are constrained by the platform.
- Certificates cannot be exported once created - plan backups of the CSR process, not the key.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Encryption

*Protecting data in transit and at rest.*

### 🌱 Simple

*Beginner - plain language*

**Encryption** converts data into unreadable ciphertext that only authorized parties (with the key) can decrypt — protecting data **in transit** (TLS) and **at rest** (stored data), and sometimes at the field/payload level.

### 📏 Limits

*Governor & platform limits*

- Shield deterministic encryption limits filtering and sorting; probabilistic prevents it entirely.
- Encrypted fields cannot be used in formulas, roll-ups or many filters.
- Encrypted fields still require the "View Encrypted Data" permission to be readable.
- Key rotation is an operational process, not a one-off setting.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## JWT

*JSON Web Token — signed, self-contained claims.*

### 🌱 Simple

*Beginner - plain language*

A **JWT** (JSON Web Token) is a compact, signed token carrying claims (identity, scopes, expiry) as JSON. Its signature lets a receiver verify authenticity without a database lookup.

### 📏 Limits

*Governor & platform limits*

- Signature, issuer, audience and expiry must all be validated by the receiver.
- Clock skew between systems causes intermittent validation failures.
- Signing certificates expire and must be rotated ahead of time.
- JWTs cannot be revoked before expiry - keep lifetimes short.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## API Keys

*Simple shared-secret tokens identifying a caller.*

### 🌱 Simple

*Beginner - plain language*

An **API key** is a simple secret string a client sends with each request (header/query) to identify itself to an API. It's easy but weak — it only identifies, doesn't strongly authenticate or authorize per-user.

### 📏 Limits

*Governor & platform limits*

- Static credentials with no expiry unless the vendor enforces one.
- Cannot be stored safely in LWC - anything in the browser is public.
- Store in External Credentials, never in Custom Settings or code.
- Rotation requires coordination and a documented owner.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Secrets

*Managing credentials, keys, and tokens securely.*

### 🌱 Simple

*Beginner - plain language*

**Secrets** are sensitive values — passwords, API keys, tokens, certificate private keys, client secrets — that must never be hardcoded and must be stored, accessed, and rotated securely.

### 📏 Limits

*Governor & platform limits*

- Anyone with Modify All Data can read Custom Setting and Custom Metadata values.
- Named/External Credential secrets are not exportable and do not deploy.
- Secrets in debug logs persist for 7 days and are visible to anyone with log access.
- Every secret needs a documented owner, rotation schedule and expiry alert.

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
