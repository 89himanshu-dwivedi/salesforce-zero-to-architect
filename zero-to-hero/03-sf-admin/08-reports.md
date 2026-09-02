[Home](../index.md) / [03 · SF Admin](index.md) / **Reports**

# Reports

10 topics · Series 3: SF Admin

**Topics on this page**

- [Standard Report Types](#standard-report-types)
- [Custom Report Types](#custom-report-types)
- [Tabular Format](#tabular-format)
- [Summary Format](#summary-format)
- [Matrix Format](#matrix-format)
- [Joined Format](#joined-format)
- [Bucket Fields](#bucket-fields)
- [Row Level Formula](#row-level-formula)
- [Cross Filters](#cross-filters)
- [Sub Filters](#sub-filters)

## Standard Report Types

*Pre-built object+related-object templates that define which fields/records a report can use.*

### 🌱 Simple

*Beginner - plain language*

A **report type** is the template that decides *which object(s) and fields* a report can include. **Standard report types** are the ones Salesforce provides automatically for every object and its common relationships (e.g., "Opportunities", "Accounts with Contacts").

### 📏 Limits

*Governor & platform limits*

- Cannot be edited or extended - fields and object combinations are fixed.
- Some object relationships have no standard report type at all.
- Limited to the relationships Salesforce chose to expose.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Custom Report Types

*Admin-defined templates controlling objects, joins (with/without), and available fields.*

### 🌱 Simple

*Beginner - plain language*

A **custom report type** is a template *you* build to control exactly which objects, relationships, and fields a report can use — including **"with or without"** (outer join) related records that standard types can't do.

### 📏 Limits

*Governor & platform limits*

- Maximum 4 objects per report type.
- Up to 1,000 selected fields per report type.
- Adding an object after creation is possible but removing one is not.
- Deployment requires the report type and all referenced fields.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Tabular Format

*Simple row-and-column list — like a spreadsheet; fast, but no grouping/charts.*

### 🌱 Simple

*Beginner - plain language*

**Tabular** is the simplest report format — just rows and columns, like a spreadsheet. Great for a straight list (e.g., "all my open cases") but it can't group data or show charts.

### 📏 Limits

*Governor & platform limits*

- No grouping, no charts and cannot be used for most dashboard component types.
- Limited to 2,000 displayed rows like every report format.
- Cannot be used as the source for a bucketed grouping.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Summary Format

*Groups rows by one or more fields with subtotals — the workhorse analytical format.*

### 🌱 Simple

*Beginner - plain language*

**Summary** reports group rows by a field (e.g., by Stage, by Owner) and show **subtotals** for each group. It's the go-to format for analyzing data and powering most charts/dashboards.

### 📏 Limits

*Governor & platform limits*

- Maximum 3 grouping levels.
- 2,000 rows displayed; 50,000 on formatted export.
- Row-level formulas are limited to 5 per report.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Matrix Format

*Two-dimensional pivot: group by rows AND columns with subtotals at intersections.*

### 🌱 Simple

*Beginner - plain language*

**Matrix** reports group by **both rows and columns** — like a pivot table. For example, rows = Product, columns = Month, cells = revenue. It's for comparing two dimensions at once.

### 📏 Limits

*Governor & platform limits*

- Maximum 2 row groupings and 2 column groupings.
- Wide matrices are slow to render and are truncated on export.
- Cannot be used for some dashboard component types.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Joined Format

*Multiple report blocks side-by-side (even different report types) sharing a common grouping.*

### 🌱 Simple

*Beginner - plain language*

**Joined** reports place several report **blocks side by side** — even from *different* report types — so you can compare related data in one view. E.g., Opportunities, Cases, and Activities for the same accounts together.

### 📏 Limits

*Governor & platform limits*

- Maximum 5 report blocks.
- Cannot be used as a dashboard source for all component types.
- Cross-block formulas are limited in what they can reference.
- Not supported in some export formats.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Bucket Fields

*Categorize records on the fly inside a report without creating a new field.*

### 🌱 Simple

*Beginner - plain language*

A **bucket field** groups values into categories *inside the report* — no new field or automation needed. E.g., bucket Amount into "Small / Medium / Large", or Stage into "Open / Won / Lost".

### 📏 Limits

*Governor & platform limits*

- Maximum 5 bucket fields per report, 20 buckets each with up to 20 values per bucket.
- Buckets are report-scoped and cannot be reused across reports.
- Bucketing is applied after row retrieval, so it does not improve query selectivity.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Row Level Formula

*A per-row calculated column in a report using fields from that row — no schema change.*

### 🌱 Simple

*Beginner - plain language*

A **row-level formula** adds a calculated column to a report that computes a value **for each row** from that row's fields — like "days open" = Today − Created Date — without creating a field on the object.

### 📏 Limits

*Governor & platform limits*

- Maximum 5 row-level formulas per report.
- Cannot reference other row-level formulas.
- Formula length and function support are more limited than field formulas.
- Evaluated per row, so they add render cost.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Cross Filters

*Filter records by the existence/absence of related records — with or without conditions.*

### 🌱 Simple

*Beginner - plain language*

A **cross filter** filters your report by whether records have (or don't have) **related records** meeting criteria — e.g., "Accounts *with* Opportunities" or "Accounts *without* open Cases" — without adding those related fields as columns.

### 📏 Limits

*Governor & platform limits*

- Maximum 3 cross filters per report, each with up to 5 sub-filters.
- Cross filters are non-selective and slow on large objects.
- Cannot be combined with all report formats.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Sub Filters

*Conditions applied to the related object within a cross filter — qualify which related records count.*

### 🌱 Simple

*Beginner - plain language*

A **subfilter** refines a **cross filter** by adding conditions on the *related* object. Instead of "Accounts with Opportunities", a subfilter makes it "Accounts with Opportunities **where Stage = Closed Won and Amount > 50k**".

### 📏 Limits

*Governor & platform limits*

- Apply to related object only; limited count per cross filter; AND logic; performance on large related sets.

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
