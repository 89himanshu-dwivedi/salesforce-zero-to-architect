[Home](../index.md) / [08 · Apex Advanced](index.md) / **Custom Metadata**

# Custom Metadata

3 topics · Series 8: Apex Advanced

**Topics on this page**

- [CMDT](#cmdt)
- [Hierarchy Custom Settings](#hierarchy-custom-settings)
- [List Custom Settings](#list-custom-settings)

## CMDT

*Deployable, queryable configuration records that don't count against DML/SOQL limits.*

### 🌱 Simple

*Beginner - plain language*

**Custom Metadata Types (CMDT)** are like custom objects but their **records are metadata** — deployable between orgs and queryable in Apex without consuming SOQL query rows. Perfect for app configuration.

### 📏 Limits

*Governor & platform limits*

- `getInstance()` and `getAll()` are cached and consume no SOQL.
- Records are metadata and can be deployed, unlike Custom Setting values.
- Cannot be inserted or updated with standard DML - use the Metadata API.
- Field types are restricted (no formula, no roll-up, limited relationships).

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Hierarchy Custom Settings

*Org/profile/user-tiered settings resolved automatically by context.*

### 🌱 Simple

*Beginner - plain language*

**Hierarchy Custom Settings** store values that can be set at **org, profile, or user** level, with the most specific level winning automatically. Read via `MySetting__c.getInstance()` — resolved for the running user.

### 📏 Limits

*Governor & platform limits*

- Values do not deploy between orgs - they must be created per org.
- `getInstance()` is cached and consumes no SOQL.
- Resolution order is org default, then profile, then user.
- Counts toward the org's data storage and the 10 MB custom settings cache limit.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## List Custom Settings

*Flat, named sets of org-wide reference data cached for fast access.*

### 🌱 Simple

*Beginner - plain language*

**List Custom Settings** hold a flat list of named records of org-wide reference data (no hierarchy). Read all via `MySetting__c.getAll()` or one by name via `getInstance(name)` — cached, no SOQL cost.

### 📏 Limits

*Governor & platform limits*

- Values do not deploy; `getAll()` is cached and SOQL-free.
- Total custom settings cache is limited (typically 10 MB org-wide).
- No record types, no relationships, and limited field types.
- Custom Metadata is the preferred modern replacement.

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
