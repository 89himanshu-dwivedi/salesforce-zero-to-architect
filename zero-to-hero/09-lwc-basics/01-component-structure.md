[Home](../index.md) / [09 · LWC Basics](index.md) / **Component Structure**

# Component Structure

6 topics · Series 9: LWC Basics

**Topics on this page**

- [HTML Template](#html-template)
- [JavaScript Controller](#javascript-controller)
- [CSS Styling](#css-styling)
- [XML Metadata](#xml-metadata)
- [Folder Structure](#folder-structure)
- [Naming Conventions](#naming-conventions)

## HTML Template

*The declarative markup file that defines a component's rendered UI.*

### 🌱 Simple

*Beginner - plain language*

Every LWC has an **HTML template** (`myCmp.html`) wrapped in a single `<template>` root. It declares the UI declaratively and binds to JS via `{property}` syntax.

### 📏 Limits

*Governor & platform limits*

- Expressions must be a property or getter - no method calls, operators or arbitrary JS.
- One root `<template>` per file; nested templates only for directives.
- Shadow DOM scopes the markup - `document.querySelector` cannot reach inside.
- No inline event handler attributes; use `on*` bindings only.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## JavaScript Controller

*The ES module class that holds component state and behavior.*

### 🌱 Simple

*Beginner - plain language*

The **JavaScript controller** (`myCmp.js`) is an ES module exporting a class that extends `LightningElement`. It holds the component's properties, getters, event handlers, and lifecycle logic.

### 📏 Limits

*Governor & platform limits*

- ES modules only; the class must be the default export and extend `LightningElement`.
- No `eval` and no inline scripts - blocked by CSP.
- Third-party libraries must load from a Static Resource under Lightning Web Security.
- Reactivity tracks assignment, not mutation.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## CSS Styling

*Scoped component styles isolated by Shadow DOM.*

### 🌱 Simple

*Beginner - plain language*

Each LWC can have a **CSS file** (`myCmp.css`) whose styles are **scoped** to that component via Shadow DOM — they don't leak out or bleed in.

### 📏 Limits

*Governor & platform limits*

- Styles are scoped to the shadow root - they cannot leak in or out.
- Child components cannot be styled from the parent except via CSS custom properties or `::part`.
- No CSS preprocessors; the file must be plain CSS.
- SLDS design tokens are the supported theming mechanism.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## XML Metadata

*The config file controlling where and how a component can be used.*

### 🌱 Simple

*Beginner - plain language*

The **meta XML** file (`myCmp.js-meta.xml`) configures the component: API version, whether it's **exposed**, which targets (App/Record/Home pages, Experience Cloud), and any design properties.

### 📏 Limits

*Governor & platform limits*

- `isExposed` must be true or the component is invisible in App Builder.
- Each target must be declared explicitly - a missing target means the component cannot be placed.
- API version here controls which platform features the component can use.
- Design attributes are only honoured for supported targets.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Folder Structure

*The required file bundle and naming convention for a component.*

### 🌱 Simple

*Beginner - plain language*

An LWC lives in a **folder** matching the component name, containing the `.html`, `.js`, and `.js-meta.xml` (CSS and SVG optional). All under `force-app/.../lwc/`.

### 📏 Limits

*Governor & platform limits*

- Folder, file and class names must align; the folder name cannot contain a hyphen.
- Folder names must start with a lowercase letter and be camelCase.
- Only files matching the folder name are treated as the component entry point.
- Nested component folders are not supported.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Naming Conventions

*camelCase JS, kebab-case markup, and the c- namespace.*

### 🌱 Simple

*Beginner - plain language*

LWC uses **camelCase** for folders/files/JS properties and **kebab-case** with a `c-` prefix for component tags in markup (`myCmp` → `<c-my-cmp>`). Properties also convert camelCase to kebab-case as HTML attributes.

### 📏 Limits

*Governor & platform limits*

- camelCase folder maps to kebab-case in markup (`myCard` → `<c-my-card>`).
- Reserved namespaces (`lightning`, `force`, `lwc`) cannot be used.
- Event names must be lowercase with no hyphens or camelCase.
- Renaming a component breaks every reference including FlexiPages.

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
