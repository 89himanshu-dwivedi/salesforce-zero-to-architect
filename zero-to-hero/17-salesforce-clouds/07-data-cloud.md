[Home](../index.md) / [17 · Salesforce Clouds](index.md) / **Data Cloud**

# Data Cloud

8 topics · Series 17: Salesforce Clouds

**Topics on this page**

- [Data Streams](#data-streams)
- [Identity Resolution](#identity-resolution)
- [Data Model Objects](#data-model-objects)
- [Unified Profiles](#unified-profiles)
- [Segmentation](#segmentation)
- [Activation](#activation)
- [Calculated Insights](#calculated-insights)
- [Data Actions](#data-actions)

## Data Streams

*Ingest data into Data Cloud from any source.*

### 🌱 Simple

*Beginner - plain language*

**Data Streams** bring data **into Data Cloud** from sources — Salesforce CRM, Marketing Cloud, web/mobile SDKs, cloud storage (S3), and external systems — defining how each source's data is ingested and refreshed.

### 📏 Limits

*Governor & platform limits*

- Connectors (CRM/MC/SDK/S3/API); Profile/Engagement/Other categories.
- Land DLOs → map DMOs; batch/streaming; usage-priced.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Identity Resolution

*Match and merge records into unified individuals.*

### 🌱 Simple

*Beginner - plain language*

**Identity Resolution** matches records from different sources that belong to the **same person** and merges them into a **Unified Individual** — creating a single view of the customer across systems.

### 📏 Limits

*Governor & platform limits*

- Match (exact/fuzzy) + reconciliation rules → Unified Individual + ID graph.
- Profile-category only; tune to avoid over/under-merge.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Data Model Objects

*Standardized schema for harmonized data.*

### 🌱 Simple

*Beginner - plain language*

**Data Model Objects (DMOs)** are the **standardized schema** in Data Cloud that raw ingested data (DLOs) maps into — harmonizing different sources into a common model (e.g., a unified Individual, Account Contact, Sales Order).

### 📏 Limits

*Governor & platform limits*

- DLO (raw) → DMO (standardized, harmonized); standard + custom.
- Related, queryable model; feeds identity/segmentation/insights.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Unified Profiles

*Single 360° view of each customer.*

### 🌱 Simple

*Beginner - plain language*

**Unified Profiles** are the **single, 360° view** of each customer produced by Data Cloud — combining identity-resolved profile data and engagement across all sources into one queryable individual.

### 📏 Limits

*Governor & platform limits*

- Unified Individual + contact points + engagement; queryable.
- Real-time; powers segmentation/insights/personalization/activation.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Segmentation

*Build audiences from unified data.*

### 🌱 Simple

*Beginner - plain language*

**Segmentation** builds **audiences** from unified profiles — filtering on profile attributes, engagement, and calculated insights to target the right customers for activation and personalization.

### 📏 Limits

*Governor & platform limits*

- Attribute/engagement/insight + related-DMO criteria; nested.
- Batch or real-time refresh; feeds activation; credit-aware.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Activation

*Send segments and data to action channels.*

### 🌱 Simple

*Beginner - plain language*

**Activation** sends Data Cloud segments and profile attributes **out to channels** — Marketing Cloud, advertising platforms, Salesforce CRM, and external systems — so unified data drives real campaigns and experiences.

### 📏 Limits

*Governor & platform limits*

- Targets: Marketing Cloud, Ads, CRM, external; mapped attributes.
- Scheduled/real-time; consent-enforced; closed-loop; credit-aware.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Calculated Insights

*Compute metrics and KPIs over unified data.*

### 🌱 Simple

*Beginner - plain language*

**Calculated Insights** compute **metrics and KPIs** over unified data — like lifetime value, purchase frequency, or engagement scores — at the individual or aggregate level, usable in segmentation and activation.

### 📏 Limits

*Governor & platform limits*

- SQL-like over DMOs → measures/dimensions; per-individual or aggregate.
- Used in segmentation/activation/profiles; scheduled; credit-aware.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Data Actions

*Trigger downstream actions from data events.*

### 🌱 Simple

*Beginner - plain language*

**Data Actions** trigger **downstream actions** from Data Cloud events — like publishing to a platform event, calling a webhook, or starting a flow — when data changes or insights cross thresholds, enabling real-time reactions.

### 📏 Limits

*Governor & platform limits*

- Triggers: data change/insight threshold/engagement → Platform Events/MC/webhook.
- Event-driven (vs scheduled activation); real-time; consent-aware.

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
