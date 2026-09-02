[Home](../index.md) / [07 · Apex Fundamentals](index.md) / **DML**

# DML

9 topics · Series 7: Apex Fundamentals

**Topics on this page**

- [Insert](#insert)
- [Update](#update)
- [Delete](#delete)
- [Undelete](#undelete)
- [Upsert](#upsert)
- [Merge](#merge)
- [Database.insert](#database-insert)
- [Database.update](#database-update)
- [allOrNone](#allornone)

## Insert

*Creating new records in the database.*

### 🌱 Simple

*Beginner - plain language*

`insert` creates new records: `insert new Account(Name='Acme');`. Pass a List to insert many at once (bulk).

### 📏 Limits

*Governor & platform limits*

- 150 DML statements and 10,000 rows per transaction.
- Insert fails entirely on any row error unless you use `Database.insert(list, false)`.
- Cannot insert setup and non-setup objects in the same transaction (Mixed DML).
- Triggers fire in chunks of 200 regardless of how many rows you insert.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Update

*Modifying existing records by Id.*

### 🌱 Simple

*Beginner - plain language*

`update` saves changes to existing records (matched by **Id**): `acct.Name='New'; update acct;`. Bulk-update a List for many records.

### 📏 Limits

*Governor & platform limits*

- 150 statements / 10,000 rows per transaction.
- Updating a master-detail child locks the parent for the whole transaction.
- Read-only fields (formula, roll-up, auto-number, system audit) cannot be updated.
- Concurrent updates to the same row throw `UNABLE_TO_LOCK_ROW` after ~10 seconds.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Delete

*Removing records to the Recycle Bin.*

### 🌱 Simple

*Beginner - plain language*

`delete` removes records (by Id), sending them to the **Recycle Bin**: `delete acct;`. Bulk-delete a List.

### 📏 Limits

*Governor & platform limits*

- Deleted records go to the Recycle Bin for 15 days; capacity is 25x data storage.
- Cascade-deleted children count toward the 10,000 DML row limit.
- `Database.emptyRecycleBin` is limited to 10,000 records per call.
- Records referenced by a required lookup or in an active approval cannot be deleted.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Undelete

*Restoring records from the Recycle Bin.*

### 🌱 Simple

*Beginner - plain language*

`undelete` restores soft-deleted records from the Recycle Bin: `undelete acct;` (you need the record/Id). It brings them back with their relationships.

### 📏 Limits

*Governor & platform limits*

- Only works within the 15-day Recycle Bin window.
- Fires `after undelete` triggers, which most handlers never implement.
- Hard-deleted records (Bulk API hard delete) cannot be undeleted at all.
- Cascade-restored children count toward DML row limits.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Upsert

*Insert-or-update in one operation, keyed by Id or an External Id.*

### 🌱 Simple

*Beginner - plain language*

`upsert` inserts records without an Id and updates those with one — in a single call: `upsert records Field__c;` using an External Id as the match key.

### 📏 Limits

*Governor & platform limits*

- The External Id field must be marked External Id or Unique.
- Throws if the External Id value matches more than one existing record.
- Counts as one DML statement but the rows count normally.
- Upsert on a non-unique field is not supported and fails at compile or runtime.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Merge

*Combining up to three duplicate records into one master.*

### 🌱 Simple

*Beginner - plain language*

`merge` combines duplicate records (Leads, Contacts, Accounts, Cases) into one **master**, re-parenting related records: `merge master duplicate;` (up to 3 records total).

### 📏 Limits

*Governor & platform limits*

- Supported only on Account, Contact, Lead and Case.
- Maximum 3 records per merge call (1 master + 2 to merge).
- All records must be the same object type; master-detail children are reparented.
- Merge fires delete and update triggers and is not reversible.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Database.insert

*Database methods for DML with options like partial success and assignment rules.*

### 🌱 Simple

*Beginner - plain language*

`Database.insert(list, false)` does the same as `insert` but with **options** — notably **partial success** (don't roll back everything on one failure) and returns result objects.

### 📏 Limits

*Governor & platform limits*

- `Database.insert(list, false)` allows partial success and returns `SaveResult[]`.
- You must inspect `getErrors()` yourself - failures are silent otherwise.
- Still bounded by 150 statements / 10,000 rows.
- `Database.DMLOptions` adds assignment rule, duplicate rule and email header control.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Database.update

*Database.update with partial-success and options for resilient updates.*

### 🌱 Simple

*Beginner - plain language*

`Database.update(list, false)` updates records with **partial success** and returns `SaveResult[]` — valid records save even if some fail, unlike the all-or-none `update` keyword.

### 📏 Limits

*Governor & platform limits*

- Partial success with `allOrNone = false`; returns per-row `SaveResult`.
- Same 150 statement / 10,000 row limits as plain DML.
- Partial success does not roll back already-committed rows in the same statement.
- Row locks are still taken per record - sort by parent to reduce contention.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## allOrNone

*The flag controlling whether one failed record rolls back the whole DML.*

### 🌱 Simple

*Beginner - plain language*

**allOrNone** is the boolean on `Database.*` methods: `true` (default) rolls back everything if any record fails; `false` commits the good records and reports failures.

### 📏 Limits

*Governor & platform limits*

- `false` commits successful rows and reports failures; `true` (default) rolls the statement back.
- Even with `false`, an uncaught exception later in the transaction still rolls everything back.
- You must log `SaveResult` errors explicitly or failures disappear.
- Does not change the 150 statement / 10,000 row limits.

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
