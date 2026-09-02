[Home](../index.md) / [07 · Apex Fundamentals](index.md) / **Apex Classes**

# Apex Classes

5 topics · Series 7: Apex Fundamentals

**Topics on this page**

- [Utility Classes](#utility-classes)
- [Service Classes](#service-classes)
- [Helper Classes](#helper-classes)
- [Wrapper Classes](#wrapper-classes)
- [DTO Classes](#dto-classes)

## Utility Classes

*Stateless collections of reusable static helper methods.*

### 🌱 Simple

*Beginner - plain language*

A **utility class** groups reusable, **stateless static methods** — e.g., `StringUtils.isBlank(s)`, `DateUtils.businessDays(a,b)`. No instance state; just pure helpers shared across the codebase.

### 📏 Limits

*Governor & platform limits*

- Static-only classes hold no state, so they are safe across batch chunks.
- Static members still count toward the 6 MB heap for the duration of the transaction.
- Utility classes are hard to mock - prefer instance methods behind an interface for anything with dependencies.
- All code counts toward the 6 MB org Apex character limit.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Service Classes

*The business-logic layer orchestrating use cases across objects.*

### 🌱 Simple

*Beginner - plain language*

A **service class** holds **business logic** for a use case — e.g., `OrderService.submitOrders(orders)`. It coordinates queries, calculations, and DML behind a clean, bulk-safe API.

### 📏 Limits

*Governor & platform limits*

- Service methods should accept collections - a single-record signature will fail on bulk loads.
- All governor limits are shared across the layers a service calls.
- The service is the natural transaction boundary; `Savepoint` is limited to 5 per transaction.
- Avoid `@AuraEnabled` directly on services so security checks stay explicit in the controller.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Helper Classes

*Focused classes that assist a specific class or feature.*

### 🌱 Simple

*Beginner - plain language*

A **helper class** offloads detailed logic for a specific feature/handler — e.g., `AccountTriggerHelper` holding the methods a trigger handler calls. It keeps the main class lean.

### 📏 Limits

*Governor & platform limits*

- Helpers that hold static mutable state cause cross-record bleed within a transaction.
- Statics reset between transactions and batch chunks - never use them for durable state.
- Helper sprawl makes CPU attribution in debug logs harder; keep the call graph shallow on hot paths.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Wrapper Classes

*Custom objects bundling related data (often record + UI state) for display or transport.*

### 🌱 Simple

*Beginner - plain language*

A **wrapper class** bundles related data into one object — e.g., a record plus a `Boolean selected` flag for a UI table, or several fields combined for a component.

### 📏 Limits

*Governor & platform limits*

- Wrappers used in Visualforce count toward the 170 KB view state unless `transient`.
- Large wrapper lists are a common heap failure - 6 MB sync / 12 MB async.
- For `Set`/`Map` membership, wrappers must override `equals()` and `hashCode()`.
- Wrappers returned to LWC must have `@AuraEnabled` on every exposed member.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## DTO Classes

*Data Transfer Objects — plain data containers for integration and serialization.*

### 🌱 Simple

*Beginner - plain language*

A **DTO (Data Transfer Object)** is a plain class holding data to **transfer** between systems/layers — e.g., mapping an external JSON payload to typed Apex fields with no behavior.

### 📏 Limits

*Governor & platform limits*

- JSON serialisation and deserialisation are CPU-intensive on large structures.
- Callout request and response bodies cap at 6 MB sync / 12 MB async.
- `JSON.deserializeStrict` throws on unknown fields - vendors adding a field will break you.
- Reserved Apex words cannot be field names; use `JSON.deserializeUntyped` or a mapping layer.

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
