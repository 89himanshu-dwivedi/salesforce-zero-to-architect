[Home](../index.md) / [18 · AI in Salesforce](index.md) / **AI Governance**

# AI Governance

4 topics · Series 18: AI in Salesforce

**Topics on this page**

- [Trust Layer](#trust-layer)
- [Data Masking](#data-masking)
- [Auditability](#auditability)
- [Compliance](#compliance)

## Trust Layer

*Salesforce's secure gateway for generative AI.*

### 🌱 Simple

*Beginner - plain language*

The **Einstein Trust Layer** is the security and governance layer that sits **between Salesforce and the LLM** — providing data masking, grounding, toxicity scoring, audit, and zero retention so generative AI is enterprise-safe.

### 📏 Limits

*Governor & platform limits*

- Pipeline: ground→mask→LLM(zero-retain)→score→demask→audit.
- Vendor-agnostic (incl. BYOM); consistent across Prompt Builder/Agentforce.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Data Masking

*Redact sensitive data before it reaches the LLM.*

### 🌱 Simple

*Beginner - plain language*

**Data masking** in the Trust Layer **redacts sensitive data** (PII like names, emails, SSNs) from the prompt **before** it's sent to the LLM, then **restores** it in the response — so the model vendor never sees raw sensitive data.

### 📏 Limits

*Governor & platform limits*

- Detect→placeholder→LLM→demask; configurable entity types; consistent.
- Privacy vs context tradeoff; layer with minimization + zero-retention.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Auditability

*Log AI interactions for review and compliance.*

### 🌱 Simple

*Beginner - plain language*

**Auditability** means every AI interaction — prompts, responses, grounding, masking, toxicity scores — is **logged**, so you can review, troubleshoot, and prove compliance.

### 📏 Limits

*Governor & platform limits*

- Logs prompts/responses/grounding/masking/toxicity/model in Data Cloud.
- Compliance/security/quality uses; secure (masked) logs; retention + alerting.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Compliance

*Meet legal and regulatory requirements for AI.*

### 🌱 Simple

*Beginner - plain language*

**Compliance** ensures AI use meets **legal and regulatory requirements** — GDPR, HIPAA, industry rules, and emerging AI regulations — covering privacy, fairness, transparency, and accountability.

### 📏 Limits

*Governor & platform limits*

- Privacy (consent/masking/residency), fairness, transparency, oversight, audit.
- Maps to GDPR/HIPAA/EU AI Act; Trust Layer + zero-retention DPAs evidence it.

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
