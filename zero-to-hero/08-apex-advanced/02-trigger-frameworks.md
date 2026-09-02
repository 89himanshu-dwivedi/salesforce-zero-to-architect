[Home](../index.md) / [08 · Apex Advanced](index.md) / **Trigger Frameworks**

# Trigger Frameworks

5 topics · Series 8: Apex Advanced

**Topics on this page**

- [One Trigger Per Object](#one-trigger-per-object)
- [Trigger Handler Pattern](#trigger-handler-pattern)
- [Trigger Dispatcher](#trigger-dispatcher)
- [Metadata Driven Framework](#metadata-driven-framework)
- [Feature Toggle Framework](#feature-toggle-framework)

## One Trigger Per Object

*The foundational rule: a single trigger per object for deterministic order.*

### 🌱 Simple

*Beginner - plain language*

**One trigger per object** means each sObject has exactly **one** trigger that delegates to a handler class. It guarantees predictable execution order and one place to control logic.

### 📏 Limits

*Governor & platform limits*

- No platform cap on trigger count, but execution order across triggers is undefined.
- Managed package triggers always run and cannot be reordered or bypassed.
- All triggers on the object share the same transaction limits.
- Max stack depth 16 applies across the whole chain.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Trigger Handler Pattern

*Moving trigger logic into a structured, testable handler class.*

### 🌱 Simple

*Beginner - plain language*

The **trigger handler pattern** puts all logic in a class with methods like `beforeInsert()`/`afterUpdate()`. The trigger just calls the handler, keeping logic organized and testable.

### 📏 Limits

*Governor & platform limits*

- Handler state in statics resets between transactions and batch chunks.
- Handlers must accept the full 200-record context - single-record logic fails on bulk.
- Deep handler layering adds CPU on hot paths; measure before adding another hop.
- Bypass flags must be scoped (per object, per user) - a global switch is itself a risk.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Trigger Dispatcher

*The routing layer that maps trigger context to handler hooks.*

### 🌱 Simple

*Beginner - plain language*

A **trigger dispatcher** is the code (often in the base handler's `run()`) that reads the context (before/after × insert/update/...) and calls the matching handler method.

### 📏 Limits

*Governor & platform limits*

- Dynamic instantiation via `Type.forName()` throws at runtime, not compile time, if a class is renamed.
- Each dispatch adds CPU; keep the handler list short on high-volume objects.
- Metadata-driven ordering relies on Custom Metadata, which is cached and free of SOQL cost.
- The dispatcher cannot reorder managed package triggers.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Metadata Driven Framework

*Config-driven trigger handling using Custom Metadata for handlers and toggles.*

### 🌱 Simple

*Beginner - plain language*

A **metadata-driven framework** stores *which* handlers run (and their order/toggles) in **Custom Metadata**, so behavior is configured by admins — not hard-coded — and changed without deployment.

### 📏 Limits

*Governor & platform limits*

- Custom Metadata `getAll()`/`getInstance()` is cached and consumes no SOQL.
- CMDT records must be included in the package or they are missing after deploy.
- Runtime type resolution fails at execution time if a class name in metadata is wrong.
- Adds indirection that makes debug-log attribution harder - log the resolved handler name.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Feature Toggle Framework

*Switching features/automation on or off via configuration at runtime.*

### 🌱 Simple

*Beginner - plain language*

A **feature toggle** (flag) turns automation/features on or off via **Custom Metadata/Settings** — no deployment. Used to bypass triggers during data loads, do phased rollouts, or kill a misbehaving feature instantly.

### 📏 Limits

*Governor & platform limits*

- Custom Setting values do not deploy; Custom Metadata records can.
- Every active flag doubles the regression matrix - flags need owners and expiry dates.
- `FeatureManagement.checkPermission` is cached and free of SOQL cost.
- Flags are the fastest incident control available - config change, not deploy.

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
