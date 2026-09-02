[Home](../index.md) / [09 · LWC Basics](index.md) / **Data Binding**

# Data Binding

3 topics · Series 9: LWC Basics

**Topics on this page**

- [One Way Binding](#one-way-binding)
- [Template Expressions](#template-expressions)
- [Property Binding](#property-binding)

## One Way Binding

*Data flows from JS to template; the DOM never writes back automatically.*

### 🌱 Simple

*Beginner - plain language*

LWC uses **one-way data binding**: the JS controller's data flows **into** the template via `{property}`, but the template doesn't write back to JS automatically. To update state, you handle events.

### 📏 Limits

*Governor & platform limits*

- Data flows parent to child only; children must never mutate an `@api` property.
- Mutating an object or array in place does not trigger a re-render - reassign it.
- Deep reactivity is not tracked for nested objects.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Template Expressions

*Property and getter references — the only logic allowed in markup.*

### 🌱 Simple

*Beginner - plain language*

LWC template "expressions" are limited to **property and getter references** in `{}` — e.g., `{user.name}` or `{isReady}`. No operators, method calls, or arbitrary JS.

### 📏 Limits

*Governor & platform limits*

- Only property and getter references are allowed - no operators, no function calls.
- Getters run on every render, so heavy work there is a performance problem.
- Optional chaining and nullish coalescing are not supported in templates.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Property Binding

*Passing data into child components and elements via attributes.*

### 🌱 Simple

*Beginner - plain language*

**Property binding** passes data into a child component or HTML element through attributes — e.g., `<c-child item={record}>` sets the child's `@api item` property.

### 📏 Limits

*Governor & platform limits*

- `@api` properties are the public contract and are read-only inside the component.
- Attribute names are kebab-case in markup and camelCase in JS.
- Boolean attributes are true when present - passing `false` as a string makes them truthy.

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
