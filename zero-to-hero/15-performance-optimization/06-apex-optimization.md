[Home](../index.md) / [15 · Performance Optimization](index.md) / **Apex Optimization**

# Apex Optimization

5 topics · Series 15: Performance Optimization

**Topics on this page**

- [Bulkification](#bulkification)
- [Collection Optimization](#collection-optimization)
- [Map Based Processing](#map-based-processing)
- [Lazy Loading](#lazy-loading)
- [Caching](#caching)

## Bulkification

*Process collections, not single records, to respect limits.*

### 🌱 Simple

*Beginner - plain language*

**Bulkification** means writing Apex to handle **many records at once** — querying and doing DML on collections outside loops — so 1 record or 200 records use the same small number of SOQL/DML statements.

### 📏 Limits

*Governor & platform limits*

- Triggers fire in chunks of 200 - all logic must handle collections.
- 100 SOQL / 150 DML statements / 10,000 DML rows per transaction.
- Limits are shared across every automation in the transaction.
- Heap often fails before row limits on wide objects.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Collection Optimization

*Choosing the right collection and avoiding nested loops.*

### 🌱 Simple

*Beginner - plain language*

**Collection optimization** is picking the right data structure (**List, Set, Map**) and access pattern so lookups are fast and you avoid expensive **nested loops** that consume CPU time.

### 📏 Limits

*Governor & platform limits*

- `List.contains()` is O(n) - use a Set inside loops.
- Maps cost more heap than Lists due to key and hash overhead.
- Nested collections multiply heap and are a common 6 MB failure.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Map Based Processing

*Keying data by id for O(1) correlation across objects.*

### 🌱 Simple

*Beginner - plain language*

**Map-based processing** uses `Map<Id, SObject>` (or keyed Maps) to relate records across queries and loops in **constant time** — the backbone of bulkified, CPU-efficient Apex.

### 📏 Limits

*Governor & platform limits*

- Map keys are restricted to primitives, sObjects, enums and classes with equals/hashCode.
- String keys are case-insensitive, which can silently merge entries.
- `Map<Id, sObject>` of 50,000 wide records will exceed heap.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Lazy Loading

*Compute/fetch data only when actually needed.*

### 🌱 Simple

*Beginner - plain language*

**Lazy loading** defers expensive work (queries, computation) until it's **actually needed** — and only does it **once**, caching the result — instead of loading everything upfront.

### 📏 Limits

*Governor & platform limits*

- Deferred work still consumes governor limits when it eventually runs.
- In LWC, dynamic imports add a network round trip on first use.
- Lazy patterns can hide the true cost until peak load.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Caching

*Reusing computed/queried results to avoid repeat work.*

### 🌱 Simple

*Beginner - plain language*

**Caching** stores the result of an expensive query or computation so it can be **reused** instead of redone — within a transaction (variables/Maps), across requests (Platform Cache), or on the client (LWC).

### 📏 Limits

*Governor & platform limits*

- Platform Cache is best-effort - entries can be evicted before TTL.
- Maximum 100 KB per org-cache item; capacity is purchased.
- No automatic invalidation on deploy - version your keys.
- Never cache sharing-restricted data in org cache.

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
