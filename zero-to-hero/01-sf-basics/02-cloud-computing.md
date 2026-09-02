[Home](../index.md) / [01 · SF Basics](index.md) / **Cloud Computing**

# Cloud Computing

9 topics · Series 1: SF Basics

**Topics on this page**

- [What is Cloud](#what-is-cloud)
- [On Premise vs Cloud](#on-premise-vs-cloud)
- [SaaS](#saas)
- [PaaS](#paas)
- [IaaS](#iaas)
- [Public Cloud](#public-cloud)
- [Private Cloud](#private-cloud)
- [Hybrid Cloud](#hybrid-cloud)
- [Multi Cloud](#multi-cloud)

## What is Cloud

*Renting computing (servers, storage, software) over the internet instead of owning hardware.*

### 🌱 Simple

*Beginner - plain language*

**Cloud computing** means using someone else's computers over the internet instead of buying and running your own. You pay for what you use, scale up/down on demand, and access it from anywhere — like electricity from the grid rather than your own generator.

### 📏 Limits

*Governor & platform limits*

- Salesforce enforces per-transaction governor limits and API/storage quotas precisely because it's shared multi-tenant cloud.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## On Premise vs Cloud

*Own-and-operate hardware vs consume-as-a-service — cost, control and responsibility trade-offs.*

### 🌱 Simple

*Beginner - plain language*

**On-premise:** you buy servers, install software, and run everything in your building — full control, full responsibility. **Cloud:** a vendor runs it; you just use it — less control, far less hassle.

### 📏 Limits

*Governor & platform limits*

- On-premise gives full control but you own patching, scaling and DR; cloud trades that for shared-tenant constraints.
- In Salesforce those constraints appear as governor limits, API allocations and storage quotas.
- You cannot install software, run background daemons, or access the database directly.
- Upgrade timing is not yours - three seasonal releases per year are mandatory.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## SaaS

*Software-as-a-Service — fully managed applications you just log in and use. Salesforce is SaaS.*

### 🌱 Simple

*Beginner - plain language*

**SaaS** = ready-to-use software over the internet. You don't install or maintain anything — just log in and work (Gmail, Salesforce, Netflix). The vendor handles servers, updates and security.

### 📏 Limits

*Governor & platform limits*

- No control over infrastructure, patching schedule or upgrade timing.
- Customisation is bounded by what the vendor exposes - governor limits, metadata limits and API quotas.
- Data residency depends on the region chosen at provisioning and is hard to change later.
- Vendor lock-in is real: extraction is possible, but re-platforming logic is not.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## PaaS

*Platform-as-a-Service — build & run custom apps without managing servers. Salesforce Platform & Heroku.*

### 🌱 Simple

*Beginner - plain language*

**PaaS** gives developers a ready platform to build apps — database, runtime, deployment — without managing servers. You bring code; the platform runs it.

### 📏 Limits

*Governor & platform limits*

- You write code but run it inside the vendor's runtime - Apex has CPU, heap, SOQL and DML limits.
- No filesystem, no threads, no long-running processes.
- Language and library choice is fixed; you cannot add JARs or npm packages server-side.
- Scaling is automatic but so is throttling.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## IaaS

*Infrastructure-as-a-Service — rent raw VMs, storage, network. AWS/Azure/GCP. Salesforce runs ON IaaS.*

### 🌱 Simple

*Beginner - plain language*

**IaaS** rents you the raw building blocks — virtual servers, storage, networking — and you install/manage everything above the OS. Examples: AWS EC2, Azure VMs, Google Compute Engine.

### 📏 Limits

*Governor & platform limits*

- You own the OS, patching, scaling and security hardening - far more operational burden.
- Cost scales with provisioned capacity, not usage, unless carefully managed.
- Integrating IaaS workloads with Salesforce still consumes callout and API limits.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Public Cloud

*Shared, multi-tenant infrastructure operated by a provider — maximum scale & economy.*

### 🌱 Simple

*Beginner - plain language*

**Public cloud** is infrastructure shared by many organisations, run by a provider (AWS, Azure, Salesforce). You get huge scale and low cost because resources are pooled.

### 📏 Limits

*Governor & platform limits*

- Isolation is logical, not physical - encrypt sensitive data and design for shared infrastructure.
- Multi-tenant fairness is the origin of every governor limit you will hit.
- Region selection determines data residency and is set at provisioning.
- You cannot audit the underlying hardware or hypervisor.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Private Cloud

*Dedicated cloud infrastructure for one organisation — more control/isolation, higher cost.*

### 🌱 Simple

*Beginner - plain language*

**Private cloud** is cloud infrastructure dedicated to a single organisation — more control and isolation than public cloud, but more cost and management. Common in banking, defence, healthcare.

### 📏 Limits

*Governor & platform limits*

- Higher cost and longer provisioning than public cloud.
- Salesforce does not offer a true private-cloud edition; Hyperforce runs on public cloud infrastructure with tenant isolation.
- You still cannot bypass platform limits by paying more in most cases.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Hybrid Cloud

*Mix of public cloud + private/on-prem, integrated — the realistic enterprise state.*

### 🌱 Simple

*Beginner - plain language*

**Hybrid cloud** combines public cloud and private/on-prem systems working together. Most large companies are hybrid: Salesforce in the cloud, some legacy systems still on-prem, connected by integration.

### 📏 Limits

*Governor & platform limits*

- Every hop between cloud and on-premise adds latency that counts toward the 120s callout budget.
- Requires network connectivity (VPN, MuleSoft, gateway) that becomes a single point of failure.
- Data synchronised across the boundary has residency and compliance implications.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Multi Cloud

*Using multiple cloud providers/clouds together — avoid lock-in, best-of-breed, resilience.*

### 🌱 Simple

*Beginner - plain language*

**Multi-cloud** means using more than one cloud provider/service together — e.g., Salesforce for CRM, AWS for compute, Azure for AI — picking the best tool from each and avoiding dependence on one vendor.

### 📏 Limits

*Governor & platform limits*

- Each platform brings its own limits, identity model and operational tooling.
- Cross-cloud data movement consumes API allocations on both sides and adds egress cost.
- No single pane of glass - observability must be built deliberately.

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
