[Home](../index.md) / [06 · SOQL & SOSL](index.md) / **Aggregate Queries**

# Aggregate Queries

9 topics · Series 6: SOQL & SOSL

**Topics on this page**

- [COUNT()](#count)
- [COUNT_DISTINCT()](#count-distinct)
- [SUM()](#sum)
- [AVG()](#avg)
- [MAX() MIN()](#max-min)
- [GROUP BY](#group-by)
- [HAVING](#having)
- [ROLLUP](#rollup)
- [CUBE](#cube)

## COUNT()

*Counting rows that match a query without retrieving them.*

### 🌱 Simple

*Beginner - plain language*

`COUNT()` returns **how many records** match — without pulling the records themselves. `SELECT COUNT() FROM Contact WHERE AccountId = :id` gives a number, cheaply.

### 📏 Limits

*Governor & platform limits*

- COUNT() returns an integer (no fields).
- Still needs selective WHERE on LDV.
- COUNT(field) ignores nulls.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## COUNT_DISTINCT()

*Counting the number of unique values of a field.*

### 🌱 Simple

*Beginner - plain language*

`COUNT_DISTINCT(field)` counts **unique values** — e.g., how many distinct Industries exist among your Accounts, ignoring duplicates.

### 📏 Limits

*Governor & platform limits*

- Returns an `AggregateResult`; grouped results cap at 2,000 rows.
- Cannot be combined with `FOR UPDATE`.
- The underlying scan still needs selectivity on large objects.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## SUM()

*Totaling a numeric field across matching records.*

### 🌱 Simple

*Beginner - plain language*

`SUM(field)` adds up a numeric field across records: `SELECT SUM(Amount) FROM Opportunity WHERE IsWon = true` gives total won revenue.

### 📏 Limits

*Governor & platform limits*

- Numeric/currency fields only; ignores nulls.
- Returned via AggregateResult.
- Multi-currency needs conversion.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## AVG()

*Computing the average of a numeric field across records.*

### 🌱 Simple

*Beginner - plain language*

`AVG(field)` returns the **average** of a numeric field: `SELECT AVG(Amount) FROM Opportunity` gives the mean deal size.

### 📏 Limits

*Governor & platform limits*

- Ignores nulls (base = non-null count).
- No median/percentile in SOQL.
- Multi-currency conversion needed.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## MAX() MIN()

*Finding the largest and smallest values of a field.*

### 🌱 Simple

*Beginner - plain language*

`MAX(field)` and `MIN(field)` return the **highest and lowest** values — e.g., the largest deal or the earliest created date.

### 📏 Limits

*Governor & platform limits*

- Return value, not the record.
- Work on numeric/date/text.
- LDV selectivity + multi-currency caveats.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## GROUP BY

*Bucketing records so aggregate functions compute per group.*

### 🌱 Simple

*Beginner - plain language*

`GROUP BY` groups records by a field so aggregates (SUM, COUNT, AVG…) are computed **per group**: `SELECT StageName, COUNT(Id) FROM Opportunity GROUP BY StageName`.

### 📏 Limits

*Governor & platform limits*

- Non-aggregated fields must be grouped.
- Returns AggregateResult[].
- WHERE selectivity still required.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## HAVING

*Filtering grouped results based on aggregate values.*

### 🌱 Simple

*Beginner - plain language*

`HAVING` filters **groups** after aggregation — like WHERE, but for aggregate results: `GROUP BY AccountId HAVING COUNT(Id) > 5` keeps only accounts with more than 5 records.

### 📏 Limits

*Governor & platform limits*

- HAVING runs post-aggregation (no index use).
- Requires GROUP BY.
- Aggregate conditions only.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## ROLLUP

*Adding subtotal and grand-total rows to grouped query results.*

### 🌱 Simple

*Beginner - plain language*

`ROLLUP` in a `GROUP BY` adds **subtotal and grand-total** rows to your aggregates — so you get per-group totals plus an overall total in one query.

### 📏 Limits

*Governor & platform limits*

- Capped number of grouping combinations.
- Subtotal rows have null grouped field (use GROUPING()).
- Requires aggregate query.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## CUBE

*Generating subtotals for every combination of grouped dimensions.*

### 🌱 Simple

*Beginner - plain language*

`CUBE` is like `ROLLUP` but produces subtotals for **every combination** of the grouped fields — full cross-tab totals in one query.

### 📏 Limits

*Governor & platform limits*

- Combinatorial — keep dimensions few.
- Grouping-combination cap.
- Use GROUPING() to read subtotals.

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
