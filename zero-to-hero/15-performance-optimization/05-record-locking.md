[Home](../index.md) / [15 · Performance Optimization](index.md) / **Record Locking**

# Record Locking

5 topics · Series 15: Performance Optimization

**Topics on this page**

- [Parent Child Locking](#parent-child-locking)
- [Ownership Locking](#ownership-locking)
- [Lookup Skew](#lookup-skew)
- [Account Skew](#account-skew)
- [Ownership Skew](#ownership-skew)

## Parent Child Locking

*Updating a child locks its master-detail parent.*

### 🌱 Simple

*Beginner - plain language*

In a **master-detail** relationship, inserting/updating/deleting a **child** record briefly **locks the parent** (to maintain roll-up integrity). If many children of the same parent are processed concurrently, they contend for that parent lock and fail with `UNABLE_TO_LOCK_ROW`.

### 📏 Limits

*Governor & platform limits*

- Master-detail child DML locks the parent for the whole transaction.
- Lock wait is about 10 seconds before `UNABLE_TO_LOCK_ROW`.
- Reparenting locks both old and new parents.
- Roll-up recalculation extends the lock duration.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Ownership Locking

*Changing ownership locks user/role/group rows.*

### 🌱 Simple

*Beginner - plain language*

Changing a record's **owner** (or ownership-related operations) locks related **user, role, and group** records to recalculate sharing. Mass owner changes on records tied to one user/role cause lock contention.

### 📏 Limits

*Governor & platform limits*

- Ownership changes lock group membership tables broadly.
- Mass reassignment triggers sharing recalculation that can run for hours.
- Deferred sharing calculation must be explicitly re-enabled afterwards.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Lookup Skew

*Too many child records pointing to one lookup target.*

### 🌱 Simple

*Beginner - plain language*

**Lookup skew** happens when a very large number of records point to the **same lookup target** record. Concurrent updates to those children can lock the target, causing contention even though lookups aren't master-detail.

### 📏 Limits

*Governor & platform limits*

- Threshold is around 10,000 records pointing at one lookup target.
- Causes brief locks that serialise concurrent inserts.
- Cannot be resolved by indexing - only by distributing the target values.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Account Skew

*One Account with too many child records (>10k).*

### 🌱 Simple

*Beginner - plain language*

**Account skew** is when a single **Account** has an extremely large number of child records (contacts, opportunities, cases) — typically **more than 10,000**. This causes locking contention and sharing-recalculation problems on that account.

### 📏 Limits

*Governor & platform limits*

- More than 10,000 child records under one Account.
- Amplified by implicit sharing to Contacts, Cases and Opportunities.
- Degrades sharing recalculation and reporting on that branch.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Ownership Skew

*One user/queue owning a huge number of records (>10k).*

### 🌱 Simple

*Beginner - plain language*

**Ownership skew** is when a single **user or queue owns more than ~10,000 records**. Because ownership drives sharing through the role hierarchy, this causes expensive sharing recalculations and locking when that owner's role or data changes.

### 📏 Limits

*Governor & platform limits*

- More than 10,000 records owned by a single user.
- Placing that user outside the role hierarchy reduces recalculation scope.
- Queue ownership generally behaves better than a single user.

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
