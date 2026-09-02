[Home](../index.md) / [16 · Enterprise Patterns](index.md) / **SOLID Principles**

# SOLID Principles

5 topics · Series 16: Enterprise Patterns

**Topics on this page**

- [Single Responsibility](#single-responsibility)
- [Open Closed](#open-closed)
- [Liskov Substitution](#liskov-substitution)
- [Interface Segregation](#interface-segregation)
- [Dependency Inversion](#dependency-inversion)

## Single Responsibility

*A class should have one, and only one, reason to change.*

### 🌱 Simple

*Beginner - plain language*

**Single Responsibility Principle (SRP)**: a class should do **one thing** and have a single reason to change. If a class handles validation *and* persistence *and* email, three different concerns force it to change for three different reasons.

### 📏 Limits

*Governor & platform limits*

- More, smaller classes means more code against the 6 MB org Apex character limit.
- Each additional call hop adds CPU against the 10s budget.
- Over-decomposition makes governor-limit attribution in debug logs harder.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Open Closed

*Open for extension, closed for modification.*

### 🌱 Simple

*Beginner - plain language*

**Open/Closed Principle (OCP)**: you should be able to **add new behavior without modifying existing, tested code** — extend via new classes/implementations rather than editing the original.

### 📏 Limits

*Governor & platform limits*

- Interfaces + strategy/factory; metadata-driven dispatch.
- Type.forName for dynamic instantiation.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Liskov Substitution

*Subtypes must be substitutable for their base type.*

### 🌱 Simple

*Beginner - plain language*

**Liskov Substitution Principle (LSP)**: anywhere you use a base type, you should be able to use any subclass **without breaking behavior**. A subclass must honor the contract of its parent.

### 📏 Limits

*Governor & platform limits*

- Apex has single inheritance only, which constrains substitution hierarchies.
- Violations surface as runtime `TypeException`, not compile errors.
- Managed package classes cannot be extended unless declared `global virtual`.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Interface Segregation

*Many small interfaces beat one fat interface.*

### 🌱 Simple

*Beginner - plain language*

**Interface Segregation Principle (ISP)**: clients shouldn't be forced to depend on methods they don't use. Prefer several **small, focused interfaces** over one large "fat" one.

### 📏 Limits

*Governor & platform limits*

- Each interface is a separate class file counting toward the Apex size limit.
- Adding a method to a `global` interface in a package breaks every implementer.
- Interfaces cannot hold fields or default implementations.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Dependency Inversion

*Depend on abstractions, not concretions.*

### 🌱 Simple

*Beginner - plain language*

**Dependency Inversion Principle (DIP)**: high-level modules shouldn't depend on low-level modules — both should depend on **abstractions (interfaces)**. Don't hard-code concrete classes; inject them.

### 📏 Limits

*Governor & platform limits*

- Apex has no built-in DI container - `Type.forName()` or a registry is the mechanism.
- Resolution failures surface only at runtime.
- `Test.createStub()` requires an interface and works only in test context.

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
