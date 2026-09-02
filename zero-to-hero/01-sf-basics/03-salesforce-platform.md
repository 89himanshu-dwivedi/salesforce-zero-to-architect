[Home](../index.md) / [01 · SF Basics](index.md) / **Salesforce Platform**

# Salesforce Platform

10 topics · Series 1: SF Basics

**Topics on this page**

- [Salesforce History](#salesforce-history)
- [Salesforce Architecture](#salesforce-architecture)
- [Multi Tenant Architecture](#multi-tenant-architecture)
- [Metadata Driven Architecture](#metadata-driven-architecture)
- [Hyperforce](#hyperforce)
- [POD](#pod)
- [Instance](#instance)
- [Trust Site](#trust-site)
- [Release Cycle](#release-cycle)
- [Seasonal Releases](#seasonal-releases)

## Salesforce History

*From 'The End of Software' (1999) to Hyperforce, Einstein and Agentforce.*

### 🌱 Simple

*Beginner - plain language*

Salesforce was founded in **1999** by Marc Benioff with the slogan *"The End of Software"* — the idea that business software should be delivered over the internet like a website, not installed on servers. It pioneered SaaS CRM.

### 📏 Limits

*Governor & platform limits*

- Backward compatibility means legacy features (Workflow Rules, Process Builder, Classic) linger long after retirement is announced.
- API versions are supported for a limited window - old versions are retired on a published schedule.
- Features deprecated in one release may still be present but unsupported for years.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Salesforce Architecture

*Layered: Infrastructure (Hyperforce) → Multi-tenant kernel → Metadata → Apps → UI.*

### 🌱 Simple

*Beginner - plain language*

Salesforce's architecture is built in **layers**. At the bottom is shared cloud infrastructure; on top sits a smart engine that serves many customers at once using **metadata** (your config) to render each org differently — all the way up to the UI you click.

### 📏 Limits

*Governor & platform limits*

- Every governor limit derives from the shared multi-tenant kernel - they are not negotiable.
- No direct database access, no stored procedures, no long-running server processes.
- Query behaviour is controlled by the optimiser; you influence it only through indexes and selectivity.
- Compute is throttled per transaction, not per org.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Multi Tenant Architecture

*Many customers share one application instance, isolated logically by OrgId.*

### 🌱 Simple

*Beginner - plain language*

**Multi-tenancy** means many companies (tenants) share the same Salesforce application and infrastructure, like apartments in one building. Each has private space (their data/metadata) but shares the structure (the platform), which keeps costs low and upgrades universal.

### 📏 Limits

*Governor & platform limits*

- All governor limits exist to protect other tenants from your code.
- Limits are per transaction, not per org, and are shared with flows and managed packages.
- You cannot buy your way out of most limits - only a few are purchasable add-ons.
- Noisy-neighbour effects are managed by the platform, not by you.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Metadata Driven Architecture

*Your config (objects, fields, flows, code) is metadata the platform reads to render the app at runtime.*

### 🌱 Simple

*Beginner - plain language*

In Salesforce, almost everything you build — objects, fields, page layouts, flows, even Apex — is stored as **metadata** ("data about your app"). The platform reads this metadata to generate your application on the fly, so two orgs look totally different from the same engine.

### 📏 Limits

*Governor & platform limits*

- Metadata deploys cap at 10,000 files or 39 MB per deployment.
- Per-object and per-org component limits (fields, objects, tabs, apps) constrain the model.
- Metadata bloat slows describe calls, page loads and every deployment.
- Some configuration is data, not metadata, and never deploys.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Hyperforce

*Salesforce's next-gen infrastructure running on public cloud (AWS etc.) for scale & data residency.*

### 🌱 Simple

*Beginner - plain language*

**Hyperforce** is Salesforce's modern infrastructure that runs the platform on public clouds like AWS. It lets Salesforce deploy in many regions worldwide, so customers get better performance and can keep data in a specific country.

### 📏 Limits

*Governor & platform limits*

- Region is chosen at provisioning; migrating between regions is a Salesforce-managed project, not a setting.
- Not all features and add-ons are available in every Hyperforce region.
- IP ranges differ from classic instances - allow-lists must be updated.
- Governor limits are unchanged by Hyperforce.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## POD

*A 'POD' (instance) is a cluster of infrastructure hosting many orgs; your org lives on one.*

### 🌱 Simple

*Beginner - plain language*

A **POD** (also called an **instance**) is a bundle of servers that hosts many customer orgs together. Your org runs on one POD — e.g., historically named like *NA45* or *EU16*. Maintenance and releases happen per POD.

### 📏 Limits

*Governor & platform limits*

- Your org shares its pod with other tenants; performance can be affected by pod-level events.
- Pod maintenance windows are published on Trust and are not negotiable.
- Pod migrations happen with notice and can change instance URLs - never hardcode them.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Instance

*The infrastructure unit your org runs on — synonymous with POD; tracked on Salesforce Trust.*

### 🌱 Simple

*Beginner - plain language*

An **instance** is the specific Salesforce infrastructure your org runs on. It's effectively the same idea as a POD. You can find yours in *Setup → Company Information* and monitor it on Salesforce Trust.

### 📏 Limits

*Governor & platform limits*

- Instance name can change during a pod migration - always use My Domain, never the instance URL.
- Maintenance windows are set by Salesforce and published on Trust.
- Instance-level incidents affect all tenants on it and are outside your control.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Trust Site

*trust.salesforce.com / status.salesforce.com — real-time platform status, security & compliance.*

### 🌱 Simple

*Beginner - plain language*

**Salesforce Trust** (trust.salesforce.com) is the public website showing live system status, performance, maintenance schedules, security advisories and compliance certifications for every instance. It's how you check "is Salesforce down?".

### 📏 Limits

*Governor & platform limits*

- Reports instance-level availability and maintenance only - it will not show org-specific problems.
- Known Issues are tracked separately and may have no fix or workaround.
- Status updates can lag the incident by several minutes.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Release Cycle

*Salesforce ships three major releases a year automatically — design for continuous upgrade.*

### 🌱 Simple

*Beginner - plain language*

Salesforce upgrades everyone **three times a year** automatically (Spring, Summer, Winter). You don't install anything — new features just appear. Your customisations keep working because of the metadata model.

### 📏 Limits

*Governor & platform limits*

- Three major releases per year that cannot be deferred or skipped.
- Sandbox preview is the only lead time you get - typically a few weeks.
- Features can be deprecated or changed with one release of notice.
- Release windows are fixed per instance and published in advance.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Seasonal Releases

*Spring / Summer / Winter — the three named yearly releases and their cadence.*

### 🌱 Simple

*Beginner - plain language*

Salesforce names its three yearly releases by season: **Spring** (Feb), **Summer** (June), and **Winter** (Oct, named for the *next* year, e.g., Winter '25). Each brings new features to every org automatically.

### 📏 Limits

*Governor & platform limits*

- Spring, Summer and Winter releases are mandatory; there is no opt-out.
- Preview sandboxes are assigned by instance - you cannot choose.
- Critical updates activate automatically on a published schedule.
- Managed package compatibility must be verified before your instance upgrades.

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
