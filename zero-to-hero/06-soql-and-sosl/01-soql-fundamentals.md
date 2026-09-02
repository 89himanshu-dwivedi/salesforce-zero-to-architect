[Home](../index.md) / [06 · SOQL & SOSL](index.md) / **SOQL Fundamentals**

# SOQL Fundamentals

8 topics · Series 6: SOQL & SOSL

**Topics on this page**

- [SELECT FROM WHERE](#select-from-where)
- [ORDER BY LIMIT OFFSET](#order-by-limit-offset)
- [NULLS FIRST / LAST](#nulls-first-last)
- [= != < > <= >=](#item)
- [LIKE IN NOT IN](#like-in-not-in)
- [INCLUDES EXCLUDES](#includes-excludes)
- [AND OR NOT](#and-or-not)
- [Date Literals (TODAY, LAST_WEEK, etc.)](#date-literals-today-last-week-etc)

## SELECT FROM WHERE

*The core of every SOQL query — choose fields, an object, and filter rows.*

### 🌱 Simple

*Beginner - plain language*

**SOQL** (Salesforce Object Query Language) reads records. The basic shape is `SELECT fields FROM Object WHERE condition` — pick which columns, from which object, matching which filter.

### 📏 Limits

*Governor & platform limits*

- No `SELECT *`; one object in FROM.
- 100/200 queries, 50k rows per transaction.
- WHERE should hit indexes on large objects.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## ORDER BY LIMIT OFFSET

*Sorting results and controlling how many rows (and from where) are returned.*

### 🌱 Simple

*Beginner - plain language*

`ORDER BY` sorts results, `LIMIT` caps how many rows return, and `OFFSET` skips a number of rows (for paging). Example: `ORDER BY Name ASC LIMIT 50 OFFSET 100`.

### 📏 Limits

*Governor & platform limits*

- OFFSET max 2,000.
- ORDER BY on unindexed fields is costly at scale.
- LIMIT/ORDER BY still subject to row/timeouts.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## NULLS FIRST / LAST

*Controlling where null-valued records appear in a sorted result.*

### 🌱 Simple

*Beginner - plain language*

`NULLS FIRST` and `NULLS LAST` decide whether records with a **blank value** in the sort field appear at the start or end of the results.

### 📏 Limits

*Governor & platform limits*

- Only valid with an explicit `ORDER BY`.
- Sorting on an unindexed field forces a full sort of the result set.
- Default is NULLS FIRST for ascending and NULLS LAST for descending.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## = != < > <= >=

*The comparison operators that build SOQL filter conditions.*

### 🌱 Simple

*Beginner - plain language*

These are the **comparison operators** in a `WHERE` clause: equals (`=`), not equals (`!=`), and the ordering operators (`<`, `>`, `<=`, `>=`) for numbers, dates, and currencies.

### 📏 Limits

*Governor & platform limits*

- Ordering operators need ordered types.
- Negation is generally non-selective.
- Selectivity depends on indexes + data distribution.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## LIKE IN NOT IN

*Pattern matching and set membership operators for flexible filtering.*

### 🌱 Simple

*Beginner - plain language*

`LIKE` matches text patterns with wildcards (`%` = any chars, `_` = one char). `IN` matches any value in a list; `NOT IN` excludes them. Example: `WHERE Name LIKE 'Acme%'`.

### 📏 Limits

*Governor & platform limits*

- Leading wildcards skip indexes.
- NOT IN is non-selective.
- Very large IN-lists lose selectivity.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## INCLUDES EXCLUDES

*Operators specifically for filtering multi-select picklist fields.*

### 🌱 Simple

*Beginner - plain language*

`INCLUDES` and `EXCLUDES` filter **multi-select picklists** — fields that can hold several values at once. `INCLUDES ('A;B')` matches records containing those selections.

### 📏 Limits

*Governor & platform limits*

- Only for multi-select picklists.
- Non-indexed/non-selective.
- Value-count and reporting limitations.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## AND OR NOT

*Logical operators to combine multiple filter conditions.*

### 🌱 Simple

*Beginner - plain language*

`AND`, `OR`, and `NOT` combine conditions in a `WHERE` clause. Use parentheses to group: `WHERE (A AND B) OR C`.

### 📏 Limits

*Governor & platform limits*

- OR can defeat index usage.
- NOT is non-selective.
- Precedence requires explicit grouping.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Date Literals (TODAY, LAST_WEEK, etc.)

*Built-in relative date keywords for dynamic, timezone-aware date filtering.*

### 🌱 Simple

*Beginner - plain language*

**Date literals** are keywords like `TODAY`, `YESTERDAY`, `THIS_WEEK`, `LAST_N_DAYS:30` that filter by date **without hardcoding** values. Example: `WHERE CloseDate = THIS_MONTH`.

### 📏 Limits

*Governor & platform limits*

- Fixed set of literals (+ parameterized N variants).
- Selectivity depends on date-field indexing.
- Respect org timezone/fiscal config.

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
