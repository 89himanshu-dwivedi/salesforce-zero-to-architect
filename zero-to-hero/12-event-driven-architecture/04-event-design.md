[Home](../index.md) / [12 · Event Driven Architecture](index.md) / **Event Design**

# Event Design

4 topics · Series 12: Event Driven Architecture

**Topics on this page**

- [Event Schema](#event-schema)
- [Event Contract](#event-contract)
- [Event Versioning](#event-versioning)
- [Event Governance](#event-governance)

## Event Schema

*The defined structure/fields of an event payload.*

### 🌱 Simple

*Beginner - plain language*

An **event schema** defines the structure of an event — its fields, types, and meaning — so publishers and subscribers agree on the payload format. In Salesforce, platform-event/CDC schemas are Avro-based.

### 📏 Limits

*Governor & platform limits*

- Only additive changes are safe - removing or renaming a field breaks consumers.
- Field types are limited (no formula, no roll-up, limited relationships).
- Message maximum 1 MB.
- Schema is versioned by the event object itself, so v2 usually means a new event.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Event Contract

*The agreement between publisher and subscribers.*

### 🌱 Simple

*Beginner - plain language*

An **event contract** is the agreement — schema plus semantics and guarantees — that publishers and subscribers rely on. It defines what an event means, its fields, and its delivery behavior.

### 📏 Limits

*Governor & platform limits*

- There is no schema registry - the contract lives in your documentation.
- Consumers cannot negotiate a version; the publisher decides.
- Breaking changes require publishing a parallel event and a migration window.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Event Versioning

*Evolving events without breaking subscribers.*

### 🌱 Simple

*Beginner - plain language*

**Event versioning** is how you evolve an event's schema/meaning over time without breaking existing subscribers — favoring additive changes and, when breaking changes are unavoidable, introducing a new version.

### 📏 Limits

*Governor & platform limits*

- Platform Events have no native versioning - a new version means a new event object.
- Running two versions in parallel doubles publish allocation consumption.
- Custom object limits apply to the number of event definitions.
- Retirement requires confirming every consumer has migrated.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Event Governance

*Managing events as enterprise assets.*

### 🌱 Simple

*Beginner - plain language*

**Event governance** is the discipline of managing events as first-class enterprise assets — cataloging them, enforcing naming/schema standards, controlling access, and tracking consumers and versions.

### 📏 Limits

*Governor & platform limits*

- No native catalogue or registry - maintain one yourself.
- Publish and delivery allocations are shared org-wide, so one team can starve another.
- Event definitions count toward metadata limits.
- Without ownership, event sprawl becomes unmanageable within a year.

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
