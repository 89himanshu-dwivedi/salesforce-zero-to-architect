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
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

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
