[Home](../index.md) / [10 · LWC Advanced](index.md) / **Advanced UX**

# Advanced UX

5 topics · Series 10: LWC Advanced

**Topics on this page**

- [Reusable Components](#reusable-components)
- [Dynamic Forms Integration](#dynamic-forms-integration)
- [Dynamic Actions Integration](#dynamic-actions-integration)
- [Experience Cloud LWC](#experience-cloud-lwc)
- [Mobile Responsive LWC](#mobile-responsive-lwc)

## Reusable Components

*Designing composable, configurable LWC building blocks.*

### 🌱 Simple

*Beginner - plain language*

**Reusable components** are generic LWCs designed to be used in many contexts — configured via `@api` properties, composed via slots, and communicating via events — instead of one-off components.

### 📏 Limits

*Governor & platform limits*

- Every `@api` property is a permanent public contract - changing it breaks consumers.
- Reusable components cannot assume a specific container or record context.
- Over-generalisation adds configuration complexity and render cost.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Dynamic Forms Integration

*Using LWC alongside Dynamic Forms for flexible layouts.*

### 🌱 Simple

*Beginner - plain language*

**Dynamic Forms** lets admins place individual fields (and custom LWCs) on a record page as components, instead of one monolithic record detail — and your LWCs can live among them with visibility rules.

### 📏 Limits

*Governor & platform limits*

- Not available on all objects.
- Field-level visibility rules are evaluated server-side and add page-load cost.
- Migrating from page layouts is one-way in practice.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Dynamic Actions Integration

*Surfacing LWC-driven actions via Dynamic Actions.*

### 🌱 Simple

*Beginner - plain language*

**Dynamic Actions** lets admins control which buttons/actions appear on a record page (with visibility rules) — including quick actions backed by your LWCs.

### 📏 Limits

*Governor & platform limits*

- Supported on a limited set of objects and page types.
- Action visibility rules add evaluation cost on every page load.
- Mobile action support differs from desktop.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Experience Cloud LWC

*Building LWCs for Experience Cloud sites and guests.*

### 🌱 Simple

*Beginner - plain language*

**Experience Cloud LWC** means components built for community/portal sites — exposed to the Experience Builder, often used by **guest (unauthenticated)** users, with extra security and sharing considerations.

### 📏 Limits

*Governor & platform limits*

- Guest user access is restricted by "Secure guest user record access".
- Stricter CSP levels can block third-party resources entirely.
- Not all base components and platform modules are supported on Experience Cloud.
- Guest users have no API access - Apex must be exposed carefully.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Mobile Responsive LWC

*Building LWCs that adapt across desktop and mobile.*

### 🌱 Simple

*Beginner - plain language*

**Mobile-responsive LWC** means components that adapt their layout to screen size and work in the Salesforce mobile app — using SLDS responsive utilities and form factor awareness.

### 📏 Limits

*Governor & platform limits*

- Mobile pages must be assigned separately from desktop.
- Iframes and some navigation types behave inconsistently on device.
- Constrained CPU and memory make component count and payload size critical.
- Offline support is limited to specific features.

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
