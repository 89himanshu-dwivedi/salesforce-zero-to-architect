[Home](../index.md) / [17 · Salesforce Clouds](index.md) / **CPQ**

# CPQ

9 topics · Series 17: Salesforce Clouds

**Topics on this page**

- [CPQ Products](#cpq-products)
- [Bundles](#bundles)
- [Product Rules](#product-rules)
- [Price Rules](#price-rules)
- [Discount Schedules](#discount-schedules)
- [Quote Calculator Plugin](#quote-calculator-plugin)
- [Amendments](#amendments)
- [Renewals](#renewals)
- [Contracts](#contracts)

## CPQ Products

*Configurable catalog items in Salesforce CPQ.*

### 🌱 Simple

*Beginner - plain language*

In **Salesforce CPQ**, **Products** are catalog items enriched for configuration — with options, features, attributes, and pricing — so reps can build complex quotes accurately through guided selling.

### 📏 Limits

*Governor & platform limits*

- Options/Features/Attributes; List/Cost+Markup/Block/%-of-Total.
- Subscription pricing; product rules; ERP-synced catalog.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Bundles

*Parent product with configurable options and features.*

### 🌱 Simple

*Beginner - plain language*

A **Bundle** is a parent product containing **options** (child products) organized into **features**, letting reps configure a package (e.g., a laptop + accessories + warranty) as one quote line structure.

### 📏 Limits

*Governor & platform limits*

- Features (min/max) + Options (qty/required/pricing).
- Option Constraints + Product Rules; nesting (watch performance).

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Product Rules

*Constrain and automate product configuration.*

### 🌱 Simple

*Beginner - plain language*

**Product Rules** in CPQ control configuration — validating selections, auto-adding/removing products, alerting reps, and filtering option lists — to ensure only valid, complete quotes are built.

### 📏 Limits

*Governor & platform limits*

- Validation/Selection/Alert/Filter; conditions + actions.
- Lookup Queries + Summary Variables; scope global/bundle; perf-sensitive.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Price Rules

*Inject/calculate pricing logic during quoting.*

### 🌱 Simple

*Beginner - plain language*

**Price Rules** in CPQ dynamically **set or adjust field values** (prices, discounts, custom fields) on quote lines during calculation — applying pricing logic beyond static price books.

### 📏 Limits

*Governor & platform limits*

- Conditions + actions + evaluation event (Before/On/After).
- Lookup Queries + Summary Variables; QCP for beyond-rules logic.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Discount Schedules

*Volume/term-based tiered discounting.*

### 🌱 Simple

*Beginner - plain language*

**Discount Schedules** apply **tiered discounts** based on quantity or term — e.g., buy more, save more — automatically adjusting price as the rep changes quantities on a quote line.

### 📏 Limits

*Governor & platform limits*

- Range (whole qty) vs Slab (marginal bands); volume/term.
- Attach product/account; aggregation; live recalc.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Quote Calculator Plugin

*JavaScript hook into the CPQ pricing engine.*

### 🌱 Simple

*Beginner - plain language*

The **Quote Calculator Plugin (QCP)** is a JavaScript hook that lets you inject **custom pricing/calculation logic** into the CPQ quote calculation lifecycle when declarative price rules aren't enough.

### 📏 Limits

*Governor & platform limits*

- JS static resource; lifecycle methods; client-side.
- Escape hatch after declarative; perf- and upgrade-sensitive.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Amendments

*Mid-term changes to active subscriptions.*

### 🌱 Simple

*Beginner - plain language*

**Amendments** handle **mid-term changes** to an active subscription contract — adding products, increasing/reducing quantities, or upgrading — with **proration** for the remaining term.

### 📏 Limits

*Governor & platform limits*

- Amend from Contract; pre-loaded subscriptions; proration.
- Co-terminate to contract end; honor contracted pricing.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Renewals

*Extend subscriptions for the next term.*

### 🌱 Simple

*Beginner - plain language*

**Renewals** create the next-term quote/contract for an expiring subscription — automatically or manually — carrying forward products, quantities, and (optionally) pricing uplift, to retain recurring revenue.

### 📏 Limits

*Governor & platform limits*

- Auto-generate renewal opp/quote; carry-forward subscriptions.
- Uplift pricing (honor caps); manual vs evergreen; feeds forecast.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Contracts

*System of record for subscriptions and terms.*

### 🌱 Simple

*Beginner - plain language*

In CPQ, a **Contract** is generated when an order is activated; it's the **system of record for active subscriptions** and terms, and the basis for amendments and renewals.

### 📏 Limits

*Governor & platform limits*

- Order activation → Contract + Subscriptions; contracted pricing.
- Drives amend/renew; co-termination; feeds billing.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

---

## Connect

These pages carry the **definitions and limits** only. The advanced depth, real-world
scenarios, error playbooks, best-option reasoning and interview questions are kept aside.

If you would like them, or you want to talk about anything on this page:

- **LinkedIn** - [in/himanshukumar-sf](https://www.linkedin.com/in/himanshukumar-sf/)
- **X** - [@kum60094](https://x.com/kum60094)
- **GitHub** - [89himanshu-dwivedi](https://github.com/89himanshu-dwivedi)
- **Email** - [himanshu.jee.1996@gmail.com](mailto:himanshu.jee.1996@gmail.com)

*- Himanshu Kumar*
