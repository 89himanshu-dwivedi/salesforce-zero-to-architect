[Home](../index.md) / [05 · Developer Foundations](index.md) / **Metadata**

# Metadata

7 topics · Series 5: Developer Foundations

**Topics on this page**

- [Apex Class](#apex-class)
- [Apex Trigger](#apex-trigger)
- [Flow](#flow)
- [Object](#object)
- [Field](#field)
- [Layout](#layout)
- [Permission Set](#permission-set)

## Apex Class

*A unit of Apex code, stored in the org as metadata with its own XML descriptor.*

### 🌱 Simple

*Beginner - plain language*

An **Apex Class** is a file of server-side Salesforce code (business logic). As metadata it's two files: `MyClass.cls` (the code) and `MyClass.cls-meta.xml` (its API version + status).

### 📏 Limits

*Governor & platform limits*

- Max **6 MB** of Apex per org (chars).
- Single class file size and method/loop limits apply.
- Prod deploy needs 75% coverage + all triggers covered.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Apex Trigger

*Apex that runs automatically on DML events for an object — stored as its own metadata type.*

### 🌱 Simple

*Beginner - plain language*

An **Apex Trigger** is code that fires automatically when records are inserted, updated, deleted, or undeleted. As metadata it's `MyTrigger.trigger` + `MyTrigger.trigger-meta.xml` under `triggers/`.

### 📏 Limits

*Governor & platform limits*

- Trigger depth / recursion limits; counts toward Apex limits.
- Runs within the transaction's governor limits.
- Must be bulk-safe (200-record batches).

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Flow

*Declarative automation stored as metadata — versioned XML deployed like code.*

### 🌱 Simple

*Beginner - plain language*

A **Flow** is point-and-click automation. As metadata it's a `.flow-meta.xml` file under `flows/` that you can version-control and deploy between orgs just like Apex.

### 📏 Limits

*Governor & platform limits*

- Per-transaction Flow element/SOQL/DML limits (shared governor limits).
- Version accumulation; only one active version.
- XML diffs are not human-friendly.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Object

*A custom object's definition (and its sub-components) as deployable metadata.*

### 🌱 Simple

*Beginner - plain language*

An **Object** (custom object) in metadata is a folder under `objects/` containing the object's `.object-meta.xml` plus child folders for its **fields**, **list views**, **validation rules**, etc.

### 📏 Limits

*Governor & platform limits*

- Custom object/field count limits per edition.
- Relationship and field-type-change restrictions.
- Deploy order driven by dependencies.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Field

*A custom field definition deployed as its own granular metadata file.*

### 🌱 Simple

*Beginner - plain language*

A **Field** in metadata is a `.field-meta.xml` file inside its object's `fields/` folder, describing the field's type, label, length, and properties.

### 📏 Limits

*Governor & platform limits*

- Per-object field limits; type-specific limits (e.g., picklist values, text length).
- FLS stored separately from the field.
- Restricted/destructive type changes.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Layout

*Page layout metadata — controls field arrangement and is profile-assigned.*

### 🌱 Simple

*Beginner - plain language*

A **Layout** (page layout) in metadata is a `.layout-meta.xml` file under `layouts/` describing which fields, buttons, and related lists appear on a record page and in what order.

### 📏 Limits

*Governor & platform limits*

- Assignments stored in profiles (split metadata).
- References must exist in target.
- Many record types × profiles = assignment sprawl.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Permission Set

*The recommended way to grant access — deployable metadata that's additive to profiles.*

### 🌱 Simple

*Beginner - plain language*

A **Permission Set** grants extra permissions (object/field access, system perms, Apex class access) to specific users *on top of* their profile. In metadata it's a `.permissionset-meta.xml` file under `permissionsets/`.

### 📏 Limits

*Governor & platform limits*

- Additive only (can't remove a profile permission).
- Assignment is separate from deployment.
- Per-user assignment limits are high but finite; use groups.

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
