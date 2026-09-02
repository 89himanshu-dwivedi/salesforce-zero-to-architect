[Home](../index.md) / [09 · LWC Basics](index.md) / **Template Directives**

# Template Directives

4 topics · Series 9: LWC Basics

**Topics on this page**

- [if:true / if:false](#if-true-if-false)
- [lwc:if / lwc:elseif / lwc:else](#lwc-if-lwc-elseif-lwc-else)
- [for:each](#for-each)
- [iterator](#iterator)

## if:true / if:false

*The legacy conditional directives for showing/hiding markup.*

### 🌱 Simple

*Beginner - plain language*

`if:true={prop}` and `if:false={prop}` are the **legacy** conditional directives — they render a `<template>` block based on a boolean. Newer code uses `lwc:if`.

### 📏 Limits

*Governor & platform limits*

- Deprecated in favour of `lwc:if`; both remove the element from the DOM rather than hiding it.
- The expression must be a property or getter, not an inline comparison.
- Removed elements lose their state and re-run lifecycle hooks when re-added.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## lwc:if / lwc:elseif / lwc:else

*The modern conditional directives with else-if support.*

### 🌱 Simple

*Beginner - plain language*

`lwc:if`, `lwc:elseif`, and `lwc:else` are the **modern** conditional directives (API 55+). They support clean if/else-if/else chains, replacing `if:true/if:false`.

### 📏 Limits

*Governor & platform limits*

- Requires API version 58.0 or later.
- `lwc:elseif` and `lwc:else` must immediately follow the previous branch.
- The condition must be a property or getter.
- Branches destroy and recreate DOM, resetting component state.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## for:each

*Iterating a list to render repeated markup, keyed for efficiency.*

### 🌱 Simple

*Beginner - plain language*

`for:each={items}` with `for:item="item"` iterates a list to render repeated markup. Each iteration needs a unique `key={item.id}` for efficient rendering.

### 📏 Limits

*Governor & platform limits*

- `key` is required and must be unique and stable - array index is not acceptable.
- Rendering thousands of rows is DOM-bound; the browser is the limit, not Apex.
- The iterable must be an array; Sets and Maps are not supported directly.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## iterator

*Iteration with first/last metadata for special-casing items.*

### 🌱 Simple

*Beginner - plain language*

The `iterator:it={items}` directive is like `for:each` but gives each item metadata — `it.value`, `it.index`, `it.first`, `it.last` — useful for styling the first/last items differently.

### 📏 Limits

*Governor & platform limits*

- Exposes `first`, `last`, `index` and `value` only.
- `key` is still required on the iterated element.
- Slightly more overhead than `for:each` - use only when you need the metadata.

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
