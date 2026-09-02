[Home](../index.md) / [08 · Apex Advanced](index.md) / **Triggers**

# Triggers

5 topics · Series 8: Apex Advanced

**Topics on this page**

- [Before Insert/Update/Delete](#before-insert-update-delete)
- [After Insert/Update/Delete/Undelete](#after-insert-update-delete-undelete)
- [Trigger.new / Trigger.old](#trigger-new-trigger-old)
- [Trigger.newMap / Trigger.oldMap](#trigger-newmap-trigger-oldmap)
- [Trigger.isInsert / isUpdate / isDelete](#trigger-isinsert-isupdate-isdelete)

## Before Insert/Update/Delete

*Before-context trigger events for in-record validation and defaulting.*

### 🌱 Simple

*Beginner - plain language*

**Before triggers** fire *before* the database save. Use **before insert/update** to set/validate field values directly on the records (no DML needed), and **before delete** to block or prepare for deletion.

### 📏 Limits

*Governor & platform limits*

- Fires in chunks of 200; limits are shared across the whole transaction.
- `Trigger.old` is unavailable on insert; record Ids do not exist yet in before insert.
- Field assignment is free (no DML) but read-only fields still cannot be set.
- Before-save Flows run *before* before-triggers - your assumption about order may be wrong.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## After Insert/Update/Delete/Undelete

*After-context events for related-record DML, rollups, and integrations.*

### 🌱 Simple

*Beginner - plain language*

**After triggers** fire *after* the save, when records have **Ids** and are read-only. Use them to create/update **related** records, roll up values, send notifications, or fire integrations.

### 📏 Limits

*Governor & platform limits*

- Records are read-only; changing a field requires an extra DML against the 150/10,000 limits.
- `after undelete` only fires within the 15-day Recycle Bin window.
- `Trigger.new` is unavailable on delete; use `Trigger.old`.
- Related-record DML here is where most recursion and lock contention originates.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Trigger.new / Trigger.old

*The context lists holding the new and prior versions of records.*

### 🌱 Simple

*Beginner - plain language*

`Trigger.new` is the list of records being saved (new values); `Trigger.old` is the list of their **prior** values (update/delete only). Compare them to detect changes.

### 📏 Limits

*Governor & platform limits*

- Maximum 200 records per invocation.
- `Trigger.new` is null in delete contexts; `Trigger.old` is null in insert contexts.
- `Trigger.new` is read-only in after contexts and mutable in before contexts.
- Holding these collections in statics across chunks causes heap growth and stale data.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Trigger.newMap / Trigger.oldMap

*Id-keyed maps of the records for O(1) lookups and comparison.*

### 🌱 Simple

*Beginner - plain language*

`Trigger.newMap` and `Trigger.oldMap` are **Maps keyed by record Id** of the new and old versions — giving O(1) access to any record by Id for comparison and relation.

### 📏 Limits

*Governor & platform limits*

- `Trigger.newMap` is null in before insert - Ids do not exist yet.
- Both are capped at 200 entries per invocation.
- Map keys are 18-character Ids; comparing against 15-character values fails silently.
- Field-change detection via oldMap is the cheapest recursion guard available.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Trigger.isInsert / isUpdate / isDelete

*Context variables that tell which event and timing fired the trigger.*

### 🌱 Simple

*Beginner - plain language*

Context booleans — `Trigger.isInsert`, `isUpdate`, `isDelete`, `isUndelete`, plus `isBefore`/`isAfter` — tell your code **which event** and **timing** fired, so one trigger can route to the right logic.

### 📏 Limits

*Governor & platform limits*

- Multiple context booleans can be true in a single execution path after workflow field updates.
- Workflow field updates re-fire update triggers exactly once.
- Context variables are unavailable outside trigger execution - `Trigger.isExecuting` guards handler reuse.
- Lead conversion and cascade operations follow different context paths than you expect.

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
