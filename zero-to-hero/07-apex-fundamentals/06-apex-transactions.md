[Home](../index.md) / [07 · Apex Fundamentals](index.md) / **Apex Transactions**

# Apex Transactions

3 topics · Series 7: Apex Fundamentals

**Topics on this page**

- [Savepoint](#savepoint)
- [Rollback](#rollback)
- [Transaction Control](#transaction-control)

## Savepoint

*A rollback marker capturing database state mid-transaction.*

### 🌱 Simple

*Beginner - plain language*

A **Savepoint** marks a point in the transaction you can roll back to: `Savepoint sp = Database.setSavepoint();` ... `Database.rollback(sp);`. It undoes DML done after the savepoint.

### 📏 Limits

*Governor & platform limits*

- Maximum 5 savepoints per transaction.
- Rolling back to a savepoint releases every savepoint created after it.
- Savepoints do not roll back callouts, emails sent, or Platform Events published with Publish Immediately.
- Each `setSavepoint()` counts as a DML statement against the 150 limit.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Rollback

*Reverting database changes to a savepoint or via uncaught exception.*

### 🌱 Simple

*Beginner - plain language*

**Rollback** undoes database changes. Either explicitly with `Database.rollback(savepoint)`, or automatically when an **uncaught exception** ends the transaction — all DML is reverted.

### 📏 Limits

*Governor & platform limits*

- `Database.rollback(sp)` reverts DML but not Id values already assigned to in-memory sObjects.
- Cannot undo callouts, emails, or immediately-published Platform Events.
- An uncaught exception rolls back the whole transaction automatically.
- Governor limits consumed before the rollback are not restored.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Transaction Control

*Designing transaction boundaries, atomicity, and async hand-offs.*

### 🌱 Simple

*Beginner - plain language*

**Transaction control** is managing what counts as one atomic unit of work — where it commits, where it rolls back, and when work is deferred to a new transaction (async).

### 📏 Limits

*Governor & platform limits*

- Apex has no explicit commit - the transaction commits when the outermost context completes successfully.
- Async boundaries (future, Queueable, Batch) create new transactions with fresh limits and separate commits.
- Callouts are blocked once uncommitted DML exists.
- Platform Events with Publish After Commit are the safe way to signal downstream systems.

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
