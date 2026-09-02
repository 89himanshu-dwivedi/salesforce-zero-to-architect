[Home](../index.md) / [16 · Enterprise Patterns](index.md) / **fflib Framework**

# fflib Framework

8 topics · Series 16: Enterprise Patterns

**Topics on this page**

- [Apex Common](#apex-common)
- [Application Class](#application-class)
- [Service Classes](#service-classes)
- [Selector Classes](#selector-classes)
- [Domain Classes](#domain-classes)
- [Unit of Work](#unit-of-work)
- [IOC Container](#ioc-container)
- [Dependency Injection](#dependency-injection)

## Apex Common

*FinancialForce's open-source enterprise patterns library.*

### 🌱 Simple

*Beginner - plain language*

**Apex Common** (`fflib-apex-common`) is FinancialForce's open-source library implementing Martin Fowler's enterprise patterns for Apex: **Application factory, Service, Domain, Selector, Unit of Work**. It's the de-facto standard for layered Salesforce architecture.

### 📏 Limits

*Governor & platform limits*

- Application/Service/Domain/Selector/UoW base classes.
- Built on ApexMocks; for complex, multi-team orgs.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Application Class

*Central factory wiring layers for dependency injection.*

### 🌱 Simple

*Beginner - plain language*

The **Application class** is a central **factory** that produces instances of your services, domains, selectors, and unit of work. Everything is created through it, so dependencies can be **swapped for mocks** in tests.

### 📏 Limits

*Governor & platform limits*

- Static factory state is transaction-scoped and resets between async transactions.
- Mock injection works only in test context.
- A central Application class becomes a merge-conflict hotspot across teams.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Service Classes

*fflib service layer: interface + impl, UoW-owning.*

### 🌱 Simple

*Beginner - plain language*

**fflib Service classes** implement the service layer: an **interface** (`IOrderService`) plus an **implementation** (`OrderServiceImpl`), registered in the Application factory. They hold coarse-grained, bulkified business use cases and own the Unit of Work.

### 📏 Limits

*Governor & platform limits*

- Service methods must accept collections - single-record signatures fail on bulk.
- All governor limits are shared across the layers a service calls.
- The service is the transaction boundary; `Savepoint` is capped at 5.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Selector Classes

*fflib query layer: per-object SOQL with security.*

### 🌱 Simple

*Beginner - plain language*

**fflib Selector classes** extend `fflib_SObjectSelector` and centralize all SOQL for one sObject — defining the **standard field list**, ordering, and security, with query methods like `selectById`.

### 📏 Limits

*Governor & platform limits*

- fflib_SObjectSelector; standard field list; FLS.
- Registered in factory; mockable; QueryLocator for batch.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Domain Classes

*fflib domain: record behavior on fflib_SObjectDomain.*

### 🌱 Simple

*Beginner - plain language*

**fflib Domain classes** extend `fflib_SObjectDomain`, wrapping records of one sObject and holding their **record-level rules** — validation, defaulting, and behavior — invoked from triggers via the base class's hooks.

### 📏 Limits

*Governor & platform limits*

- fflib_SObjectDomain + Constructor; hook overrides.
- Trigger one-liner; bulkified; factory + mockable.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Unit of Work

*fflib UoW: registered DML committed once, ordered.*

### 🌱 Simple

*Beginner - plain language*

**fflib Unit of Work** (`fflib_SObjectUnitOfWork`) implements the Unit of Work pattern: you `registerNew/registerDirty/registerRelationship` records and `commitWork()` once, committing all DML in dependency order with minimal statements.

### 📏 Limits

*Governor & platform limits*

- register* + commitWork once; dependency order.
- Savepoint-atomic, bundled DML; service-owned; mockable.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## IOC Container

*Inversion of Control container (force-di) for Apex.*

### 🌱 Simple

*Beginner - plain language*

An **IoC container** manages object creation and wiring centrally, injecting dependencies instead of classes constructing their own. In Salesforce, **force-di** is the IoC/DI container, often paired with fflib.

### 📏 Limits

*Governor & platform limits*

- force-di; CMDT/code bindings; runtime resolution.
- For cross-package extensibility; complements fflib factory.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Dependency Injection

*Provide dependencies externally; enables mocking.*

### 🌱 Simple

*Beginner - plain language*

**Dependency Injection** (DI) means a class **receives its collaborators from outside** rather than constructing them. In fflib, the Application factory injects services/selectors/domains/UoW — making code loosely coupled and testable.

### 📏 Limits

*Governor & platform limits*

- External provision via interfaces; DIP realized.
- fflib factory / force-di; mockable; inject at seams.

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
