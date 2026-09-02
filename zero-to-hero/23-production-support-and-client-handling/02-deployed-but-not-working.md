[Home](../index.md) / [23 · Production Support & Client Handling](index.md) / **Deployed But Not Working**

# Deployed But Not Working

10 topics · Series 23: Production Support & Client Handling

**Topics on this page**

- [Deploy Succeeded Feature Missing](#deploy-succeeded-feature-missing)
- [Permission Set Not Assigned](#permission-set-not-assigned)
- [Profile Field-Level Security](#profile-field-level-security)
- [Page Layout Not Updated](#page-layout-not-updated)
- [Flow Active Version Wrong](#flow-active-version-wrong)
- [Trigger Not Firing](#trigger-not-firing)
- [Cache Showing Old Version](#cache-showing-old-version)
- [Sandbox Config Not Migrated](#sandbox-config-not-migrated)
- [Hardcoded Sandbox Id](#hardcoded-sandbox-id)
- [Feature Flag Off in Prod](#feature-flag-off-in-prod)

## Deploy Succeeded Feature Missing

*Green deploy, but users don't see the feature — why.*

### 🌱 Simple

*Beginner - plain language*

A **successful deploy** only means the metadata saved — not that users can *use* it. The component almost always exists but is **not visible/assigned**: missing permission set, FLS, layout, app/tab access, or it's on an inactive Flow version. Deploy ≠ enabled.

### 📏 Limits

*Governor & platform limits*

- Metadata deploy: **10,000 files** / **39 MB**.
- Custom Metadata and Custom Setting *records* are data, not included by default.
- Profiles deploy only permissions for components in the same package.
- Flow active-version behaviour depends on API version and deploy settings.
- Deploy history detail is easy to lose - export it during incidents.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Permission Set Not Assigned

*Code/object deployed but users get 'insufficient privileges'.*

### 🌱 Simple

*Beginner - plain language*

Permission sets **don't assign themselves on deploy**. If users get `Insufficient Privileges` or can't run a feature, the permission set (object/field/Apex class/tab access) likely isn't **assigned to them**. Assign it — manually, via permission set group, or automation.

### 📏 Limits

*Governor & platform limits*

- Max **1,000** permission sets and **200** permission set groups per org.
- Group recalculation can take minutes after an edit.
- Permission sets are additive - only muting sets inside a group can remove.
- Some permissions require a Permission Set Licence first.
- PermissionSetAssignment is a setup object - Mixed DML rules apply.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Profile Field-Level Security

*Field deployed but blank/missing — FLS not granted.*

### 🌱 Simple

*Beginner - plain language*

A new field can deploy but be **hidden or read-only** because **Field-Level Security (FLS)** isn't granted to the user's profile/permission set. The field shows blank, is missing from layouts, or APIs can't write it. Grant FLS (Visible/Editable) per profile or permission set.

### 📏 Limits

*Governor & platform limits*

- FLS cannot be set on required fields, master-detail fields, or some standard fields.
- Profile metadata deploys partially - pair with permission sets.
- Not enforced in Apex system mode.
- Formula fields inherit access from referenced fields - a user can be blocked by a field they never see.
- Shield encrypted fields need "View Encrypted Data" separately.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Page Layout Not Updated

*Component/field exists but isn't on the page users see.*

### 🌱 Simple

*Beginner - plain language*

If a field/related list/component "isn't there," it may simply **not be on the layout that profile uses**. Orgs have multiple layouts per object by profile/record type — you might have updated the wrong one. Check the **layout assignment**.

### 📏 Limits

*Governor & platform limits*

- Assignment is per Profile x Record Type - not per permission set.
- Dynamic Forms are not available on every object.
- Each related list adds query cost on page load.
- Layouts and assignments are separate metadata types.
- Compact layouts are separate again and drive highlights and mobile cards.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Flow Active Version Wrong

*Flow deployed but old logic runs — wrong active version.*

### 🌱 Simple

*Beginner - plain language*

Flows are **versioned**, and only the **active version** runs. A deploy can leave the **old version active** (or deploy it inactive), so users still get the old behavior. Activate the correct version.

### 📏 Limits

*Governor & platform limits*

- **50 versions** per flow; deploys fail at the limit.
- Only **one active version** at a time.
- Flow limits mirror Apex: 100 SOQL, 150 DML, 10s CPU shared with the transaction.
- Execution order across multiple record-triggered flows is not guaranteed.
- You cannot delete a version that is in use - deactivate first.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Trigger Not Firing

*Trigger deployed but nothing happens — common causes.*

### 🌱 Simple

*Beginner - plain language*

A deployed trigger that "doesn't fire" is usually **inactive, on the wrong event, bypassed by a guard/flag, or its DML path isn't actually hit**. Confirm it's active, on the right event (before/after insert/update), and that a recursion flag isn't short-circuiting it.

### 📏 Limits

*Governor & platform limits*

- Triggers fire in chunks of **200**; limits are shared across the transaction.
- Max stack depth **16** for recursive DML.
- Roll-up and formula recalculation do not fire triggers.
- Order across multiple triggers on one object is undefined.
- Before-save Flows run before before-triggers.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Cache Showing Old Version

*Users see old LWC/page after deploy — it's cached.*

### 🌱 Simple

*Beginner - plain language*

After deploying UI changes, users sometimes see the **old version** because of **browser/CDN/Lightning caching**. The code is updated; the client is stale. A hard refresh, cache clear, or waiting for cache rollover usually fixes it.

### 📏 Limits

*Governor & platform limits*

- Platform Cache: org cache default TTL **24h** (max 48h), session cache **8h**.
- Capacity is purchased; entries can be evicted before TTL.
- Max **100 KB** per org-cache item.
- `cacheable=true` Apex cannot perform DML.
- No API flushes the Lightning client cache for all users.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Sandbox Config Not Migrated

*Code deployed but the config it depends on didn't.*

### 🌱 Simple

*Beginner - plain language*

Features often depend on **configuration data** — Custom Metadata, Custom Settings, Custom Labels, record types, queues — that **isn't part of a code deploy** unless explicitly included. The code runs but finds **no config**, so it misbehaves or errors.

### 📏 Limits

*Governor & platform limits*

- Custom Settings and most reference data are excluded from metadata deploys.
- Named Credential secrets never deploy, by design.
- Max **100 scheduled jobs** per org.
- Sandbox refresh wipes org-specific config and regenerates Ids.
- Profile deployments are partial.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Hardcoded Sandbox Id

*Works in sandbox only because an Id is hardcoded.*

### 🌱 Simple

*Beginner - plain language*

A frequent "works in sandbox, breaks in prod" cause: a **hardcoded record/user/queue Id** (15/18 chars) that exists only in the sandbox. In prod that Id doesn't exist → errors or wrong behavior. Never hardcode Ids — look them up by a stable key.

### 📏 Limits

*Governor & platform limits*

- **100**`describeSObjects` calls per transaction - cache results.
- 15-character Ids are case-sensitive, 18-character are not; mixing them causes silent mismatches.
- Sandbox refresh regenerates all Ids.
- Formula and validation rule Id references are missed by Apex-only scanners.
- Hardcoded Ids inside managed packages cannot be fixed by you.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Feature Flag Off in Prod

*Deployed behind a flag that's disabled in production.*

### 🌱 Simple

*Beginner - plain language*

If you ship features behind a **feature flag** (Custom Setting/Metadata/Custom Permission), the code can deploy while the flag stays **off in prod** — so nothing happens. That's by design for safe rollout, but you must **enable the flag** when ready.

### 📏 Limits

*Governor & platform limits*

- CMDT `getInstance()` is cached and does not consume SOQL limits.
- Custom Setting values do not deploy.
- Deploying CMDT records requires them to be in the package.
- Each active flag doubles the regression matrix.
- Custom permissions count toward org metadata limits.

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
