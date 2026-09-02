[Home](../index.md) / [13 · Security Deep Dive](index.md) / **Sharing**

# Sharing

4 topics · Series 13: Security Deep Dive

**Topics on this page**

- [Implicit Sharing](#implicit-sharing)
- [Explicit Sharing](#explicit-sharing)
- [Criteria Based Sharing](#criteria-based-sharing)
- [Ownership Based Sharing](#ownership-based-sharing)

## Implicit Sharing

*Automatic, built-in account-related sharing.*

### 🌱 Simple

*Beginner - plain language*

**Implicit sharing** is automatic sharing Salesforce enforces between accounts and their child records (contacts, opportunities, cases) — you can't configure it; it's built into the platform.

### 📏 Limits

*Governor & platform limits*

- Cannot be disabled, queried or inspected.
- Applies between Account and its Contacts, Cases and Opportunities.
- Amplifies the cost of Account data skew.
- Recalculated on ownership and hierarchy changes.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Explicit Sharing

*Sharing you configure: rules, manual, Apex, teams.*

### 🌱 Simple

*Beginner - plain language*

**Explicit sharing** is the sharing you deliberately configure — sharing rules, manual shares, Apex shares, teams — as opposed to implicit (automatic) sharing. It's recorded as share rows.

### 📏 Limits

*Governor & platform limits*

- Stored in `__Share` objects that consume data storage.
- Deleted when the record owner changes for manual shares.
- Share rows can be queried and audited, unlike implicit sharing.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Criteria Based Sharing

*Sharing records by field-value conditions.*

### 🌱 Simple

*Beginner - plain language*

**Criteria-based sharing** grants access to records whose **field values** match defined conditions (e.g., Type = 'Partner') — regardless of who owns them.

### 📏 Limits

*Governor & platform limits*

- Maximum 50 criteria-based rules per object.
- Re-evaluated whenever a referenced field changes - the most expensive rule type.
- Cannot reference formula fields that span relationships in all cases.
- Recalculation is asynchronous and can take hours.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Ownership Based Sharing

*Sharing records based on who owns them.*

### 🌱 Simple

*Beginner - plain language*

**Ownership-based sharing** grants access to records based on their **owner** — sharing records owned by members of one role/group with another role/group.

### 📏 Limits

*Governor & platform limits*

- Counts toward the 300 sharing rules per object.
- Recalculates when ownership changes - mass reassignment is a storm trigger.
- Cheaper than criteria-based but still locks group membership during recalculation.

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
