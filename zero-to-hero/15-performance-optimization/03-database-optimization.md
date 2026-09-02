[Home](../index.md) / [15 · Performance Optimization](index.md) / **Database Optimization**

# Database Optimization

4 topics · Series 15: Performance Optimization

**Topics on this page**

- [Skinny Tables](#skinny-tables)
- [Divisions](#divisions)
- [Data Partitioning](#data-partitioning)
- [Archiving](#archiving)

## Skinny Tables

*Salesforce-built denormalized table for fast reads.*

### 🌱 Simple

*Beginner - plain language*

A **skinny table** is a special read-optimized copy Salesforce creates (via support) that contains a **subset of frequently-used fields** from a large object — avoiding joins to the standard/custom field tables for faster queries and reports.

### 📏 Limits

*Governor & platform limits*

- Created only by Salesforce Support.
- Cannot include formula fields or fields with nulls in some configurations.
- Must be recreated when the field list changes.
- Not automatically copied to sandboxes on refresh.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Divisions

*Partition data into segments to scope queries/reports.*

### 🌱 Simple

*Beginner - plain language*

**Divisions** let you partition an org's data into logical segments (e.g., by business unit or region) so users, queries, and reports can be **scoped to one division** — reducing the data each operation touches.

### 📏 Limits

*Governor & platform limits*

- Must be enabled by Salesforce Support and cannot easily be removed.
- Maximum 100 divisions per org.
- Adds a division filter to reports and list views that users must understand.
- Largely superseded by better sharing design.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Data Partitioning

*Designing data layout to reduce scan size and skew.*

### 🌱 Simple

*Beginner - plain language*

**Data partitioning** is the architectural practice of organizing large data so operations touch **smaller, selective subsets** — via indexed keys, divisions, ownership distribution, record types, or separate objects/orgs.

### 📏 Limits

*Governor & platform limits*

- Salesforce has no native table partitioning - you partition logically.
- Big Objects have an immutable index that defines the partition strategy.
- Repartitioning existing data requires a full migration.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Archiving

*Moving old data out of hot objects to keep them fast.*

### 🌱 Simple

*Beginner - plain language*

**Archiving** moves old, rarely-accessed records out of your active objects (into Big Objects, external storage, or another system) so the hot tables stay **small and fast** while history remains retrievable.

### 📏 Limits

*Governor & platform limits*

- Big Object index is immutable - design the query pattern first.
- Archived records lose triggers, reports and standard list views.
- Archival jobs consume async allocations and API limits.
- Retention obligations may prevent deletion after archiving.

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
