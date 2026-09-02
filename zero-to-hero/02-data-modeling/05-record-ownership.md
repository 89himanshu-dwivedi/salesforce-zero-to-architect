[Home](../index.md) / [02 · Data Modeling](index.md) / **Record Ownership**

# Record Ownership

2 topics · Series 2: Data Modeling

**Topics on this page**

- [Owner](#owner)
- [Queue Ownership](#queue-ownership)

## Owner

*The user (or queue) who owns a record — the foundation of sharing and visibility.*

### 🌱 Simple

*Beginner - plain language*

Every record has an **Owner** — usually the user who created it. The owner has full access and is the starting point for who else can see the record (via role hierarchy and sharing rules).

### 📏 Limits

*Governor & platform limits*

- More than 10,000 records owned by one user causes ownership skew.
- Ownership changes trigger sharing recalculation that can run for hours.
- Manual shares are deleted when ownership changes.
- Owner cannot be set to an inactive user.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Queue Ownership

*A queue owns records so a team can share and pull work — common for Cases/Leads.*

### 🌱 Simple

*Beginner - plain language*

A **queue** can own records instead of a single user. Records sit in the queue until a team member **takes ownership** — great for distributing Cases, Leads, or custom-object work to a group.

### 📏 Limits

*Governor & platform limits*

- Queues are supported on a limited set of objects.
- Queue membership changes take group membership locks.
- A record owned by a queue has no user owner, which affects roll-ups and some reports.
- Queue email notifications are subject to org email limits.

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
