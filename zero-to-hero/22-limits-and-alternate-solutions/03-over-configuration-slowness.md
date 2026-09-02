[Home](../index.md) / [22 · Limits & Alternate Solutions](index.md) / **Over-Configuration Slowness**

# Over-Configuration Slowness

16 topics · Series 22: Limits & Alternate Solutions

**Topics on this page**

- [Too Many Sharing Rules](#too-many-sharing-rules)
- [Deep Role Hierarchy](#deep-role-hierarchy)
- [Too Many Permission Sets](#too-many-permission-sets)
- [Sharing Recalculation Storms](#sharing-recalculation-storms)
- [Group Membership Locks](#group-membership-locks)
- [Too Many Triggers per Object](#too-many-triggers-per-object)
- [Too Many Flows per Object](#too-many-flows-per-object)
- [Workflow + PB Stacking](#workflow-plus-pb-stacking)
- [Too Many Validation Rules](#too-many-validation-rules)
- [Cross-Object Formula Overload](#cross-object-formula-overload)
- [Too Many Roll-Up Summaries](#too-many-roll-up-summaries)
- [Too Many Fields on Object](#too-many-fields-on-object)
- [Heavy Page Layouts](#heavy-page-layouts)
- [Public Groups Explosion](#public-groups-explosion)
- [Deep Master-Detail Chains](#deep-master-detail-chains)
- [Metadata Bloat](#metadata-bloat)

## Too Many Sharing Rules

*Hundreds of sharing rules slow saves & recalcs even without errors.*

### 🌱 Simple

*Beginner - plain language*

This is the "**no error but org is slow**" problem. Every owner/criteria-based **sharing rule** must be evaluated and stored as share rows. Pile up hundreds (especially *criteria-based* on big objects) and every insert/update/owner-change triggers **sharing recalculation** — saves crawl, deployments time out, and "Recalculate Sharing" runs for hours.

### 📏 Limits

*Governor & platform limits*

- 300 sharing rules per object; 50 criteria-based.
- Recalculation is asynchronous and can take hours.
- Group membership locks during recalculation are org-wide.
- Rules cannot remove access, only grant.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Deep Role Hierarchy

*Too many roles / too deep a hierarchy slows sharing math.*

### 🌱 Simple

*Beginner - plain language*

The role hierarchy grants record access upward. A **very deep or very wide** hierarchy (thousands of roles, 10+ levels) makes **sharing recalculation and group maintenance** expensive — every ownership change ripples through the tree. Salesforce supports up to ~500 roles / 10 levels, but performance suffers long before the hard cap.

### 📏 Limits

*Governor & platform limits*

- 500 roles recommended (hard limit higher but unsupported in practice).
- 10 levels of hierarchy recommended.
- Role changes trigger asynchronous recalculation.
- Territory hierarchy is separate and adds its own cost.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Too Many Permission Sets

*Hundreds of permission sets/assignments add login & query cost.*

### 🌱 Simple

*Beginner - plain language*

Permission sets are the right tool — but **hundreds of tiny ones**, or thousands of individual assignments, add overhead to **login, permission evaluation, and metadata**. The alternate is **Permission Set Groups**: bundle related permission sets and assign the group, with muting for exceptions.

### 📏 Limits

*Governor & platform limits*

- 1,000 permission sets; 200 permission set groups.
- Group recalculation is asynchronous.
- Permission sets cannot remove profile permissions - only muting sets can.
- Some permissions require a Permission Set Licence.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Sharing Recalculation Storms

*Mass owner/role changes trigger org-wide recalcs that lock things.*

### 🌱 Simple

*Beginner - plain language*

A "**recalc storm**" happens when a bulk change (mass reassign owners, change a role, add a sharing rule, data load) forces Salesforce to **rebuild share rows** across millions of records at once — locking records, slowing the whole org, and sometimes running for hours.

### 📏 Limits

*Governor & platform limits*

- Recalculation is asynchronous and can run for hours.
- Group membership locks are org-wide.
- Deferred sharing must be explicitly re-enabled.
- Most recalculations cannot be cancelled.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Group Membership Locks

*Group/role membership changes serialize and block each other.*

### 🌱 Simple

*Beginner - plain language*

Public group and role **membership recalculation** uses internal locks. Doing many membership changes in parallel (or while sharing recalcs run) causes **lock contention** — operations queue up and slow down. Alternate: **serialize membership changes and batch them off-peak**.

### 📏 Limits

*Governor & platform limits*

- Group membership operations lock broadly.
- Granular Locking must be enabled by Salesforce Support.
- Lock wait is around 10 seconds.
- Territory and role changes lock the same structures.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Too Many Triggers per Object

*Multiple triggers on one object = unpredictable, slow saves.*

### 🌱 Simple

*Beginner - plain language*

Salesforce lets you create **many triggers on one object**, but you shouldn't: order isn't guaranteed, they each re-query, and together they balloon **CPU and save time**. Best practice: **one trigger per object** delegating to a handler class.

### 📏 Limits

*Governor & platform limits*

- No documented cap, but limits are shared.
- Execution order across triggers is not guaranteed.
- Max stack depth 16.
- Managed package triggers cannot be reordered.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Too Many Flows per Object

*Stacked record-triggered Flows compound save time.*

### 🌱 Simple

*Beginner - plain language*

Multiple **record-triggered Flows** on the same object all run on save, each doing queries and updates. Stack several (plus triggers and old Process Builders) and saves get slow and order-dependent. Alternate: **consolidate to fewer Flows per timing (before/after), ordered, and move heavy logic to Apex/async**.

### 📏 Limits

*Governor & platform limits*

- 50 versions per flow.
- Flow limits mirror Apex and are shared with the transaction.
- Order across flows is not guaranteed without trigger order.
- Only one active version per flow.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Workflow + PB Stacking

*Legacy Workflow Rules + Process Builder piled on top of Flows.*

### 🌱 Simple

*Beginner - plain language*

Many orgs run **Workflow Rules + Process Builder + Flows + triggers** on the same object — all firing on save, sometimes re-triggering each other. This is a top cause of slow, unpredictable saves. The alternate is **migrate WF/PB to Flow and consolidate** (Salesforce has retired WF/PB).

### 📏 Limits

*Governor & platform limits*

- Workflow field updates re-fire triggers once.
- Process Builder and Workflow Rules are retired for new creation.
- They share the transaction's SOQL/DML/CPU budget.
- Order of execution places them after after-triggers.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Too Many Validation Rules

*Dozens of validation rules add CPU and block legit saves.*

### 🌱 Simple

*Beginner - plain language*

Each **validation rule** runs on every save. Dozens of complex, cross-object formula rules add CPU and can **block bulk loads**. Alternate: **consolidate rules, simplify formulas, and bypass them for trusted integration users** via a custom permission.

### 📏 Limits

*Governor & platform limits*

- 500 active validation rules per object.
- They apply to API and Apex DML, not just the UI.
- Re-evaluated after workflow field updates.
- Formula size limits apply per rule.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Cross-Object Formula Overload

*Many cross-object/spanning formulas slow queries & reports.*

### 🌱 Simple

*Beginner - plain language*

**Cross-object (spanning) formula fields** compute by traversing relationships at *read* time. Lots of them — especially nested ones — slow **reports, list views, and queries**, and they **can't be indexed** (so filtering on them is non-selective). Alternate: **flatten the value into a real field** updated by automation.

### 📏 Limits

*Governor & platform limits*

- 10 unique cross-object relationships per object across all formulas.
- Formula compile size 5,000 bytes; 3,900 characters.
- Not indexable in most cases.
- Evaluated on every query and every row.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Too Many Roll-Up Summaries

*Many rollups (incl. DLRS) recalc on every child change.*

### 🌱 Simple

*Beginner - plain language*

Roll-up summary fields recompute when children change. Many rollups on a parent — or **declarative-Apex rollups (DLRS)** across lookups — add work to **every child insert/update/delete** and can amplify under data skew. Alternate: **limit rollups, use async rollups for lookups, and precompute via scheduled Batch where real-time isn't required**.

### 📏 Limits

*Governor & platform limits*

- 25 roll-up summary fields per object.
- Only on master-detail (or via DLRS on lookups).
- Recalculation locks the parent record.
- Roll-up updates do not fire triggers.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Too Many Fields on Object

*Hundreds of fields slow page loads, queries, and describes.*

### 🌱 Simple

*Beginner - plain language*

Objects have field limits (e.g. ~**800/500 custom fields** depending on edition), but performance degrades well before that: **page layouts render slower, `SELECT *`-style queries cost more, and describes get heavier**. Alternate: **archive unused fields, split rarely-used data, and query only needed fields**.

### 📏 Limits

*Governor & platform limits*

- ~800 custom fields per object (500 in some editions).
- Roll-up summaries count separately (25).
- Deleted fields sit in the field Recycle Bin for 15 days.
- Wide objects degrade query and report performance.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Heavy Page Layouts

*Too many components/related lists make record pages crawl.*

### 🌱 Simple

*Beginner - plain language*

A Lightning record page with **many components, related lists, and report charts** issues lots of parallel server calls on load — making pages feel slow. Alternate: **fewer components, tabs/accordions to lazy-load, and conditional visibility** so only what's needed loads.

### 📏 Limits

*Governor & platform limits*

- Related lists each add query cost.
- Lightning pages have component count guidance, not a hard cap.
- Dynamic Forms are not available on all objects.
- Usage App data is aggregated daily, not real-time.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Public Groups Explosion

*Thousands of nested public groups slow membership recalcs.*

### 🌱 Simple

*Beginner - plain language*

Public groups are great for sharing — but **thousands of them, deeply nested**, make **group membership recalculation** expensive and slow sharing operations. Alternate: **fewer, flatter groups** and reuse them across sharing rules rather than creating one-off groups.

### 📏 Limits

*Governor & platform limits*

- Group membership operations lock broadly.
- Nested groups expand at recalculation.
- Recalculation is asynchronous and can take hours.
- Empty groups still cost.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Deep Master-Detail Chains

*Long MD/lookup chains cascade locks, rollups, and recalcs.*

### 🌱 Simple

*Beginner - plain language*

Master-Detail chains give rollups and cascade delete/sharing, but **deep chains** (3 levels is the max for MD) mean a single child change can **cascade rollups and lock parents up the chain** — slow and lock-prone under volume/skew. Alternate: **flatten where possible, use lookups + async rollups, and avoid skew**.

### 📏 Limits

*Governor & platform limits*

- 3 levels of custom master-detail.
- 2 master-detail relationships per object.
- Cascade-deleted rows count toward DML limits.
- Conversion between master-detail and lookup is restricted.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Metadata Bloat

*Huge metadata (classes, fields, layouts) slows deploys & org.*

### 🌱 Simple

*Beginner - plain language*

Years of accumulation — thousands of **Apex classes, fields, layouts, flows, reports** — slow **deployments, test runs, and Setup**, and make the org hard to maintain. Alternate: **regular cleanup, unmanaged-to-package modularization, and removing dead metadata**.

### 📏 Limits

*Governor & platform limits*

- Metadata deploy caps: 10,000 files / 39 MB.
- Per-object and per-org component limits apply.
- Deleted fields retain data for 15 days.
- Dependencies block deletion and must be resolved first.

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
