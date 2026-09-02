[Home](../index.md) / [20 · Real-Time Scenarios (CTA)](index.md) / **High Availability**

# High Availability

6 topics · Series 20: Real-Time Scenarios (CTA)

**Topics on this page**

- [Disaster Recovery](#disaster-recovery)
- [Backup Strategy](#backup-strategy)
- [Failover Strategy](#failover-strategy)
- [Business Continuity](#business-continuity)
- [RTO](#rto)
- [RPO](#rpo)

## Disaster Recovery

*Plan to recover Salesforce service and data after a disaster.*

### 🌱 Simple

*Beginner - plain language*

**Disaster Recovery (DR)** is the plan to **restore Salesforce service and data** after a major disruption — covering Salesforce's own platform DR plus your own data/config recovery responsibilities.

### 📏 Limits

*Governor & platform limits*

- Shared responsibility; define RTO/RPO; independent data+metadata backups.
- Scenario coverage; tested runbooks; integration DR + event replay.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Backup Strategy

*Protect Salesforce data and metadata with reliable backups.*

### 🌱 Simple

*Beginner - plain language*

A **backup strategy** ensures Salesforce **data and metadata** are reliably copied and restorable — protecting against corruption, accidental deletion, and bad deployments, since Salesforce doesn't undo your data errors.

### 📏 Limits

*Governor & platform limits*

- RPO-driven frequency; full scope (data+metadata+files+relationships).
- Granular + full restore via tool; governed retention/storage; test restores.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Failover Strategy

*Keep service available when components fail.*

### 🌱 Simple

*Beginner - plain language*

A **failover strategy** keeps service available when components fail — switching to **backup systems, sandboxes, or degraded modes** so users and integrations continue working during outages.

### 📏 Limits

*Governor & platform limits*

- Salesforce platform failover awareness; queued/retrying resilient integrations.
- Circuit-breaker fallbacks; degraded/offline modes; decoupling; failover drills.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Business Continuity

*Keep the business operating through any disruption.*

### 🌱 Simple

*Beginner - plain language*

**Business Continuity (BCP)** ensures the **business keeps operating** during and after disruptions — people, processes, and technology — with Salesforce DR/failover being one part of a broader plan.

### 📏 Limits

*Governor & platform limits*

- BCP = people+process+technology; DR/failover are subsets; BIA sets priorities.
- Alternate workflows; roles + comms; dependency mapping; regular drills.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## RTO

*Recovery Time Objective — max acceptable downtime.*

### 🌱 Simple

*Beginner - plain language*

**RTO (Recovery Time Objective)** is the **maximum acceptable downtime** for a system or process after a disruption — how quickly it must be back up before the impact is unacceptable.

### 📏 Limits

*Governor & platform limits*

- Max acceptable downtime; set per process by BIA; drives recovery design/cost.
- Distinct from RPO (data loss); validate via drills; Salesforce = restore/integration time.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## RPO

*Recovery Point Objective — max acceptable data loss.*

### 🌱 Simple

*Beginner - plain language*

**RPO (Recovery Point Objective)** is the **maximum acceptable data loss** — how much recent data (measured in time) you can afford to lose in a disruption, which sets how often you must back up.

### 📏 Limits

*Governor & platform limits*

- Max acceptable data loss; set per data by BIA; drives backup/replication frequency.
- Distinct from RTO (downtime); tooling must meet it; validate restores.

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
