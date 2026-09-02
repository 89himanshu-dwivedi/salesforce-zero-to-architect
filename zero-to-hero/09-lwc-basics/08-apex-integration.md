[Home](../index.md) / [09 · LWC Basics](index.md) / **Apex Integration**

# Apex Integration

2 topics · Series 9: LWC Basics

**Topics on this page**

- [Imperative Calls](#imperative-calls)
- [Wire Apex](#wire-apex)

## Imperative Calls

*Calling Apex on demand via promises for full control.*

### 🌱 Simple

*Beginner - plain language*

**Imperative Apex calls** invoke a method on demand (e.g., on button click) by importing it and calling it as a promise: `myMethod({param}).then(...).catch(...)`. You control exactly when it runs.

### 📏 Limits

*Governor & platform limits*

- No client-side caching unless the method is `cacheable=true`.
- Each call is a server round trip counting toward org request limits.
- Apex governor limits apply per call.
- Errors arrive as `error.body.message` - throw `AuraHandledException` server-side.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Wire Apex

*Reactively binding cacheable Apex methods via @wire.*

### 🌱 Simple

*Beginner - plain language*

**Wiring Apex** binds a `cacheable=true` Apex method to a property/function via `@wire` — it fetches reactively and caches results, like LDS adapters.

### 📏 Limits

*Governor & platform limits*

- Method must be `@AuraEnabled(cacheable=true)` and cannot perform DML.
- Reactive parameters need the `$` prefix.
- Cannot be awaited or re-invoked on demand.
- The user needs Apex class access and FLS on every returned field.

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
