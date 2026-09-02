[Home](../index.md) / [06 · SOQL & SOSL](index.md) / **Relationship Queries**

# Relationship Queries

5 topics · Series 6: SOQL & SOSL

**Topics on this page**

- [Child to Parent Dot Notation](#child-to-parent-dot-notation)
- [Multi Level Parent Query](#multi-level-parent-query)
- [Parent to Child Sub Query](#parent-to-child-sub-query)
- [Nested Relationship Query](#nested-relationship-query)
- [Polymorphic WhoId WhatId](#polymorphic-whoid-whatid)

## Child to Parent Dot Notation

*Traversing up a relationship to read parent fields with dot notation.*

### 🌱 Simple

*Beginner - plain language*

**Child-to-parent** queries read fields from a related parent using a dot: `SELECT Name, Account.Name FROM Contact` grabs each Contact's parent Account name in one query.

### 📏 Limits

*Governor & platform limits*

- Up to 5 levels of parent traversal.
- Custom rels use `__r`.
- Many parent fields add heap.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Multi Level Parent Query

*Chaining dot-notation across several parent hops in one query.*

### 🌱 Simple

*Beginner - plain language*

A **multi-level parent query** walks more than one parent up: `Contact.Account.Owner.Name` reads the Account's Owner's name from a Contact — across multiple relationships in a single query.

### 📏 Limits

*Governor & platform limits*

- Max 5 parent levels per query.
- Deep filters can be non-selective.
- Each hop must be a real relationship.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Parent to Child Sub Query

*Retrieving related child records inside a parent query using a nested SELECT.*

### 🌱 Simple

*Beginner - plain language*

A **parent-to-child sub-query** fetches a parent's children in one go: `SELECT Name, (SELECT LastName FROM Contacts) FROM Account` returns each Account with its Contacts nested inside.

### 📏 Limits

*Governor & platform limits*

- Max 20 child sub-queries; 1 level down.
- Child rows count toward 50k + heap.
- Must use the child relationship name.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Nested Relationship Query

*Combining parent traversal and child sub-queries to shape rich result trees.*

### 🌱 Simple

*Beginner - plain language*

A **nested relationship query** mixes both directions — child sub-queries plus parent dot-notation — to return a structured tree of related data in one query.

### 📏 Limits

*Governor & platform limits*

- 1 level down, 5 levels up.
- 20 sub-queries, 50k rows, heap shared across the tree.
- Outer query must be selective.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Polymorphic WhoId WhatId

*Querying relationship fields that can point to multiple different object types.*

### 🌱 Simple

*Beginner - plain language*

**Polymorphic** fields like Task/Event's `WhoId` (Lead or Contact) and `WhatId` (Account, Opportunity, etc.) can relate to **several object types**. SOQL handles them specially with `TYPEOF` or the `.Type` field.

### 📏 Limits

*Governor & platform limits*

- TYPEOF only on polymorphic relationships.
- Type-aware logic required in Apex/reports.
- Supported parent types are field-defined.

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
