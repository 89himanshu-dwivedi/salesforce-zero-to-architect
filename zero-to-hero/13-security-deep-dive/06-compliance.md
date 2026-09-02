[Home](../index.md) / [13 · Security Deep Dive](index.md) / **Compliance**

# Compliance

5 topics · Series 13: Security Deep Dive

**Topics on this page**

- [GDPR](#gdpr)
- [HIPAA](#hipaa)
- [SOC2](#soc2)
- [ISO 27001](#iso-27001)
- [PCI DSS](#pci-dss)

## GDPR

*EU data protection: consent, rights, minimization.*

### 🌱 Simple

*Beginner - plain language*

**GDPR** (General Data Protection Regulation) is the EU law governing **personal data** — requiring lawful basis (consent), data subject rights (access, erasure, portability), and protections like minimization and breach notification.

### 📏 Limits

*Governor & platform limits*

- Right to erasure conflicts with audit retention - both must be designed for.
- Deleted records persist in the Recycle Bin for 15 days and in backups longer.
- Data residency may require Hyperforce region selection at provisioning time.
- Processor obligations extend to every integrated third party.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## HIPAA

*US health data privacy and security rules.*

### 🌱 Simple

*Beginner - plain language*

**HIPAA** governs **protected health information (PHI)** in the US — requiring privacy, security safeguards (administrative, physical, technical), and breach notification for healthcare data.

### 📏 Limits

*Governor & platform limits*

- Requires a BAA with Salesforce and with every downstream processor.
- Shield encryption and Event Monitoring are typically mandatory controls.
- PHI in debug logs is a breach - logs are retained 7 days and widely readable.
- Not all Salesforce features are covered by the BAA.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## SOC2

*Trust services audit of controls (security, availability...).*

### 🌱 Simple

*Beginner - plain language*

**SOC 2** is an audit framework assessing a service organization's controls against the **Trust Services Criteria**: Security, Availability, Processing Integrity, Confidentiality, and Privacy.

### 📏 Limits

*Governor & platform limits*

- Salesforce's own certification does not make your implementation compliant.
- Evidence of access reviews, change control and monitoring is required from you.
- Audit trail retention (6 months in the UI) is often shorter than auditors expect.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## ISO 27001

*Information security management system standard.*

### 🌱 Simple

*Beginner - plain language*

**ISO 27001** is an international standard for an **Information Security Management System (ISMS)** — a risk-based framework of policies and controls (from Annex A) to manage information security.

### 📏 Limits

*Governor & platform limits*

- Covers your processes, not just the platform's certification.
- Requires documented risk assessment, access control and incident management.
- Evidence retention windows often exceed native Salesforce log retention.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## PCI DSS

*Payment card data security standard.*

### 🌱 Simple

*Beginner - plain language*

**PCI DSS** is the security standard for handling **payment card data** (cardholder data) — requiring protections like encryption, access control, network security, and monitoring to prevent card fraud.

### 📏 Limits

*Governor & platform limits*

- Storing cardholder data in Salesforce brings the whole org into scope - avoid it.
- Tokenisation through a payment provider is the standard approach.
- Shield encryption alone does not achieve PCI compliance.
- Debug logs must never contain card data.

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
