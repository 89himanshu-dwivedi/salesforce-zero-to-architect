[Home](../index.md) / [22 · Limits & Alternate Solutions](index.md) / **Query & Pagination Limits**

# Query & Pagination Limits

12 topics · Series 22: Limits & Alternate Solutions

**Topics on this page**

- [OFFSET 2000 Limit](#offset-2000-limit)
- [Deep Pagination Beyond 2000](#deep-pagination-beyond-2000)
- [Non-Selective Query Abort](#non-selective-query-abort)
- [Relationship Query 5 Levels](#relationship-query-5-levels)
- [Subquery Rows Limit](#subquery-rows-limit)
- [Aggregate Rows Limit](#aggregate-rows-limit)
- [SOQL Query Timeout](#soql-query-timeout)
- [Report Row Limit](#report-row-limit)
- [List View Row Limit](#list-view-row-limit)
- [Anti-Join Semi-Join Limits](#anti-join-semi-join-limits)
- [Large Result Streaming](#large-result-streaming)
- [Selective Filter Design](#selective-filter-design)

## OFFSET 2000 Limit

*SOQL OFFSET maxes at 2,000 — use keyset (Id bookmark) paging.*

### 🌱 Simple

*Beginner - plain language*

SOQL `OFFSET` can skip at most **2,000 rows**. So `... LIMIT 50 OFFSET 2000` works, but `OFFSET 2001` throws `NUMBER_OUTSIDE_VALID_RANGE`. You *cannot* deep-paginate with OFFSET. The alternate is **keyset pagination**: remember the last record's Id and query `WHERE Id > :lastId`.

### 📏 Limits

*Governor & platform limits*

- OFFSET max 2,000.
- Offset cost grows linearly with depth.
- Requires a deterministic ORDER BY or rows repeat/skip.
- Not supported in some sub-query contexts.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Deep Pagination Beyond 2000

*Showing page 100 of huge data without OFFSET.*

### 🌱 Simple

*Beginner - plain language*

Any time users need to browse deep into a large result set, OFFSET won't take you past row 2,000. Beyond keyset paging, the real UX answer is usually **"don't deep-paginate" — let users filter/search to narrow results**, or load-on-scroll with bookmarks.

### 📏 Limits

*Governor & platform limits*

- OFFSET hard cap 2,000.
- Keyset paging cannot jump to an arbitrary page number.
- Sort key must be indexed for performance.
- Concurrent inserts can shift results between pages.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Non-Selective Query Abort

*Salesforce auto-kills queries that aren't selective on big objects.*

### 🌱 Simple

*Beginner - plain language*

On objects over ~**200,000 rows** (or in triggers), Salesforce *refuses* to run a query whose filter isn't **selective** (backed by an index and returning a small fraction of rows). The alternate isn't a bigger limit — it's **making the WHERE clause selective** via indexed fields or a custom index.

### 📏 Limits

*Governor & platform limits*

- Threshold 200,000 rows (100,000 in triggers).
- Custom indexes require a Support case.
- Indexes are skipped for leading wildcards, negations and nulls.
- Batch QueryLocator is exempt from the 50k row limit but still benefits from selectivity.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Relationship Query 5 Levels

*SOQL traverses 5 levels up (child→parent) / 1 level down per query.*

### 🌱 Simple

*Beginner - plain language*

SOQL can dot-walk **up to 5 levels of parent relationships** (e.g. `Contact.Account.Owner.Manager...`) and only **1 level of child** relationships (subqueries) per query. Need deeper? **Split into multiple queries** joined in Apex with Maps, or denormalize a needed value with a formula field.

### 📏 Limits

*Governor & platform limits*

- 5 levels child-to-parent; 1 level parent-to-child.
- Max 20 sub-queries per SOQL statement.
- Custom relationships use the `__r` suffix and count the same.
- Deep chains reduce selectivity and increase cost.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Subquery Rows Limit

*Parent-child subqueries return capped child rows — beware skew.*

### 🌱 Simple

*Beginner - plain language*

A parent query with a child subquery (`(SELECT ... FROM Contacts)`) counts every child row against your **50,000 query-rows** budget. One parent with 30,000 children can blow it. Alternate: **query children separately with a selective filter**, or process the parent in Batch.

### 📏 Limits

*Governor & platform limits*

- Sub-query rows count toward 50,000.
- Max 20 sub-queries per statement.
- Sub-queries cannot be nested further.
- Sub-query results are lazily paged and can surprise on heap.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Aggregate Rows Limit

*GROUP BY still processes rows — and AggregateResult caps.*

### 🌱 Simple

*Beginner - plain language*

Aggregate queries (`SUM`, `COUNT`, `GROUP BY`) are great for totals, but they still **scan rows** (counting toward the 50k budget) and the **number of grouped results** is itself limited (you can't return millions of groups). For huge aggregations, push to **CRM Analytics / Data Cloud / Reports**.

### 📏 Limits

*Governor & platform limits*

- 2,000 AggregateResult rows per query.
- Aggregate queries cannot use `FOR UPDATE`.
- `GROUP BY ROLLUP`/`CUBE` add extra rows toward the cap.
- Aggregates still count toward the 100-query limit.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## SOQL Query Timeout

*A single SOQL caps around 120s — selectivity is the cure.*

### 🌱 Simple

*Beginner - plain language*

One SOQL query can run for at most ~**120 seconds** before timing out. On big objects this almost always means the query is **non-selective** (table scan). The alternate is not a longer timeout — it's **indexed, selective filters**, and offloading analytics workloads.

### 📏 Limits

*Governor & platform limits*

- Query timeout around 120s.
- Reports and list views have separate timeouts.
- Custom indexes and skinny tables need a Support case.
- Batch QueryLocator queries can still time out in `start()`.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Report Row Limit

*Reports show ~2,000 rows in UI; exports and dashboards cap too.*

### 🌱 Simple

*Beginner - plain language*

Standard reports display up to **~2,000 rows** in the browser, dashboards summarize a limited row set, and exports have their own caps. If a user wants "all 500k rows," the alternate is **filter the report, summarize instead of list, or export via Data Loader / Bulk API / CRM Analytics**.

### 📏 Limits

*Governor & platform limits*

- 2,000 rows displayed; 50,000 formatted export.
- Report types span max 4 objects.
- Dashboards: 20 components; dynamic dashboards limited per edition.
- Reports have their own timeout.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## List View Row Limit

*List views display a capped page; filter, don't scroll forever.*

### 🌱 Simple

*Beginner - plain language*

List views load a page of records (e.g. up to ~2,000 visible / paged) and can be **slow when filters are non-selective** on big objects. The alternate is **sharper filters on indexed fields** and avoiding "All" views on multi-million-row objects.

### 📏 Limits

*Governor & platform limits*

- 2,000 records displayed.
- Subject to query selectivity rules.
- List view filters cannot use formula fields efficiently.
- Inline edit is limited on certain field types.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Anti-Join Semi-Join Limits

*IN/NOT IN subqueries are limited and can be non-selective.*

### 🌱 Simple

*Beginner - plain language*

Semi-joins (`WHERE Id IN (SELECT ...)`) and anti-joins (`NOT IN`) are powerful but limited: a query can have a bounded number of them, the inner query returns capped rows, and `NOT IN` is often **non-selective**. Alternate: **two queries joined with a Set/Map in Apex**.

### 📏 Limits

*Governor & platform limits*

- 2 semi-joins or anti-joins per query.
- Sub-query limited to 100,000 rows.
- Nested semi-joins are not allowed.
- Anti-joins cannot be made selective.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Large Result Streaming

*Process millions of rows without holding them — stream.*

### 🌱 Simple

*Beginner - plain language*

When you must touch a huge result set, never load it all. **Stream** it: a SOQL for-loop pulls 200 rows at a time, and Batch Apex's QueryLocator streams up to 50M. This keeps heap and row limits in check.

### 📏 Limits

*Governor & platform limits*

- QueryLocator max 50 million rows.
- SOQL for-loops chunk at 200 sObjects.
- Batch scope max 2,000, default 200.
- 5 concurrent batch jobs.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Selective Filter Design

*The master skill behind most query limits: be selective.*

### 🌱 Simple

*Beginner - plain language*

Most query limits (non-selective abort, timeout, row caps) trace back to one skill: **writing selective filters**. A selective query filters on an **indexed field** and returns a **small fraction** of rows. Master this and the limits mostly disappear.

### 📏 Limits

*Governor & platform limits*

- ~10% of first 1M rows, 5% thereafter, max 333,333.
- Custom and two-column indexes require Support.
- Nulls are not indexed unless the index is built to include them.
- Formula fields are generally not indexable.

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
