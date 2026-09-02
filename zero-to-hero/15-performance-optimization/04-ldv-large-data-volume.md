[Home](../index.md) / [15 · Performance Optimization](index.md) / **LDV (Large Data Volume)**

# LDV (Large Data Volume)

5 topics · Series 15: Performance Optimization

**Topics on this page**

- [1M Records](#1m-records)
- [10M Records](#10m-records)
- [100M Records](#100m-records)
- [Data Retention](#data-retention)
- [Data Purging](#data-purging)

## 1M Records

*The threshold where selectivity starts to matter.*

### 🌱 Simple

*Beginner - plain language*

Around **1 million records** per object is where Salesforce's optimizer thresholds begin to bite: queries must be **selective** (indexed filters) or they slow down and can error. It's the entry point of "large data volume" thinking.

### 📏 Limits

*Governor & platform limits*

- Selectivity enforcement is already active above 200,000 rows.
- Full scans are refused, so every filter must be indexed.
- Report and list view performance degrades without indexed filters.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## 10M Records

*Where indexing, skew, and archiving become essential.*

### 🌱 Simple

*Beginner - plain language*

At **10 million records**, selective filters and proper indexing are **mandatory** — non-selective queries fail outright, data skew causes locking problems, and you typically need custom indexes, skinny tables, or archiving.

### 📏 Limits

*Governor & platform limits*

- Selectivity threshold caps at 333,333 rows regardless of table size.
- Skinny tables and custom indexes become necessary, both requiring Support.
- Batch jobs must use QueryLocator; 50,000-row transactions are impossible.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## 100M Records

*Extreme scale: Big Objects, separate storage, careful loads.*

### 🌱 Simple

*Beginner - plain language*

At **100 million+ records**, the standard object model strains: you rely on **Big Objects**, external storage, PK chunking for loads, and aggressive archiving. Every operation must be selective and bulk-engineered.

### 📏 Limits

*Governor & platform limits*

- QueryLocator supports up to 50 million rows - beyond that you must partition the job.
- Data storage cost becomes a primary architectural constraint.
- Archiving to Big Objects is effectively mandatory.
- Sharing recalculation at this volume can run for many hours.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Data Retention

*Policy for how long data stays in hot objects.*

### 🌱 Simple

*Beginner - plain language*

**Data retention** is the policy defining **how long records remain** in active objects before being archived or deleted — balancing legal/compliance needs against performance and storage cost.

### 📏 Limits

*Governor & platform limits*

- Recycle Bin holds deleted records for 15 days only.
- Field history retention is 18-24 months without Field Audit Trail.
- Retention policy must be reconciled with GDPR erasure obligations.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Data Purging

*Safely deleting data that's past retention.*

### 🌱 Simple

*Beginner - plain language*

**Data purging** is the controlled **deletion** of records that have passed their retention period — permanently removing them (including from the Recycle Bin) to reclaim storage and keep objects lean.

### 📏 Limits

*Governor & platform limits*

- `Database.emptyRecycleBin` is limited to 10,000 records per call.
- Hard delete via Bulk API bypasses the Recycle Bin irreversibly.
- Cascade deletes count toward the 10,000 DML row limit.
- Purging large volumes triggers sharing recalculation.

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
