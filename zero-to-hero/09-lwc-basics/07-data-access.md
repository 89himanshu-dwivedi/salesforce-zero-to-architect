[Home](../index.md) / [09 · LWC Basics](index.md) / **Data Access**

# Data Access

5 topics · Series 9: LWC Basics

**Topics on this page**

- [Wire Service](#wire-service)
- [LDS](#lds)
- [getRecord](#getrecord)
- [getFieldValue](#getfieldvalue)
- [getObjectInfo](#getobjectinfo)

## Wire Service

*Reactive, declarative data provisioning via @wire.*

### 🌱 Simple

*Beginner - plain language*

The **wire service** (`@wire`) reactively provisions data from Salesforce — Apex methods or LDS adapters — into a property or function. When inputs change, it re-fetches automatically.

### 📏 Limits

*Governor & platform limits*

- Cannot be invoked on demand and cannot be awaited - it is push-based.
- An `undefined` reactive parameter prevents the wire from firing at all.
- Wired Apex must be `cacheable=true`, which forbids DML.
- Results are client-cached and can be stale until `refreshApex`.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## LDS

*Lightning Data Service — declarative record CRUD without Apex.*

### 🌱 Simple

*Beginner - plain language*

**Lightning Data Service (LDS)** lets you read and modify records **without Apex** — via base components (`lightning-record-form`) or wire adapters (`getRecord`, `createRecord`, `updateRecord`). It handles caching, FLS, and sharing automatically.

### 📏 Limits

*Governor & platform limits*

- Single-record operations only - no bulk create or update.
- Not all objects are supported (some standard and external objects are excluded).
- No aggregate queries, SOSL or multi-object transactions.
- Related list adapters paginate and are not a substitute for SOQL at volume.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## getRecord

*The LDS wire adapter for fetching a single record's fields.*

### 🌱 Simple

*Beginner - plain language*

`getRecord` is an LDS wire adapter that fetches a **single record**'s fields by Id: `@wire(getRecord, {recordId: '$recordId', fields: [...]})`. It's cached and respects FLS.

### 📏 Limits

*Governor & platform limits*

- Requires either `fields` or `layoutTypes`; both cannot be arbitrary.
- Requesting a field the user cannot see returns an error, not a null.
- Cached client-side - stale until refreshed or updated through LDS.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## getFieldValue

*Safely extracting a field value from a getRecord result.*

### 🌱 Simple

*Beginner - plain language*

`getFieldValue(record, FIELD)` safely extracts a single field's value from a `getRecord` result, using the imported field reference — avoiding manual, error-prone path navigation.

### 📏 Limits

*Governor & platform limits*

- Returns undefined if the field was not requested in the wire.
- Does not traverse relationships beyond what was explicitly requested.
- Requires the field reference import or an exact API name string.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## getObjectInfo

*Fetching object metadata (fields, picklists, record types) via LDS.*

### 🌱 Simple

*Beginner - plain language*

`getObjectInfo` is an LDS wire adapter returning **metadata** about an object — its fields, child relationships, record type info, and more — without Apex describe calls.

### 📏 Limits

*Governor & platform limits*

- Returns metadata scoped to the running user's access.
- Large objects return substantial payloads - avoid calling it for many objects at once.
- Cached client-side; schema changes need a page refresh to appear.

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
