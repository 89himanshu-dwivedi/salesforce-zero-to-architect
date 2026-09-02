[Home](../index.md) / [02 · Data Modeling](index.md) / **Object Design**

# Object Design

4 topics · Series 2: Data Modeling

**Topics on this page**

- [Standard Object](#standard-object)
- [Custom Object](#custom-object)
- [External Object](#external-object)
- [Big Object](#big-object)

## Standard Object

*Salesforce-provided objects (Account, Contact, Opportunity, Case…) with built-in features.*

### 🌱 Simple

*Beginner - plain language*

A **standard object** is a table Salesforce ships with — Account, Contact, Lead, Opportunity, Case. They come ready-made with fields, page layouts, reports, security and mobile support. You use them as-is or add custom fields.

### 📏 Limits

*Governor & platform limits*

- Can't be deleted; some standard fields/behaviours are non-configurable; special objects (Activity) have constraints.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Custom Object

*You-defined tables (API name ends __c) for concepts standard objects don't cover.*

### 🌱 Simple

*Beginner - plain language*

A **custom object** is a table you create for data unique to your business — e.g., `Service_Visit__c` or `Property__c`. You define its fields, layouts and relationships. The API name ends in `__c`.

### 📏 Limits

*Governor & platform limits*

- Edition-based limits: 200 (Enterprise) to 2,000 (Unlimited).
- Roughly 800 custom fields per object; 25 roll-up summaries.
- Deleted objects hold their slot until purged.
- Object API names cannot be changed after creation without breaking references.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## External Object

*Virtual objects mapping to data outside Salesforce (OData/Apex) — no storage, real-time read.*

### 🌱 Simple

*Beginner - plain language*

An **external object** looks like a normal object but its data lives *outside* Salesforce (e.g., in an ERP). Salesforce reads it live on demand instead of copying it. API names end in `__x`.

### 📏 Limits

*Governor & platform limits*

- Salesforce Connect license; callout limits; restricted SOQL/DML; per-call row limits; no standard automation.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Big Object

*Massive on-platform storage (billions of rows) for archive/audit — async queries only.*

### 🌱 Simple

*Beginner - plain language*

A **Big Object** stores enormous volumes of data — billions of records — inside Salesforce, designed for archival and historical data you rarely need to query in real time. API names end in `__b`.

### 📏 Limits

*Governor & platform limits*

- Query only on the index prefix; no triggers/standard sharing; Async SOQL for big scans; storage is separate from regular data storage.

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
