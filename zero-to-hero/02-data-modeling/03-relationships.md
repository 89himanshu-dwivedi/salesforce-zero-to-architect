[Home](../index.md) / [02 · Data Modeling](index.md) / **Relationships**

# Relationships

9 topics · Series 2: Data Modeling

**Topics on this page**

- [Optional Lookup](#optional-lookup)
- [Required Lookup](#required-lookup)
- [Lookup Filter](#lookup-filter)
- [Master Detail](#master-detail)
- [Cascade Delete](#cascade-delete)
- [Rollup Summary](#rollup-summary)
- [Junction Object (Many to Many)](#junction-object-many-to-many)
- [Hierarchical](#hierarchical)
- [Self Relationship](#self-relationship)

## Optional Lookup

*A loose link to another record that may be empty — independent records, flexible.*

### 🌱 Simple

*Beginner - plain language*

An **optional lookup** connects one record to another, but the link can be left blank. The two records are independent — deleting the parent doesn't delete the child (the field just clears).

### 📏 Limits

*Governor & platform limits*

- Deleting the parent clears the lookup rather than deleting the child.
- No roll-up summaries and no sharing inheritance.
- Lookup skew above ~10,000 records per target causes lock contention.
- Maximum 40 lookups per object.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Required Lookup

*A lookup that must be filled — enforces a relationship without master-detail coupling.*

### 🌱 Simple

*Beginner - plain language*

A **required lookup** is a lookup field you must populate to save the record. It enforces that every child points to a parent, but keeps the records otherwise independent (unlike master-detail).

### 📏 Limits

*Governor & platform limits*

- Deleting the parent is blocked while children exist, or cascades depending on configuration.
- Still provides no roll-up summaries or sharing inheritance.
- Required-ness is enforced at the UI and API layer, so integrations must supply it.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Lookup Filter

*Restricts which records a lookup can select via criteria — data quality at the relationship level.*

### 🌱 Simple

*Beginner - plain language*

A **lookup filter** limits which records appear/are valid in a lookup field. For example, only let users pick *Active* accounts, or contacts from the *same* account.

### 📏 Limits

*Governor & platform limits*

- Maximum 5 lookup filters per object.
- Filters apply in the UI and to API writes when marked required, which can break integrations.
- Cross-object filter criteria are limited in depth.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Master Detail

*Tight parent-child link: child inherits sharing/owner, cascade delete, enables roll-ups.*

### 🌱 Simple

*Beginner - plain language*

A **master-detail** relationship tightly binds a child to a parent. The child has no owner of its own (it inherits the parent's), is deleted when the parent is (cascade), and the parent can **roll up** totals from children.

### 📏 Limits

*Governor & platform limits*

- Maximum 2 master-detail relationships per object; 3 levels of custom relationships.
- Child inherits parent sharing and cannot have its own OWD.
- Child DML locks the parent for the whole transaction.
- Converting to lookup is only possible when no roll-ups exist.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Cascade Delete

*Deleting a master-detail parent automatically deletes its children — and can chain.*

### 🌱 Simple

*Beginner - plain language*

**Cascade delete** means when you delete a parent record in a master-detail relationship, all its child records are deleted too — automatically.

### 📏 Limits

*Governor & platform limits*

- Cascade-deleted rows count toward the 10,000 DML row limit.
- Deep chains can cascade far more rows than expected.
- Cascade deletes fire child delete triggers.
- Restoring from the Recycle Bin fires `after undelete`, which most handlers ignore.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Rollup Summary

*Parent-side aggregate of master-detail children (COUNT/SUM/MIN/MAX) — see also Field Types.*

### 🌱 Simple

*Beginner - plain language*

A **Roll-Up Summary** field on a master record automatically aggregates its detail (child) records — count them, or sum/min/max a field. It updates as children change.

### 📏 Limits

*Governor & platform limits*

- Maximum 25 per object.
- Only on master-detail relationships (or via DLRS on lookups).
- Recalculation locks the parent record and is a primary cause of lock contention.
- Roll-up updates do not fire triggers.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Junction Object (Many to Many)

*A child with two master-detail links — implements many-to-many between two objects.*

### 🌱 Simple

*Beginner - plain language*

A **junction object** sits between two objects to create a **many-to-many** relationship. Example: a *Course* has many *Students* and a *Student* takes many *Courses* — a `Enrollment` junction links them.

### 📏 Limits

*Governor & platform limits*

- Requires two master-detail relationships, consuming both slots on the junction object.
- The first master-detail defines record ownership and sharing.
- Deleting either parent cascade-deletes the junction record.
- Junction records at volume create skew on both parents.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Hierarchical

*Special lookup only on the User object — links a user to another user (e.g., Manager).*

### 🌱 Simple

*Beginner - plain language*

A **hierarchical relationship** is a special lookup available *only on the User object*. It links one user to another — most commonly the **Manager** field — to model org structure.

### 📏 Limits

*Governor & platform limits*

- Available on the User object only.
- Cannot be used for roll-up summaries.
- Self-referencing loops are prevented but deep chains are slow to traverse.
- Limited to 5 levels in SOQL relationship traversal.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Self Relationship

*A lookup from an object to itself — hierarchies like Parent Account or Parent Case.*

### 🌱 Simple

*Beginner - plain language*

A **self relationship** is a lookup from an object back to the *same* object — like an Account's **Parent Account**, building a hierarchy of records of the same type.

### 📏 Limits

*Governor & platform limits*

- SOQL supports only 5 levels of parent traversal, so deep trees cannot be queried in one statement.
- Recursive traversal in Apex is bounded by CPU and heap.
- Circular references must be prevented by validation - the platform does not stop them on lookups.

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
