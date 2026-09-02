[Home](../index.md) / [16 · Enterprise Patterns](index.md) / **Architecture Concepts**

# Architecture Concepts

6 topics · Series 16: Enterprise Patterns

**Topics on this page**

- [Monolithic](#monolithic)
- [Modular](#modular)
- [SOA](#soa)
- [Event Driven](#event-driven)
- [Microservices](#microservices)
- [API First Design](#api-first-design)

## Monolithic

*Single deployable unit; all logic together.*

### 🌱 Simple

*Beginner - plain language*

**Monolithic architecture** packages all functionality into a **single deployable unit** sharing one codebase, data model, and runtime. Simple to start, but everything is coupled and deploys together.

### 📏 Limits

*Governor & platform limits*

- Single unit; shared limits/metadata; simple, consistent.
- Couples at scale; mitigate via modular packaging.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Modular

*Decompose into cohesive, loosely-coupled modules.*

### 🌱 Simple

*Beginner - plain language*

**Modular architecture** decomposes a system into **cohesive, independent modules** with clear interfaces. Each module owns a concern and can be developed, tested, and deployed with minimal impact on others.

### 📏 Limits

*Governor & platform limits*

- Cohesive modules + loose coupling; unlocked packages.
- Acyclic deps; interfaces/events; middle ground.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## SOA

*Service-Oriented Architecture: reusable network services.*

### 🌱 Simple

*Beginner - plain language*

**SOA** (Service-Oriented Architecture) structures a system as **reusable, network-accessible services** with well-defined contracts, often coordinated via an integration layer or ESB. Services are coarse-grained and shared across applications.

### 📏 Limits

*Governor & platform limits*

- Coarse reusable services + contracts; ESB/MuleSoft.
- Salesforce as participant; vs decentralized microservices.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Event Driven

*Components react to events asynchronously; decoupled.*

### 🌱 Simple

*Beginner - plain language*

**Event-Driven Architecture** has components communicate by **producing and consuming events** rather than calling each other directly. Producers don't know consumers — decoupled, asynchronous, and scalable.

### 📏 Limits

*Governor & platform limits*

- Platform Events/CDC/Pub-Sub API; pub/sub decoupling.
- At-least-once → idempotent; eventual consistency.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Microservices

*Small, independently deployable, decentralized services.*

### 🌱 Simple

*Beginner - plain language*

**Microservices** decompose a system into **small, independently deployable services**, each owning its data and business capability, communicating over the network (REST/events). Decentralized, independently scalable, polyglot.

### 📏 Limits

*Governor & platform limits*

- Independent deploy/scale; own data; decentralized.
- Offload from Salesforce SoR; distributed costs; only when justified.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## API First Design

*Design the API contract before implementation.*

### 🌱 Simple

*Beginner - plain language*

**API-First design** means defining the **API contract (OpenAPI/schema) before building** the implementation. The contract becomes the agreement that frontend, backend, and integrators build against in parallel.

### 📏 Limits

*Governor & platform limits*

- OpenAPI contract first; parallel dev via mocks.
- External Services/Apex REST/MuleSoft; versioned, governed.

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
