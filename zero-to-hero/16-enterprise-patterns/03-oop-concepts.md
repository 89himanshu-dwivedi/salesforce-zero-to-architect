[Home](../index.md) / [16 · Enterprise Patterns](index.md) / **OOP Concepts**

# OOP Concepts

8 topics · Series 16: Enterprise Patterns

**Topics on this page**

- [Encapsulation](#encapsulation)
- [Inheritance](#inheritance)
- [Polymorphism](#polymorphism)
- [Abstraction](#abstraction)
- [Composition](#composition)
- [Aggregation](#aggregation)
- [Association](#association)
- [Dependency](#dependency)

## Encapsulation

*Bundle data with behavior; hide internal state.*

### 🌱 Simple

*Beginner - plain language*

**Encapsulation** bundles data and the methods that operate on it into one unit (a class) and **hides internal state** behind access modifiers — exposing a controlled public interface.

### 📏 Limits

*Governor & platform limits*

- private/protected/public/global; properties.
- `@TestVisible`; global = permanent contract.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Inheritance

*A subclass reuses and extends a base class.*

### 🌱 Simple

*Beginner - plain language*

**Inheritance** lets a class (subclass) **reuse and extend** another (superclass) via an "is-a" relationship — inheriting fields and methods and optionally overriding them.

### 📏 Limits

*Governor & platform limits*

- Single inheritance; virtual/abstract/override/super.
- Multiple interfaces; prefer composition for reuse.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Polymorphism

*One interface, many implementations resolved at runtime.*

### 🌱 Simple

*Beginner - plain language*

**Polymorphism** lets different classes be used through a **common interface or base type**, with the right implementation chosen at runtime. The same call does different things depending on the actual object.

### 📏 Limits

*Governor & platform limits*

- Overriding (runtime) vs overloading (compile-time).
- Type.forName dynamic dispatch; metadata-driven.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Abstraction

*Expose essential behavior; hide implementation detail.*

### 🌱 Simple

*Beginner - plain language*

**Abstraction** models the **essential features** of something while hiding complex details — you interact with *what* it does, not *how*. Interfaces and abstract classes define these contracts.

### 📏 Limits

*Governor & platform limits*

- Interface (contract) vs abstract class (partial impl).
- Clean contracts at real variation; enables DIP/OCP.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Composition

*A whole owns its parts; parts don't outlive the whole.*

### 🌱 Simple

*Beginner - plain language*

**Composition** is a strong "has-a" relationship where the **whole owns its parts** and the parts can't exist independently — e.g., an Order owns its OrderLines; delete the order and the lines go too.

### 📏 Limits

*Governor & platform limits*

- Exclusive ownership/lifecycle; master-detail in data.
- Delegation in code; powers strategy/decorator.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Aggregation

*A whole references parts that exist independently.*

### 🌱 Simple

*Beginner - plain language*

**Aggregation** is a weaker "has-a" relationship: the whole **references parts that can exist on their own**. A Department has Employees, but employees still exist if the department is deleted.

### 📏 Limits

*Governor & platform limits*

- Independent lifecycle; lookup relationship.
- Own sharing/ownership; reparentable; more per object.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Association

*A general relationship where objects know each other.*

### 🌱 Simple

*Beginner - plain language*

**Association** is the most general relationship — two objects are **related/aware of each other** without ownership. It can be one-to-one, one-to-many, or many-to-many, and is often bidirectional.

### 📏 Limits

*Governor & platform limits*

- Umbrella: association→aggregation→composition.
- Junction object for many-to-many; can carry attributes.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Dependency

*One class relies on another to function (uses-a).*

### 🌱 Simple

*Beginner - plain language*

A **dependency** (uses-a) is the weakest link: one class **uses another temporarily** — as a method parameter, local variable, or return type — without holding it long-term. Change the used class's contract and the user may break.

### 📏 Limits

*Governor & platform limits*

- Uses-a (weakest); point at abstractions (DIP).
- Inject; keep graph acyclic/layered; explicit deps.

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
