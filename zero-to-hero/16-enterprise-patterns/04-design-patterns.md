[Home](../index.md) / [16 · Enterprise Patterns](index.md) / **Design Patterns**

# Design Patterns

17 topics · Series 16: Enterprise Patterns

**Topics on this page**

- [Singleton](#singleton)
- [Factory](#factory)
- [Abstract Factory](#abstract-factory)
- [Builder](#builder)
- [Prototype](#prototype)
- [Adapter](#adapter)
- [Facade](#facade)
- [Decorator](#decorator)
- [Proxy](#proxy)
- [Bridge](#bridge)
- [Strategy](#strategy)
- [Observer](#observer)
- [Command](#command)
- [Chain of Responsibility](#chain-of-responsibility)
- [Mediator](#mediator)
- [Template Method](#template-method)
- [State Pattern](#state-pattern)

## Singleton

*Ensure a single shared instance per context.*

### 🌱 Simple

*Beginner - plain language*

**Singleton** ensures only **one instance** of a class exists and provides a global access point. In Apex, a common use is a per-**transaction** singleton to cache shared state.

### 📏 Limits

*Governor & platform limits*

- Transaction-scoped static; single-threaded.
- Reset hook for tests; Platform Cache for cross-request.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Factory

*Create objects without exposing instantiation logic.*

### 🌱 Simple

*Beginner - plain language*

**Factory Method** centralizes object creation: callers ask a factory for an instance of an interface, and the factory decides **which concrete class** to build — hiding the `new` logic.

### 📏 Limits

*Governor & platform limits*

- Interface + metadata + Type.forName dispatch.
- Mock override; escalate to Abstract Factory for families.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Abstract Factory

*Create families of related objects via one interface.*

### 🌱 Simple

*Beginner - plain language*

**Abstract Factory** provides an interface to create **families of related objects** without specifying their concrete classes — e.g., a UI factory that produces a matching button, dialog, and menu for a theme.

### 📏 Limits

*Governor & platform limits*

- Creates compatible families; one factory per variant.
- Metadata-selected; escalate from Factory Method.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Builder

*Construct complex objects step by step.*

### 🌱 Simple

*Beginner - plain language*

**Builder** separates the construction of a complex object from its representation, assembling it **step by step** via a fluent API — avoiding telescoping constructors with many parameters.

### 📏 Limits

*Governor & platform limits*

- Fluent `this` chaining; `build()` validates.
- Great for test data & dynamic SOQL builders.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Prototype

*Create new objects by cloning an existing instance.*

### 🌱 Simple

*Beginner - plain language*

**Prototype** creates new objects by **cloning an existing one** rather than constructing from scratch — useful when creation is expensive or you want a copy with the same configuration.

### 📏 Limits

*Governor & platform limits*

- sObject clone flags; shallow object `.clone()`.
- JSON round-trip for deep copy; mind references.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Adapter

*Make an incompatible interface usable by a client.*

### 🌱 Simple

*Beginner - plain language*

**Adapter** wraps a class with an incompatible interface so it works with the interface your code expects — like a power plug adapter. It translates calls between the two.

### 📏 Limits

*Governor & platform limits*

- Implements your interface; delegates to foreign API.
- Object adapter (composition); anti-corruption layer.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Facade

*A simple interface over a complex subsystem.*

### 🌱 Simple

*Beginner - plain language*

**Facade** provides a single, **simplified interface** to a complex subsystem of many classes — callers use the easy facade instead of orchestrating the internals themselves.

### 📏 Limits

*Governor & platform limits*

- Coarse use-case methods; owns transaction boundary.
- Reused by all entry points; simplifies, not restricts.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Decorator

*Add behavior to an object dynamically by wrapping it.*

### 🌱 Simple

*Beginner - plain language*

**Decorator** wraps an object to **add behavior dynamically** without changing its class — both the decorator and the wrapped object share the same interface, so wrapping is transparent to callers.

### 📏 Limits

*Governor & platform limits*

- Each wrapper multiplies heap on large collections.
- Nested virtual dispatch adds CPU inside loops.
- Deep decoration produces long stack traces that are harder to read in production logs.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Proxy

*A stand-in controlling access to another object.*

### 🌱 Simple

*Beginner - plain language*

**Proxy** provides a **placeholder/stand-in** for another object to control access to it — adding lazy loading, access control, caching, or remote access while sharing the real object's interface.

### 📏 Limits

*Governor & platform limits*

- Same interface; controls access (may not delegate).
- Virtual/protection/caching/remote; wired via factory.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Bridge

*Decouple an abstraction from its implementation.*

### 🌱 Simple

*Beginner - plain language*

**Bridge** separates an **abstraction** from its **implementation** so the two can vary independently — instead of binding them in one class hierarchy, you compose them via a reference.

### 📏 Limits

*Governor & platform limits*

- Abstraction holds implementor interface; m+n classes.
- Two varying dimensions; composed at runtime.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Strategy

*Select an algorithm at runtime via a common interface.*

### 🌱 Simple

*Beginner - plain language*

**Strategy** defines a family of interchangeable algorithms behind a common interface and lets you **pick one at runtime** — swapping behavior without changing the caller.

### 📏 Limits

*Governor & platform limits*

- Interchangeable algorithms; metadata-driven selection.
- Replaces conditionals; injected/mockable.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Observer

*Notify many subscribers when a subject changes.*

### 🌱 Simple

*Beginner - plain language*

**Observer** lets a subject **notify many subscribers** automatically when its state changes — subscribers register and react, without the subject knowing their concrete types.

### 📏 Limits

*Governor & platform limits*

- Platform Events/CDC; async separate transactions.
- At-least-once; replay IDs; design idempotent.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Command

*Encapsulate a request as an object.*

### 🌱 Simple

*Beginner - plain language*

**Command** wraps a request/action as an **object** — with all its parameters — so you can queue it, log it, undo it, or pass it around. The invoker triggers it without knowing the concrete operation.

### 📏 Limits

*Governor & platform limits*

- Commands serialised into Queueable state fail on non-serialisable member types.
- Command collections count toward the 6 MB heap.
- Undo requires `Savepoint`, limited to 5 per transaction.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Chain of Responsibility

*Pass a request along a chain until one handles it.*

### 🌱 Simple

*Beginner - plain language*

**Chain of Responsibility** passes a request along a **chain of handlers** until one handles it. Each handler decides to process the request or pass it to the next — decoupling sender from receiver.

### 📏 Limits

*Governor & platform limits*

- Linked handlers; handle or forward; may short-circuit.
- Metadata-ordered; terminal default handler.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Mediator

*Centralize complex many-to-many interactions.*

### 🌱 Simple

*Beginner - plain language*

**Mediator** centralizes communication between many objects so they interact through a **single mediator** instead of referencing each other directly — reducing tangled many-to-many dependencies.

### 📏 Limits

*Governor & platform limits*

- The mediator becomes a CPU hotspot as participants grow.
- In LWC, LMS is the platform mediator and offers no delivery guarantee.
- Centralising communication can hide coupling rather than remove it.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Template Method

*Define an algorithm skeleton; subclasses fill steps.*

### 🌱 Simple

*Beginner - plain language*

**Template Method** defines the **skeleton of an algorithm** in a base class, deferring specific steps to subclasses. The overall flow is fixed; the variable steps are overridden.

### 📏 Limits

*Governor & platform limits*

- Requires `virtual` or `abstract` base classes; Apex allows single inheritance only.
- Base classes in managed packages must be `global virtual` to be extended.
- Each override level adds CPU on hot paths.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## State Pattern

*An object alters its behavior as its state changes.*

### 🌱 Simple

*Beginner - plain language*

**State pattern** lets an object **change its behavior when its internal state changes** — appearing to change class. Each state is an object encapsulating the behavior valid in that state.

### 📏 Limits

*Governor & platform limits*

- State objects + encapsulated transitions.
- Object transitions itself (vs external Strategy choice).

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
