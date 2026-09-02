[Home](../index.md) / [22 · Limits & Alternate Solutions](index.md) / **Storage & Org Limits**

# Storage & Org Limits

10 topics · Series 22: Limits & Alternate Solutions

**Topics on this page**

- [Data Storage Limit](#data-storage-limit)
- [File Storage Limit](#file-storage-limit)
- [Custom Object Limit](#custom-object-limit)
- [Custom Fields per Object](#custom-fields-per-object)
- [Field History 20 Fields](#field-history-20-fields)
- [Picklist Value Limits](#picklist-value-limits)
- [Custom Tabs Limit](#custom-tabs-limit)
- [Apex Code Size Limit](#apex-code-size-limit)
- [Big Object Storage](#big-object-storage)
- [Custom Apps Limit](#custom-apps-limit)

## Data Storage Limit

*Each record ~2KB; storage fills up — archive & offload.*

### 🌱 Simple

*Beginner - plain language*

Salesforce charges **data storage** (most records count as ~**2KB each** regardless of actual size). Millions of rows fill your allocation, and buying more is costly. Alternate: **archive cold data to Big Objects/external stores and delete what you don't need**.

### 📏 Limits

*Governor & platform limits*

- Per-edition base plus per-licence increments.
- Most records count as 2 KB.
- Big Object storage is separate and effectively unlimited (paid).
- Recycle Bin capacity is 25x data storage.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## File Storage Limit

*Attachments/Files consume file storage — offload large files.*

### 🌱 Simple

*Beginner - plain language*

**File storage** (Files, Attachments, ContentDocuments) is separate from data storage and fills with documents/images. Alternate: **store large/!rarely-accessed files in external storage** (S3, SharePoint) and reference them, using **Files Connect or a link field**.

### 📏 Limits

*Governor & platform limits*

- Separate allocation from data storage.
- Each ContentVersion counts.
- Max single file size 2 GB (Files), 25 MB for Attachments.
- Orphaned ContentDocuments persist after record deletion.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Custom Object Limit

*Edition caps on custom objects — model efficiently.*

### 🌱 Simple

*Beginner - plain language*

Each edition caps the number of **custom objects** (e.g. 200–2,000 depending on edition/add-ons). Creating an object for every tiny need hits this. Alternate: **reuse objects with record types, use generic "type" fields, and remove unused objects**.

### 📏 Limits

*Governor & platform limits*

- 200-2,000 custom objects by edition.
- Big Objects: separate limit (typically 100).
- External Objects: separate limit (typically 100-200).
- Deleted objects hold their slot until purged.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Custom Fields per Object

*~800/500 custom fields per object — and perf well before.*

### 🌱 Simple

*Beginner - plain language*

An object allows up to ~**800 (Unlimited) / 500 (Enterprise)** custom fields, but performance degrades before that. Alternate: **remove unused fields, split rarely-used data into a related object, and avoid field-per-attribute anti-patterns**.

### 📏 Limits

*Governor & platform limits*

- ~800 custom fields per object; 500 in some editions.
- 25 roll-up summaries counted separately.
- Deleted fields occupy the slot for 15 days.
- Relationship fields count toward the total.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Field History 20 Fields

*Track history on max 20 fields per object — choose wisely.*

### 🌱 Simple

*Beginner - plain language*

Field History Tracking is limited to **20 fields per object**, and history rows themselves consume storage. Alternate for broader/longer auditing: **Field Audit Trail (extends retention) or a custom audit solution / Big Object**.

### 📏 Limits

*Governor & platform limits*

- 20 tracked fields per object.
- 18-24 month retention (10 years with Field Audit Trail).
- Formula and roll-up fields cannot be tracked.
- History rows consume storage.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Picklist Value Limits

*Picklists cap values; huge picklists slow UI & maintenance.*

### 🌱 Simple

*Beginner - plain language*

Picklists have limits on the **number of active values** (and total label length). Massive picklists are also a UX/maintenance problem. Alternate: **use a lookup to a custom object** for large, frequently-changing value sets.

### 📏 Limits

*Governor & platform limits*

- 1,000 active values; 500 for multi-select.
- 100 selected values per multi-select field.
- Global Value Sets: 500 values.
- Inactive values still count toward some limits.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Custom Tabs Limit

*Edition caps custom tabs — use App nav & utility bar.*

### 🌱 Simple

*Beginner - plain language*

Editions cap the number of **custom tabs**. Creating a tab for everything hits it. Alternate: **group navigation in Lightning Apps, use the utility bar, and surface objects via list views/related lists** instead of dedicated tabs.

### 📏 Limits

*Governor & platform limits*

- Edition-dependent custom tab limits.
- Separate allowances for object, web and Visualforce tabs.
- Package tabs have their own allocation.
- Tabs must be exposed per profile/permission set.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Apex Code Size Limit

*Org-wide ~6MB Apex character limit — modularize & manage.*

### 🌱 Simple

*Beginner - plain language*

All Apex in an org shares a **~6MB character limit** (managed-package code is excluded). Big monolithic orgs approach it. Alternate: **refactor duplication, move logic to config/Flow where appropriate, and modularize into managed/unlocked packages**.

### 📏 Limits

*Governor & platform limits*

- 6 MB Apex characters per org.
- Test classes excluded.
- Managed package code excluded.
- Exceeding it blocks all Apex deployment.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Big Object Storage

*Big Objects = billions of rows, cheap — but query-restricted.*

### 🌱 Simple

*Beginner - plain language*

**Big Objects** store **billions of records** at low cost — perfect for archive/audit/history. The trade-off: you can only query them efficiently via a **predefined index** (and use Async SOQL for bulk). So they're a storage *alternate*, not a replacement for transactional objects.

### 📏 Limits

*Governor & platform limits*

- Index defined at creation, immutable.
- Query only on indexed fields, left to right.
- No triggers, workflows, or standard reporting.
- Storage is separate and paid.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Custom Apps Limit

*Edition caps Lightning apps — consolidate by persona.*

### 🌱 Simple

*Beginner - plain language*

Editions cap the number of **custom apps**. Building an app per team multiplies them. Alternate: **fewer persona-based apps with tailored navigation and app-specific home pages**.

### 📏 Limits

*Governor & platform limits*

- Edition-dependent custom app limits.
- Managed package apps counted separately.
- Each app carries its own navigation and page assignments.
- Visibility controlled per profile/permission set.

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
