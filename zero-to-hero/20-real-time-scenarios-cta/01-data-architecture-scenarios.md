[Home](../index.md) / [20 · Real-Time Scenarios (CTA)](index.md) / **Data Architecture Scenarios**

# Data Architecture Scenarios

6 topics · Series 20: Real-Time Scenarios (CTA)

**Topics on this page**

- [1M Records](#1m-records)
- [10M Records](#10m-records)
- [100M Records](#100m-records)
- [Archival Strategy](#archival-strategy)
- [Data Retention](#data-retention)
- [Data Migration](#data-migration)

## 1M Records

*Designing for ~1 million records — the first scale threshold.*

### 🌱 Simple

*Beginner - plain language*

At **~1 million records** on an object, Salesforce mostly behaves normally — but you start designing for **query selectivity, indexing, and reporting** so performance stays healthy as you grow.

### 📏 Limits

*Governor & platform limits*

- Selective indexed filters; custom indexes on high-cardinality fields.
- Bounded reports/list views; avoid skew; plan archiving; test at volume.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## 10M Records

*Designing for ~10 million records — true LDV territory.*

### 🌱 Simple

*Beginner - plain language*

At **~10 million records**, you're in genuine **Large Data Volume (LDV)** territory — selectivity, indexing, skew avoidance, and possibly **skinny tables** become essential, not optional.

### 📏 Limits

*Governor & platform limits*

- Mandatory selectivity + custom/two-column indexes; skinny tables.
- Eliminate skew; PK chunking + deferred sharing recalc; archive/divisions.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## 100M Records

*Designing for 100M+ records — extreme-scale architecture.*

### 🌱 Simple

*Beginner - plain language*

At **100 million+ records**, you architect for **extreme scale** — aggressive archiving, **Big Objects**, off-platform storage, mandatory skinny tables/indexes, and minimizing what lives in the transactional hot set.

### 📏 Limits

*Governor & platform limits*

- Hot/cold tiering; Big Objects (Async SOQL, composite key); external/lake.
- Skinny tables + indexes; PK chunking/Batch; continuous archive; offload analytics.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Archival Strategy

*Move cold data out of the transactional store systematically.*

### 🌱 Simple

*Beginner - plain language*

An **archival strategy** defines how and when **old, cold data** moves out of active Salesforce objects — to Big Objects, external storage, or a warehouse — to keep the hot set lean and performant.

### 📏 Limits

*Governor & platform limits*

- Cold criteria; destination by access (Big Objects/external/warehouse).
- Extract→verify→purge in chunks; surface archive; align with retention law.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Data Retention

*Policy for how long data is kept and when it's deleted.*

### 🌱 Simple

*Beginner - plain language*

A **data retention** policy defines **how long** each type of data is kept and **when it's deleted** — balancing business need, storage cost, and legal/regulatory requirements.

### 📏 Limits

*Governor & platform limits*

- Classify data; regulation-aligned periods; delete/anonymize/archive.
- Automate disposal; right-to-erasure; legal holds; audit logging.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Data Migration

*Move data into Salesforce accurately, at scale, safely.*

### 🌱 Simple

*Beginner - plain language*

**Data migration** is moving data from legacy/source systems into Salesforce **accurately and at scale** — mapping, cleansing, loading in the right order, validating, and handling errors.

### 📏 Limits

*Governor & platform limits*

- Cleanse/dedupe; map; parent→child order with External-ID upserts.
- Bulk v2 + PK chunking; disable automation; reconcile; rehearse cutover + rollback.

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
