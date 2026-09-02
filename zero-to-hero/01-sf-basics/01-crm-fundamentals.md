[Home](../index.md) / [01 · SF Basics](index.md) / **CRM Fundamentals**

# CRM Fundamentals

8 topics · Series 1: SF Basics

**Topics on this page**

- [What is CRM](#what-is-crm)
- [Types of CRM](#types-of-crm)
- [Operational CRM](#operational-crm)
- [Analytical CRM](#analytical-crm)
- [Collaborative CRM](#collaborative-crm)
- [Customer Lifecycle](#customer-lifecycle)
- [Lead to Cash](#lead-to-cash)
- [Case to Resolution](#case-to-resolution)

## What is CRM

*CRM = the system + strategy to manage every interaction with customers and prospects in one place.*

### 🌱 Simple

*Beginner - plain language*

**CRM (Customer Relationship Management)** is software that stores everything about your customers — who they are, what they bought, every call/email/meeting, and what's next — in one shared place so the whole company sees the same truth.

Before CRM, sales data lived in spreadsheets, support data in email, and marketing data in another tool. Nobody had the full picture. CRM unifies it.

- **People:** contacts, leads, accounts (companies)
- **Deals:** opportunities / pipeline
- **Service:** cases / tickets
- **Activity:** tasks, calls, emails, events

### 📏 Limits

*Governor & platform limits*

CRM itself is a concept, but the Salesforce platform that delivers it has hard limits you design around early:

- Standard & custom objects, fields per object, and data storage are governed (e.g. storage measured in records, ~2KB each).
- Per-transaction governor limits (SOQL, DML, CPU) shape how automation is built.
- API request limits constrain integration volume.

Exact numbers vary by edition/license — verify in *Setup → Company Information → System Overview* and the Salesforce Limits guide.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Types of CRM

*Operational, Analytical, Collaborative — three lenses on the same customer data.*

### 🌱 Simple

*Beginner - plain language*

CRMs are grouped by **what job they do**:

- **Operational** — runs day-to-day work: capture leads, manage deals, log cases, automate steps.
- **Analytical** — turns stored data into insight: reports, dashboards, trends, predictions.
- **Collaborative** — shares that customer info across teams/partners so everyone stays aligned.

Salesforce is all three in one platform.

### 📏 Limits

*Governor & platform limits*

- Reports: row limits in standard reports; use CRM Analytics for very large datasets.
- Dashboards have component and dynamic-dashboard limits per edition.
- Heavy analytical workloads should move off-platform (Data Cloud / warehouse) before LDV thresholds.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Operational CRM

*The 'doing' CRM — automates and executes sales, service and marketing processes.*

### 🌱 Simple

*Beginner - plain language*

**Operational CRM** handles the everyday running of customer work: capturing a lead, moving a deal through stages, logging a support case, sending an automated email. It's about **executing processes efficiently**.

### 📏 Limits

*Governor & platform limits*

- Per-transaction governor limits apply to all automation: 100 SOQL, 150 DML, 10s CPU (sync).
- Flow has interview & element execution limits; bulk triggers process 200 records/batch.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Analytical CRM

*The 'understanding' CRM — turns customer data into reports, trends and predictions.*

### 🌱 Simple

*Beginner - plain language*

**Analytical CRM** looks at the data you collected and answers questions: Which products sell best? Which customers might churn? How long do cases take to resolve? It powers **decisions**, not day-to-day tasks.

### 📏 Limits

*Governor & platform limits*

- Standard report row display limits; dashboard refresh and component limits per edition.
- CRM Analytics has row/dataset limits by license tier.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Collaborative CRM

*The 'sharing' CRM — aligns teams, departments and partners around the same customer.*

### 🌱 Simple

*Beginner - plain language*

**Collaborative CRM** makes sure everyone touching a customer — sales, service, marketing, partners — sees the same info and can communicate in context, so the customer never has to repeat themselves.

### 📏 Limits

*Governor & platform limits*

- Sharing rule counts per object and group membership limits.
- Experience Cloud license types/limits; guest user sharing rule constraints.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Customer Lifecycle

*The end-to-end journey: Awareness → Lead → Customer → Retention → Advocacy.*

### 🌱 Simple

*Beginner - plain language*

The **customer lifecycle** is the journey a person takes with your company: they hear about you, become a lead, buy (become a customer), get supported, and ideally stay and recommend you.

```text
Awareness → Lead → Opportunity → Customer → Support → Renewal/Advocacy
```

### 📏 Limits

*Governor & platform limits*

- Lead conversion creates Account+Contact+Opportunity in one step — watch automation/limits firing on all three.
- Campaign member limits and influence model constraints.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Lead to Cash

*The revenue process: Lead → Opportunity → Quote → Order → Invoice → Cash (Q2C).*

### 🌱 Simple

*Beginner - plain language*

**Lead-to-Cash (L2C / Quote-to-Cash)** is the full money-making journey: find a lead, win the deal, send a quote, take the order, bill them, collect payment. It connects sales to finance.

### 📏 Limits

*Governor & platform limits*

- CPQ quote calculation and bundle limits; callout limits on synchronous order push.
- Integration API request limits at month-end billing spikes.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Case to Resolution

*The service process: Case created → routed → worked → escalated/SLA → resolved → closed.*

### 🌱 Simple

*Beginner - plain language*

**Case-to-Resolution** is the support journey: a customer raises an issue (a Case), it gets routed to the right agent, worked on, and resolved within a promised time, then closed. It's the service equivalent of lead-to-cash.

### 📏 Limits

*Governor & platform limits*

- Assignment/escalation rule entries and Omni-Channel capacity limits.
- Case LDV: high-volume orgs need archiving & skinny tables for reporting.

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
