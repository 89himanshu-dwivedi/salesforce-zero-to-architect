[Home](../index.md) / [17 · Salesforce Clouds](index.md) / **Sales Cloud**

# Sales Cloud

9 topics · Series 17: Salesforce Clouds

**Topics on this page**

- [Lead Capture](#lead-capture)
- [Lead Assignment](#lead-assignment)
- [Lead Conversion](#lead-conversion)
- [Opportunity Stages](#opportunity-stages)
- [Sales Process](#sales-process)
- [Forecasting](#forecasting)
- [Products](#products)
- [Price Books](#price-books)
- [Quotes](#quotes)

## Lead Capture

*Bring prospects into Salesforce from every channel.*

### 🌱 Simple

*Beginner - plain language*

**Lead Capture** is how unqualified prospects enter Salesforce as **Lead** records — via Web-to-Lead forms, landing pages, imports, marketing tools, or APIs — so sales can work them before converting to Account/Contact/Opportunity.

### 📏 Limits

*Governor & platform limits*

- Web-to-Lead ~500/day, unauthenticated; assignment rules.
- Dedup/validate/enrich at entry; capture source + consent.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Lead Assignment

*Route leads to the right owner automatically.*

### 🌱 Simple

*Beginner - plain language*

**Lead Assignment** automatically sets the **owner** of a new or updated lead based on **assignment rules** — criteria like geography, product interest, or company size — so leads reach the right rep or queue instantly.

### 📏 Limits

*Governor & platform limits*

- One active rule; first match wins; user/queue + alert.
- No native round-robin (Flow/Apex/territory); runs sync.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Lead Conversion

*Turn a qualified lead into Account, Contact, Opportunity.*

### 🌱 Simple

*Beginner - plain language*

**Lead Conversion** transforms a qualified **Lead** into an **Account, Contact**, and optionally an **Opportunity** — moving the prospect from marketing-qualified to an active sales deal, mapping lead fields to the new records.

### 📏 Limits

*Governor & platform limits*

- Creates Account/Contact/(Opportunity); lead → Converted.
- Field mapping required; triggers fire; Person Account differs.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Opportunity Stages

*Pipeline phases driving probability and forecast.*

### 🌱 Simple

*Beginner - plain language*

**Opportunity Stages** are the named phases of a deal (Prospecting → Qualification → … → Closed Won/Lost). Each stage maps to a **probability** and a **forecast category**, driving pipeline reporting and forecasting.

### 📏 Limits

*Governor & platform limits*

- Stage → probability → forecast category; IsClosed/IsWon.
- Sales Process per record type; Path; stage validation.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Sales Process

*Stage sets and methodology per opportunity type.*

### 🌱 Simple

*Beginner - plain language*

A **Sales Process** defines **which opportunity stages** (and their order) apply to a given deal type. Linked to a **record type**, it lets New Business, Renewal, and Partner deals follow tailored pipelines.

### 📏 Limits

*Governor & platform limits*

- Stage subset/order bound to record type; with Path/layout.
- Encodes methodology; stage validation/automation.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Forecasting

*Predict revenue by rolling up opportunities.*

### 🌱 Simple

*Beginner - plain language*

**Forecasting** predicts future revenue by rolling up opportunities into **forecast categories** (Pipeline, Best Case, Commit, Closed) across the sales hierarchy, so leaders see expected attainment vs quota.

### 📏 Limits

*Governor & platform limits*

- Categories roll up role hierarchy per period; quotas.
- Multiple types; manager adjustments; Einstein predictive.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Products

*What you sell: the product catalog records.*

### 🌱 Simple

*Beginner - plain language*

**Products** (Product2) represent the items/services you sell. They form the catalog; combined with **Price Books** they get prices, and they're added to Opportunities and Quotes as line items.

### 📏 Limits

*Governor & platform limits*

- Product2 + PricebookEntry (per book/currency); families.
- Schedules; deactivate not delete; CPQ adds configuration.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Price Books

*Sets of products with prices for segments/markets.*

### 🌱 Simple

*Beginner - plain language*

**Price Books** are collections of products with their prices. The **Standard Price Book** holds base prices; **custom price books** hold segment/region/currency-specific prices. An opportunity uses one price book at a time.

### 📏 Limits

*Governor & platform limits*

- Standard + custom; PricebookEntry per book/currency.
- One book per opp; sharing controls access; static (CPQ for dynamic).

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Quotes

*Formal priced offers generated from opportunities.*

### 🌱 Simple

*Beginner - plain language*

**Quotes** are formal, priced offers to a customer, created from an **Opportunity** with line items. They produce a branded **Quote PDF** and can sync line items back to the opportunity.

### 📏 Limits

*Governor & platform limits*

- Quote + QuoteLineItem; sync; templated PDF; expiry.
- Discount approvals; CPQ for config/bundles/subscriptions.

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
