[Home](../index.md) / [14 · DevOps & SFDX](index.md) / **Scratch Orgs**

# Scratch Orgs

4 topics · Series 14: DevOps & SFDX

**Topics on this page**

- [Create](#create)
- [Delete](#delete)
- [Org Shape](#org-shape)
- [Snapshots](#snapshots)

## Create

*Spinning up a disposable, source-defined org.*

### 🌱 Simple

*Beginner - plain language*

A **scratch org** is a temporary, disposable Salesforce org built from a config file. `sf org create scratch` (via a Dev Hub) spins one up for development or testing.

### 📏 Limits

*Governor & platform limits*

- Scratch org lifetime maximum 30 days, default 7.
- Active and daily scratch org counts are capped by Dev Hub edition.
- Org shape and features must be declared in the definition file - not everything is available.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Delete

*Tearing down scratch orgs to free allocations.*

### 🌱 Simple

*Beginner - plain language*

**Deleting** a scratch org (`sf org delete scratch`) tears it down — freeing your Dev Hub's active scratch org allocation. Scratch orgs are meant to be short-lived and disposable.

### 📏 Limits

*Governor & platform limits*

- Deleting a scratch org frees an active slot but not the daily allocation.
- Expired orgs still occupy the active count until deleted.
- Deletion is immediate and irreversible - unsaved metadata is lost.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Org Shape

*Replicating a source org's edition/features/settings.*

### 🌱 Simple

*Beginner - plain language*

**Org Shape** lets you create scratch orgs that mirror an existing org's **edition, features, settings, and limits** — without manually maintaining a big scratch definition file.

### 📏 Limits

*Governor & platform limits*

- Not every edition feature can be enabled via a scratch definition file.
- Org shape from a production org may include features the scratch edition cannot support.
- Shape creation is asynchronous and limited per Dev Hub.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Snapshots

*Pre-built scratch org states for fast, consistent setup.*

### 🌱 Simple

*Beginner - plain language*

A **scratch org snapshot** captures a fully configured scratch org (metadata + data + settings) so new scratch orgs can be created from it **instantly**, skipping lengthy setup.

### 📏 Limits

*Governor & platform limits*

- Scratch org snapshots have a per-Dev-Hub limit and expire.
- Snapshots must be recreated when the base org shape changes.
- Not available in all editions.

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
