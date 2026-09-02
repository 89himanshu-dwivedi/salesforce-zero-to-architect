[Home](../index.md) / [06 · SOQL & SOSL](index.md) / **Advanced SOQL**

# Advanced SOQL

17 topics · Series 6: SOQL & SOSL

**Topics on this page**

- [Query Selectivity](#query-selectivity)
- [Query Cost](#query-cost)
- [Query Plan Tool](#query-plan-tool)
- [Explain Plan](#explain-plan)
- [Standard Index](#standard-index)
- [Custom Index](#custom-index)
- [External Id Index](#external-id-index)
- [Unique Index](#unique-index)
- [Compound Index](#compound-index)
- [Skinny Tables](#skinny-tables)
- [Divisions](#divisions)
- [WITH SECURITY_ENFORCED](#with-security-enforced)
- [USER_MODE](#user-mode)
- [stripInaccessible](#stripinaccessible)
- [Database.query()](#database-query)
- [Bind Variables](#bind-variables)
- [Dynamic Filters](#dynamic-filters)

## Query Selectivity

*Whether a query's filters let the optimizer use an index instead of scanning the whole table.*

### 🌱 Simple

*Beginner - plain language*

A query is **selective** when its filter narrows to a small enough fraction of records that Salesforce can use an **index** instead of scanning every row. Selective = fast; non-selective = slow or fails on large objects.

### 📏 Limits

*Governor & platform limits*

- Selectivity thresholds (~10%/5%, with caps).
- Negation/leading wildcards/formulas are non-selective.
- Skew can defeat indexes.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Query Cost

*The optimizer's numeric estimate of how expensive a query is to run.*

### 🌱 Simple

*Beginner - plain language*

**Query cost** is a number the optimizer assigns to a query plan — lower is cheaper/faster. The **Query Plan tool** shows it; a cost **under 1.0** generally means an index will be used.

### 📏 Limits

*Governor & platform limits*

- Cost ≥ 1 ≈ full scan.
- Depends on (periodically refreshed) statistics.
- Skew distorts estimates.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Query Plan Tool

*The Developer Console feature that reveals how the optimizer will run a query.*

### 🌱 Simple

*Beginner - plain language*

The **Query Plan tool** (Developer Console) shows the **plans** the optimizer considered for your SOQL, their **cost**, which index (if any) would be used, and how many rows each plan touches — so you can diagnose slow queries.

### 📏 Limits

*Governor & platform limits*

- Dev Console only; must be enabled.
- Estimates based on current statistics.
- Reflects the org it's run in.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Explain Plan

*The optimizer's explanation of plan choice — exposed via the Query Plan tool and API.*

### 🌱 Simple

*Beginner - plain language*

**Explain Plan** is the optimizer's breakdown of *why* it chose a plan — the same data the Query Plan tool shows, also available through the **REST API** (`/query/?explain=...`) for programmatic analysis.

### 📏 Limits

*Governor & platform limits*

- Estimate only (doesn't execute).
- Reflects the target org's statistics.
- API-based; needs representative data.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Standard Index

*Indexes Salesforce maintains automatically on key system and standard fields.*

### 🌱 Simple

*Beginner - plain language*

**Standard indexes** are built-in indexes Salesforce keeps on certain fields automatically — like `Id`, `Name`, `OwnerId`, audit dates (`CreatedDate`, `SystemModstamp`), and **lookup/master-detail** fields. Filtering on these is index-friendly out of the box.

### 📏 Limits

*Governor & platform limits*

- Fixed set of auto-indexed fields.
- Can't add/remove standard indexes.
- Skew can defeat them.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Custom Index

*An index you add (often via Salesforce) on a custom or standard field to make it selective.*

### 🌱 Simple

*Beginner - plain language*

A **custom index** is an index placed on a field that isn't standard-indexed, so filtering on it becomes selective. Some are created automatically (External Id, Unique fields); others are requested through **Salesforce support**.

### 📏 Limits

*Governor & platform limits*

- Some types can't be indexed.
- Non-auto indexes need a Salesforce case.
- Skew can render an index non-selective.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## External Id Index

*The automatic index on External Id fields — enabling fast lookups and upserts.*

### 🌱 Simple

*Beginner - plain language*

Marking a field as an **External Id** automatically gives it a **custom index**, so filtering by it is fast — and it can be used as the key for **upsert** operations from integrations.

### 📏 Limits

*Governor & platform limits*

- Limited field types (Text/Number/Email/Auto Number).
- Up to a few External Ids per object.
- Case-sensitivity affects matching.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Unique Index

*An index that also enforces no duplicate values — and makes the field selective.*

### 🌱 Simple

*Beginner - plain language*

Marking a field **Unique** enforces that **no two records share the same value** and automatically indexes it (so filtering is selective). Great for natural keys like SKU or employee number.

### 📏 Limits

*Governor & platform limits*

- Limited field types.
- Fails to enable with existing dupes.
- Concurrency/lock considerations on bulk load.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Compound Index

*A two-column index used when two fields together make a query selective.*

### 🌱 Simple

*Beginner - plain language*

A **compound (two-column) index** covers **two fields together**. When neither field alone is selective but the **combination** is, a two-column index makes the query fast.

### 📏 Limits

*Governor & platform limits*

- Two-column custom indexes must be requested through Salesforce Support.
- Column order matters - the leading column must be in the filter.
- Not all field type combinations are supported.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Skinny Tables

*A Salesforce-maintained denormalized copy of frequently-used fields for fast LDV reads.*

### 🌱 Simple

*Beginner - plain language*

A **skinny table** is a behind-the-scenes copy of an object containing only the **most-used fields**, kept in sync by Salesforce. Queries/reports against it are faster because there's less data and no joins to the "detail" table.

### 📏 Limits

*Governor & platform limits*

- Field-count limit; no formulas/some types.
- Requested via Salesforce; not auto in sandboxes.
- Excludes archived/soft-deleted rows.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Divisions

*A legacy data-partitioning feature that segments records for query scoping.*

### 🌱 Simple

*Beginner - plain language*

**Divisions** let you partition records into segments (e.g., by region or business unit) so users and queries can **scope to one division**, improving focus and performance on very large datasets.

### 📏 Limits

*Governor & platform limits*

- Must be enabled by Salesforce; hard to remove.
- Adds assignment/transfer complexity.
- Largely superseded by modern patterns.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## WITH SECURITY_ENFORCED

*A SOQL clause that enforces field- and object-level security on the query.*

### 🌱 Simple

*Beginner - plain language*

`WITH SECURITY_ENFORCED` makes a SOQL query **respect the running user's field- and object-level permissions** — if they can't see a selected field/object, the query throws instead of returning data they shouldn't access.

### 📏 Limits

*Governor & platform limits*

- FLS/CRUD only — no record sharing.
- Throws (all-or-nothing), doesn't filter.
- Some complex-query edge cases.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## USER_MODE

*Running SOQL/DML in the user's security context — enforcing both FLS/CRUD and sharing.*

### 🌱 Simple

*Beginner - plain language*

`WITH USER_MODE` (and Apex `AccessLevel.USER_MODE`) runs a query or DML as the **current user** — enforcing field/object permissions *and* record-level sharing, instead of Apex's default system mode.

### 📏 Limits

*Governor & platform limits*

- Throws on permission/sharing violations.
- May hide records a system job needs.
- Newer feature — verify API version support.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## stripInaccessible

*An Apex method that removes fields/records a user can't access, instead of throwing.*

### 🌱 Simple

*Beginner - plain language*

`Security.stripInaccessible()` takes query results (or records to write) and **silently strips out fields the user can't access**, returning a safe, sanitized set — rather than throwing like `WITH SECURITY_ENFORCED`.

### 📏 Limits

*Governor & platform limits*

- FLS field-level only (no record sharing).
- Strips silently — log removed fields.
- Per access type (read/create/update/upsert).

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Database.query()

*Running SOQL built as a string at runtime — dynamic SOQL.*

### 🌱 Simple

*Beginner - plain language*

`Database.query(soqlString)` runs a query you **build as text at runtime** (dynamic SOQL), instead of a hardcoded inline query. Useful when fields/filters aren't known until execution.

### 📏 Limits

*Governor & platform limits*

- Same SOQL governor limits.
- Field/object names can't be bound (allow-list them).
- Runtime (not compile-time) validation.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Bind Variables

*Apex variables referenced in SOQL with a colon — safe, bulk-friendly parameterization.*

### 🌱 Simple

*Beginner - plain language*

**Bind variables** drop Apex values into SOQL with a colon: `WHERE Id IN :idSet`. They parameterize the query safely (no string concatenation) and are the basis of bulk-safe filtering.

### 📏 Limits

*Governor & platform limits*

- Values only — not field/object names.
- Large IN-binds lose selectivity.
- Evaluated at query time.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Dynamic Filters

*Conditionally assembling WHERE clauses at runtime for flexible queries.*

### 🌱 Simple

*Beginner - plain language*

**Dynamic filters** are `WHERE` conditions built **conditionally at runtime** — e.g., only add a status filter if the user picked one — so one query adapts to many input combinations.

### 📏 Limits

*Governor & platform limits*

- Values bind; identifiers/operators need allow-lists.
- Must guarantee selectivity on LDV.
- Complexity best centralized.

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
