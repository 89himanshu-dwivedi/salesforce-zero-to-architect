[Home](../index.md) / [07 · Apex Fundamentals](index.md) / **OOP Concepts**

# OOP Concepts

9 topics · Series 7: Apex Fundamentals

**Topics on this page**

- [Properties](#properties)
- [Methods](#methods)
- [Constructors](#constructors)
- [Public / Private / Global / Protected](#public-private-global-protected)
- [Inheritance (extends)](#inheritance-extends)
- [Interface (implements)](#interface-implements)
- [Polymorphism](#polymorphism)
- [Encapsulation](#encapsulation)
- [Abstraction](#abstraction)

## Properties

*Fields with implicit getter/setter logic for controlled access.*

### 🌱 Simple

*Beginner - plain language*

A **property** looks like a field but has built-in get/set logic: `public Integer count { get; set; }`. You can add logic in `get`/`set` to validate or compute values.

### 📏 Limits

*Governor & platform limits*

- Auto-implemented properties cannot have a body and cannot be initialised inline in all contexts.
- Property getters run on every access - avoid queries or heavy work inside them.
- Properties used in Visualforce contribute to the 170 KB view state unless marked `transient`.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Methods

*Named, reusable units of behavior with parameters and return types.*

### 🌱 Simple

*Beginner - plain language*

A **method** is a reusable block of code: `public Decimal addTax(Decimal amt) { return amt * 1.1; }`. It takes parameters, does work, and may return a value.

### 📏 Limits

*Governor & platform limits*

- Max 32 parameters per method; recursion is bounded by heap and CPU, not a fixed depth.
- Apex code size limit is 6 MB of characters per org (test classes excluded).
- Methods on managed package classes cannot be overridden unless declared `virtual`/`global`.
- Every method call adds measurable CPU on hot paths - avoid deep layering in loops.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Constructors

*Special methods that initialize new objects.*

### 🌱 Simple

*Beginner - plain language*

A **constructor** runs when you create an object with `new`: `public Cart(Id ownerId) { this.ownerId = ownerId; }`. It sets up initial state.

### 📏 Limits

*Governor & platform limits*

- A class with no explicit constructor gets a default no-arg one; defining any constructor removes it.
- Constructors cannot be `@future`, `@AuraEnabled` or return values.
- Queueable and Batch classes must have a no-arg constructor for some serialisation paths.
- Heavy work in a constructor runs on every instantiation - keep them cheap.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Public / Private / Global / Protected

*Access modifiers controlling visibility across classes and namespaces.*

### 🌱 Simple

*Beginner - plain language*

Access modifiers set visibility: **private** (same class), **protected** (same class + subclasses), **public** (same namespace/app), **global** (all namespaces, including subscribers of a managed package).

### 📏 Limits

*Governor & platform limits*

- `global` members in a managed package can never be removed - it is a permanent API commitment.
- `@RestResource` classes and their methods must be `global`.
- `private` members are still visible to test classes in the same file via `@TestVisible`.
- Access modifiers do not enforce data security - CRUD/FLS/sharing are separate concerns.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Inheritance (extends)

*Deriving a class from a base class to reuse and specialize behavior.*

### 🌱 Simple

*Beginner - plain language*

**Inheritance** lets a class reuse another's members: `class SavingsAccount extends BankAccount {...}`. The subclass inherits and can override behavior.

### 📏 Limits

*Governor & platform limits*

- Single inheritance only - a class extends at most one class.
- The parent must be declared `virtual` or `abstract`; methods must be `virtual` to be overridden.
- You cannot extend a class in a managed package unless it is `global virtual`.
- Deep hierarchies add CPU cost on hot paths and complicate stub-based testing.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Interface (implements)

*A contract of method signatures enabling polymorphism and decoupling.*

### 🌱 Simple

*Beginner - plain language*

An **interface** declares method signatures with no bodies: `interface Payable { void pay(); }`. A class `implements` it and provides the implementations — enabling interchangeable types.

### 📏 Limits

*Governor & platform limits*

- A class may implement multiple interfaces; interfaces cannot contain implementations or fields.
- Interface methods are implicitly public - you cannot narrow visibility.
- Adding a method to a `global` interface in a managed package breaks all implementers.
- Interfaces are what make `Test.createStub()` mocking possible - design for them.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Polymorphism

*One interface, many implementations — runtime behavior selection.*

### 🌱 Simple

*Beginner - plain language*

**Polymorphism** lets different classes respond to the same call differently. A `List<Shape>` can hold Circles and Squares; calling `area()` runs each type's own version.

### 📏 Limits

*Governor & platform limits*

- Resolution is at runtime; the compiler cannot warn about an unimplemented override path.
- Casting to the wrong subtype throws `TypeException` at runtime.
- `instanceof` returns false for null, which silently changes branch behaviour.
- Virtual dispatch adds CPU overhead - measurable inside large loops.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Encapsulation

*Hiding internal state behind a controlled public interface.*

### 🌱 Simple

*Beginner - plain language*

**Encapsulation** means keeping data **private** and exposing it only through methods/properties. Callers use the public interface, not the internals.

### 📏 Limits

*Governor & platform limits*

- Access modifiers are a design tool, not a security boundary - Apex runs in system mode by default.
- CRUD, FLS and sharing must be enforced explicitly with `WITH USER_MODE` or `stripInaccessible`.
- `@TestVisible` deliberately breaks encapsulation for testing - use sparingly.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Abstraction

*Exposing essential behavior while hiding implementation detail.*

### 🌱 Simple

*Beginner - plain language*

**Abstraction** means modeling *what* something does, not *how*. An `abstract` class or interface defines behavior; concrete classes implement the details.

### 📏 Limits

*Governor & platform limits*

- Abstract classes cannot be instantiated and must be extended; they may hold state, interfaces cannot.
- A class can extend one abstract class but implement many interfaces.
- Abstract methods must be implemented by every concrete subclass or the code will not compile.
- Over-abstraction adds CPU cost and makes governor-limit attribution harder in logs.

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
