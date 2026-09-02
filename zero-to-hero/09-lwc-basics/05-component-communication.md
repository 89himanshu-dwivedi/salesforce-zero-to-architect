[Home](../index.md) / [09 · LWC Basics](index.md) / **Component Communication**

# Component Communication

2 topics · Series 9: LWC Basics

**Topics on this page**

- [Parent to Child (@api)](#parent-to-child-api)
- [Child to Parent (CustomEvent)](#child-to-parent-customevent)

## Parent to Child (@api)

*Passing data and invoking methods downward via the public @api.*

### 🌱 Simple

*Beginner - plain language*

**Parent → child** communication uses `@api`: the child exposes public properties/methods with `@api`, and the parent sets properties via attributes or calls methods via `querySelector`.

### 📏 Limits

*Governor & platform limits*

- `@api` properties are read-only in the child; mutating them is unsupported.
- Objects and arrays are passed by reference - mutating them bypasses reactivity.
- `@api` methods can only be called after the child has rendered.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Child to Parent (CustomEvent)

*Notifying the parent upward by dispatching custom events.*

### 🌱 Simple

*Beginner - plain language*

**Child → parent** communication uses **custom events**: the child dispatches a `CustomEvent` with a payload, and the parent listens via `on<eventname>` and updates its own state.

### 📏 Limits

*Governor & platform limits*

- Event names must be lowercase without hyphens.
- Non-composed events stop at the shadow boundary - siblings cannot hear them.
- No sibling-to-sibling communication without a common parent or LMS.

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
