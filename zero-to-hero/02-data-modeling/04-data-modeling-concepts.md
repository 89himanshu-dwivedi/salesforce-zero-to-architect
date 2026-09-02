[Home](../index.md) / [02 · Data Modeling](index.md) / **Data Modeling Concepts**

# Data Modeling Concepts

6 topics · Series 2: Data Modeling

**Topics on this page**

- [Cardinality](#cardinality)
- [One to One](#one-to-one)
- [One to Many](#one-to-many)
- [Many to Many](#many-to-many)
- [Normalization](#normalization)
- [Denormalization](#denormalization)

## Cardinality

*How many records on one side relate to how many on the other — 1:1, 1:N, M:N.*

### 🌱 Simple

*Beginner - plain language*

**Cardinality** describes how many records of one object connect to records of another: one-to-one (1:1), one-to-many (1:N), or many-to-many (M:N). It's the core question when designing a relationship.

### 📏 Limits

*Governor & platform limits*

- Salesforce supports 1:1 only by convention - there is no native 1:1 relationship type.
- Many-to-many requires a junction object with two master-detail slots.
- Cardinality decisions are hard to reverse once data exists.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## One to One

*Each record on one side relates to exactly one on the other — not native, must be enforced.*

### 🌱 Simple

*Beginner - plain language*

A **one-to-one** relationship means one record links to exactly one other and vice-versa — like a Person and their single Passport. Salesforce has no built-in 1:1, so you enforce it.

### 📏 Limits

*Governor & platform limits*

- Not natively supported - enforced by a unique lookup plus a validation rule or trigger.
- Uniqueness enforcement at the database level requires a Unique field, not a lookup.
- Integrations can violate the constraint unless it is enforced in the schema.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## One to Many

*One parent record relates to many children — the most common relationship.*

### 🌱 Simple

*Beginner - plain language*

**One-to-many** means one parent has many children, each child has one parent — like one Account with many Contacts. It's the most common relationship in Salesforce.

### 📏 Limits

*Governor & platform limits*

- More than ~10,000 children per parent causes data skew and lock contention.
- Sub-query rows count toward the 50,000 transaction row limit.
- Related lists on skewed parents are slow to render.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Many to Many

*Records on both sides relate to multiple on the other — implemented via a junction.*

### 🌱 Simple

*Beginner - plain language*

**Many-to-many** means each record on both sides can relate to many on the other — Students↔Courses. Salesforce models this with a **junction object** between them.

### 📏 Limits

*Governor & platform limits*

- Junction object consumes both master-detail slots.
- Junction volume grows multiplicatively and often becomes the largest object in the org.
- Deleting either parent cascade-deletes junction rows against the 10,000 DML limit.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Normalization

*Splitting data into related tables to remove redundancy — clean, consistent, less duplication.*

### 🌱 Simple

*Beginner - plain language*

**Normalization** organizes data into related objects so each fact is stored once. Instead of repeating an account's address on every contact, store it once on the Account and relate contacts to it.

### 📏 Limits

*Governor & platform limits*

- More objects means more joins, and SOQL supports only 5 levels of parent traversal.
- Report types span a maximum of 4 objects, which normalised models hit quickly.
- Highly normalised models increase query count against the 100-query limit.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Denormalization

*Deliberately duplicating/aggregating data to speed reads and simplify reporting.*

### 🌱 Simple

*Beginner - plain language*

**Denormalization** is the opposite of normalization — intentionally copying or aggregating data so it's quick to read without joins. Example: storing the account name on the case for fast lists and reports.

### 📏 Limits

*Governor & platform limits*

- Duplicated values must be kept in sync by automation, consuming CPU and DML on every save.
- Roll-up summaries are limited to 25 per object.
- Wide denormalised objects slow every query and page load.
- Stale denormalised data is a common and hard-to-detect defect.

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
