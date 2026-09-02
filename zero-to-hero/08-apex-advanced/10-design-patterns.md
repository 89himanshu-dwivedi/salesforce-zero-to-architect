[Home](../index.md) / [08 · Apex Advanced](index.md) / **Design Patterns**

# Design Patterns

10 topics · Series 8: Apex Advanced

**Topics on this page**

- [Singleton](#singleton)
- [Factory](#factory)
- [Strategy](#strategy)
- [Observer](#observer)
- [Command](#command)
- [Builder](#builder)
- [Facade](#facade)
- [Adapter](#adapter)
- [Decorator](#decorator)
- [Dependency Injection](#dependency-injection)

## Singleton

*One shared instance per transaction to cache state and avoid rework.*

### 🌱 Simple

*Beginner - plain language*

The **Singleton** pattern ensures a class has **one instance**, accessed via a static accessor. In Apex it's used to cache config/state for the **duration of a transaction** so you don't recompute or re-query.

### 📏 Limits

*Governor & platform limits*

- Statics are transaction-scoped - a Singleton does not persist across transactions or batch chunks.
- Singleton state held across a 200-record chunk can leak data between records.
- Singletons are hard to mock; expose an interface and a settable instance for testing.
- Instance state counts toward heap for the whole transaction.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Factory

*Centralizing object creation behind an interface for flexibility.*

### 🌱 Simple

*Beginner - plain language*

The **Factory** pattern centralizes **object creation**: instead of `new`-ing concrete classes everywhere, a factory returns the right implementation (often by type) behind an interface.

### 📏 Limits

*Governor & platform limits*

- Dynamic instantiation with `Type.forName()` fails at runtime, not compile time.
- Class names stored in Custom Metadata break silently if a class is renamed.
- Each factory hop adds CPU - avoid inside tight loops.
- The created type must have a public no-arg constructor for `newInstance()`.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Strategy

*Selecting interchangeable algorithms behind a common interface.*

### 🌱 Simple

*Beginner - plain language*

The **Strategy** pattern defines a family of **interchangeable algorithms** behind one interface, letting you pick the behavior at runtime — e.g., different discount or routing rules.

### 📏 Limits

*Governor & platform limits*

- Requires interfaces, so every strategy is an additional class against the 6 MB Apex limit.
- Runtime selection means the compiler cannot verify all paths are covered by tests.
- Virtual dispatch adds CPU cost in high-volume loops.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Observer

*Decoupling producers from reactive consumers via subscription.*

### 🌱 Simple

*Beginner - plain language*

The **Observer** pattern lets objects **subscribe** to a subject and be notified of changes — decoupling the producer from its reactors. On Salesforce, **Platform Events** are the native realization.

### 📏 Limits

*Governor & platform limits*

- Apex has no built-in event bus for in-transaction observers - you build the registry.
- Platform Events are the cross-transaction equivalent but are asynchronous and at-least-once.
- Observer chains share the transaction's SOQL, DML and CPU limits.
- Ordering of observers must be explicit or it is undefined.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Command

*Encapsulating an action as an object to queue, log, or undo.*

### 🌱 Simple

*Beginner - plain language*

The **Command** pattern wraps an **action** (and its data) as an object with an `execute()` method — so actions can be queued, logged, parameterized, or retried uniformly.

### 📏 Limits

*Governor & platform limits*

- Serialising commands for Queueable state fails on non-serialisable member types.
- Command objects held in collections count toward heap.
- Undo semantics must be built manually - Apex offers only `Savepoint` (max 5).

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Builder

*Fluently constructing complex objects step by step.*

### 🌱 Simple

*Beginner - plain language*

The **Builder** pattern constructs a complex object **step by step** via a fluent API (chained methods), avoiding huge constructors. Common in Apex for building test data or complex requests.

### 📏 Limits

*Governor & platform limits*

- Fluent chains create intermediate objects that consume heap.
- Builders used for test data still consume the 150 DML statement / 10,000 row limits.
- Long method chains make debug-log attribution harder.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Facade

*A simple unified interface hiding a complex subsystem.*

### 🌱 Simple

*Beginner - plain language*

The **Facade** pattern provides a **simple, unified interface** over a complex subsystem — callers use the facade instead of wrangling many lower-level classes.

### 📏 Limits

*Governor & platform limits*

- Each facade layer adds CPU on hot paths - measure before adding another.
- A facade does not enforce security; CRUD/FLS/sharing remain explicit.
- Global facades in managed packages become permanent API commitments.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Adapter

*Converting one interface into another callers expect.*

### 🌱 Simple

*Beginner - plain language*

The **Adapter** pattern **converts** one class's interface into another that callers expect — letting incompatible classes work together (a "wrapper").

### 📏 Limits

*Governor & platform limits*

- Adapters that transform large payloads consume heap and CPU during serialisation.
- Callout body limits (6 MB / 12 MB) apply to the adapted payload, not the original.
- Adapting to a vendor schema couples you to their versioning - isolate it in one class.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Decorator

*Adding behavior to an object dynamically without subclassing.*

### 🌱 Simple

*Beginner - plain language*

The **Decorator** pattern **wraps** an object to add behavior dynamically while keeping the same interface — layering features without modifying the original or subclassing.

### 📏 Limits

*Governor & platform limits*

- Each decorator wraps another object, multiplying heap on large collections.
- Nested virtual dispatch adds measurable CPU inside loops.
- Deep decoration makes stack traces long and harder to read in production logs.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Dependency Injection

*Supplying dependencies from outside to decouple and enable mocking.*

### 🌱 Simple

*Beginner - plain language*

**Dependency Injection (DI)** means a class receives its dependencies from **outside** (via constructor/setter) rather than creating them itself — decoupling classes and making them easy to test with mocks.

### 📏 Limits

*Governor & platform limits*

- Apex has no built-in DI container - `Type.forName()` or a registry class is the mechanism.
- Runtime resolution failures surface only at execution time.
- `Test.createStub()` requires an interface and works only in test context.
- Each indirection layer adds CPU.

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
