[Home](../index.md) / [16 · Enterprise Patterns](index.md) / **Design Principles**

# Design Principles

6 topics · Series 16: Enterprise Patterns

**Topics on this page**

- [DRY](#dry)
- [KISS](#kiss)
- [YAGNI](#yagni)
- [Separation of Concerns](#separation-of-concerns)
- [High Cohesion](#high-cohesion)
- [Loose Coupling](#loose-coupling)

## DRY

*Don't Repeat Yourself — one source of truth per knowledge.*

### 🌱 Simple

*Beginner - plain language*

**DRY (Don't Repeat Yourself)**: every piece of knowledge should have a **single, authoritative representation**. Duplicated logic means a change must be made in many places — and you'll miss one.

### 📏 Limits

*Governor & platform limits*

- Over-abstracting to avoid repetition adds CPU on hot paths.
- Shared utilities become change-coupling points across teams.
- Duplication across managed package boundaries is sometimes unavoidable.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## KISS

*Keep It Simple — favor the simplest solution that works.*

### 🌱 Simple

*Beginner - plain language*

**KISS (Keep It Simple, Stupid)**: prefer the **simplest design that solves the problem**. Complexity should be justified by real need, not added for cleverness or hypothetical future cases.

### 📏 Limits

*Governor & platform limits*

- Simple designs still have to respect governor limits - simplicity is not an exemption.
- Naive single-record logic fails on the 200-record trigger chunk.
- The simplest implementation is often the non-bulkified one - resist it.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## YAGNI

*You Aren't Gonna Need It — build for today's requirements.*

### 🌱 Simple

*Beginner - plain language*

**YAGNI (You Aren't Gonna Need It)**: don't build features, abstractions, or configurability **until they're actually needed**. Speculative work usually goes unused and adds maintenance burden.

### 📏 Limits

*Governor & platform limits*

- Build for known requirements; defer abstraction.
- Extend where change is real; stay refactor-ready.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Separation of Concerns

*Divide a system into distinct, focused responsibilities.*

### 🌱 Simple

*Beginner - plain language*

**Separation of Concerns (SoC)**: divide software so each part addresses a **distinct concern** — presentation, business logic, data access — with minimal overlap. Each layer focuses on one aspect.

### 📏 Limits

*Governor & platform limits*

- Presentation/controller/service/domain/data layers.
- Enforced boundaries; consistent cross-cutting.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## High Cohesion

*Elements of a module belong together and serve one purpose.*

### 🌱 Simple

*Beginner - plain language*

**High cohesion** means the parts of a class/module are **strongly related** and work toward a single, well-defined purpose. A cohesive class is focused; a low-cohesion class is a grab-bag of unrelated methods.

### 📏 Limits

*Governor & platform limits*

- Cohesive classes tend to be many and small, adding to the 6 MB Apex limit.
- Each layer boundary costs CPU.
- Cohesion does not remove the need for explicit CRUD/FLS enforcement.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Loose Coupling

*Minimize dependencies between modules.*

### 🌱 Simple

*Beginner - plain language*

**Loose coupling** means modules depend on each other as **little as possible**, interacting through stable interfaces rather than internal details. Changes in one module don't ripple into others.

### 📏 Limits

*Governor & platform limits*

- Event-based decoupling is bounded by 1 MB messages and 72-hour retention.
- Interface indirection adds CPU and makes stack traces longer.
- Loose coupling shifts complexity to monitoring - you need observability across the seam.

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
