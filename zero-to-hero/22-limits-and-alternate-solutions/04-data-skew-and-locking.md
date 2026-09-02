[Home](../index.md) / [22 · Limits & Alternate Solutions](index.md) / **Data Skew & Locking**

# Data Skew & Locking

8 topics · Series 22: Limits & Alternate Solutions

**Topics on this page**

- [Account Data Skew](#account-data-skew)
- [Ownership Skew](#ownership-skew)
- [Lookup Skew](#lookup-skew)
- [UNABLE_TO_LOCK_ROW](#unable-to-lock-row)
- [Parent Record Locking](#parent-record-locking)
- [Concurrent Update Conflict](#concurrent-update-conflict)
- [Mixed DML Error](#mixed-dml-error)
- [Implicit Sharing Locks](#implicit-sharing-locks)

## Account Data Skew

*>10,000 children under one parent → locks & slow sharing.*

### 🌱 Simple

*Beginner - plain language*

**Account data skew** = one Account (or any parent) with more than ~**10,000 child records** (Contacts, Opportunities, Cases). When children change, Salesforce locks the parent and recalculates sharing — so concurrent updates collide (`UNABLE_TO_LOCK_ROW`) and saves slow. The alternate is to **distribute children across more parents**.

### 📏 Limits

*Governor & platform limits*

- >10,000 child records per parent.
- Lock wait around 10 seconds.
- Affects master-detail and lookup (implicit sharing) alike.
- Cannot be resolved by indexing.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Ownership Skew

*One user owns huge numbers of records → recalc pain.*

### 🌱 Simple

*Beginner - plain language*

**Ownership skew** = a single user (often an integration or admin user) **owns a very large number of records** (>~10,000). Because ownership drives sharing via the role hierarchy, any change to that user's role/group triggers **massive sharing recalculation**, and their records become a locking hotspot.

### 📏 Limits

*Governor & platform limits*

- >10,000 records owned by one user.
- Ownership change triggers sharing recalculation.
- Users outside the role hierarchy reduce recalculation scope.
- Queue ownership is generally cheaper.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Lookup Skew

*Too many records pointing to one lookup target → record locks.*

### 🌱 Simple

*Beginner - plain language*

**Lookup skew** = a very large number of records all referencing the **same value in a lookup field** (e.g. every Case linked to one "Default" product). Saving those records can lock the looked-up record, causing contention. Alternate: **spread lookups across more target values** or reduce concurrency.

### 📏 Limits

*Governor & platform limits*

- Threshold around 10,000 records per lookup target.
- Locks are brief but serialise concurrent inserts.
- No cascade delete implications.
- Cannot be indexed away.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## UNABLE_TO_LOCK_ROW

*The classic concurrency error — causes and every fix.*

### 🌱 Simple

*Beginner - plain language*

`UNABLE_TO_LOCK_ROW` means two transactions tried to lock the **same record (or its parent)** at once. It's the headline symptom of **data skew, parallel updates, and rollup/sharing cascades**. The fix is to **reduce contention**: serialize, shrink batches, and remove skew.

### 📏 Limits

*Governor & platform limits*

- Lock wait around 10 seconds.
- Locks are not queryable - only inferable from errors.
- `FOR UPDATE` holds the lock for the transaction and cannot be combined with ORDER BY.
- Retry is a mitigation, not a fix.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Parent Record Locking

*Child DML implicitly locks the parent — design around it.*

### 🌱 Simple

*Beginner - plain language*

Inserting/updating a child in a Master-Detail (and sometimes lookup) relationship **implicitly locks the parent** to keep rollups/sharing consistent. If many children of the same parent are processed in parallel, they fight over that parent lock. Alternate: **group children by parent and process each parent's children together/serially**.

### 📏 Limits

*Governor & platform limits*

- Master-detail child DML locks the parent for the transaction.
- Reparenting locks two parents.
- Roll-up recalculation extends the lock.
- Lock wait ~10 seconds.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Concurrent Update Conflict

*Two users/processes edit the same record simultaneously.*

### 🌱 Simple

*Beginner - plain language*

When two transactions update the **same record** at the same time, one wins and the other can fail or overwrite. In Apex, design for it with **`FOR UPDATE` locking, smaller scopes, and retry-on-conflict**; in integrations, use **idempotency and optimistic concurrency**.

### 📏 Limits

*Governor & platform limits*

- No automatic optimistic locking on the API.
- `FOR UPDATE` holds a lock for the whole transaction.
- Lock wait ~10 seconds.
- Field-level merge is not provided - last write wins.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Mixed DML Error

*Can't DML setup + non-setup objects in one transaction.*

### 🌱 Simple

*Beginner - plain language*

The **Mixed DML** error fires when you modify a **setup object** (User, Group, Permission Set assignment, Territory) and a **non-setup object** (Account, Contact) in the *same* transaction. The fix is to **separate them** — do one in the main transaction and the other in an async method.

### 📏 Limits

*Governor & platform limits*

- Setup objects include User, Group, GroupMember, PermissionSet, PermissionSetAssignment, Queue.
- `System.runAs` works only in tests.
- Async separation is the production fix.
- Some standard objects behave as setup objects unexpectedly.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Implicit Sharing Locks

*Sharing recalcs take internal locks that block other writes.*

### 🌱 Simple

*Beginner - plain language*

Sharing maintenance (from ownership changes, role edits, sharing-rule changes) takes **internal locks**. If these run while you're doing bulk DML, operations **serialize and slow down or deadlock**. Alternate: **schedule sharing-affecting changes separately from heavy DML, and use Defer Sharing Calculation for big operations**.

### 📏 Limits

*Governor & platform limits*

- Cannot be disabled or queried.
- Recalculated on ownership and hierarchy changes.
- Amplifies the cost of Account skew.
- Applies to standard Account children only.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

---

## Connect

These pages carry the **definitions and limits** only. The advanced depth, real-world
scenarios, error playbooks, best-option reasoning and interview questions are kept aside.

If you would like them, or you want to talk about anything on this page:

- **LinkedIn** - [in/himanshukumar-sf](https://www.linkedin.com/in/himanshukumar-sf/)
- **X** - [@kum60094](https://x.com/kum60094)
- **GitHub** - [89himanshu-dwivedi](https://github.com/89himanshu-dwivedi)
- **Email** - [himanshu.jee.1996@gmail.com](mailto:himanshu.jee.1996@gmail.com)

*- Himanshu Kumar*
