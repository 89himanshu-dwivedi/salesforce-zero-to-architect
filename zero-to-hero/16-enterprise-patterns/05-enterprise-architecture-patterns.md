[Home](../index.md) / [16 · Enterprise Patterns](index.md) / **Enterprise Architecture Patterns**

# Enterprise Architecture Patterns

7 topics · Series 16: Enterprise Patterns

**Topics on this page**

- [Presentation Layer](#presentation-layer)
- [Controller Layer](#controller-layer)
- [Service Layer](#service-layer)
- [Domain Layer](#domain-layer)
- [Data Layer](#data-layer)
- [Repository Pattern](#repository-pattern)
- [Unit of Work Pattern](#unit-of-work-pattern)

## Presentation Layer

*UI components; no business logic or data access.*

### 🌱 Simple

*Beginner - plain language*

The **Presentation Layer** is the UI — LWC, Aura, Visualforce, screen flows. Its only job is **display and user interaction**; it holds no business rules and never queries or does DML directly.

### 📏 Limits

*Governor & platform limits*

- Render + interact only; delegate to services.
- UI validation = UX; authoritative rules server-side.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Controller Layer

*Thin entry point marshalling between UI and services.*

### 🌱 Simple

*Beginner - plain language*

The **Controller Layer** is the thin bridge between UI and business logic — `@AuraEnabled` Apex methods (or REST resources) that receive requests, call the **service layer**, and shape responses. It contains **no business logic**.

### 📏 Limits

*Governor & platform limits*

- Marshal, delegate to one service, translate errors, return DTO.
- cacheable reads; reused across entry points.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Service Layer

*Coarse-grained business use cases; transaction boundary.*

### 🌱 Simple

*Beginner - plain language*

The **Service Layer** holds **coarse-grained business operations** (use cases like `placeOrder`, `convertLead`) and owns the **transaction boundary**. It orchestrates domain, selector, and unit-of-work classes.

### 📏 Limits

*Governor & platform limits*

- Coarse use cases; owns UoW/transaction; cross-object.
- Stateless, bulkified, interface + factory; reused.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Domain Layer

*Record-level business behavior and validation per object.*

### 🌱 Simple

*Beginner - plain language*

The **Domain Layer** encapsulates the **behavior and rules of a single sObject** — validation, defaulting, and record-level logic. It's the object-oriented model of your data (e.g., an `Accounts` domain class).

### 📏 Limits

*Governor & platform limits*

- Single-object record rules/validation; bulkified.
- Triggers delegate; fflib_SObjectDomain; mockable.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Data Layer

*All persistence: queries and DML, isolated from logic.*

### 🌱 Simple

*Beginner - plain language*

The **Data Layer** isolates all **persistence** — SOQL queries and DML — from business logic. In Salesforce this is realized through **Selector** classes (reads) and **Unit of Work** (writes), so logic never touches the database directly.

### 📏 Limits

*Governor & platform limits*

- Selectors (reads) + UoW (writes) behind interfaces.
- Mockable, FLS-aware, bulkified, consistent.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Repository Pattern

*Collection-like abstraction over data access (Selector).*

### 🌱 Simple

*Beginner - plain language*

The **Repository pattern** provides a **collection-like interface** for accessing domain objects, hiding the query details. In Salesforce, the **Selector** layer (fflib) is the repository — each object has a selector that returns records by criteria.

### 📏 Limits

*Governor & platform limits*

- Per-object selector; encapsulated SOQL; FLS central.
- Mockable via factory; fflib_SObjectSelector base.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Unit of Work Pattern

*Track changes; commit all DML once, in dependency order.*

### 🌱 Simple

*Beginner - plain language*

The **Unit of Work** tracks all record changes during a transaction and **commits them together in the correct order** with minimal DML statements — resolving parent/child relationships automatically.

### 📏 Limits

*Governor & platform limits*

- Track changes; commit once in dependency order.
- Atomic (savepoint), bundled DML, mockable; service owns it.

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
