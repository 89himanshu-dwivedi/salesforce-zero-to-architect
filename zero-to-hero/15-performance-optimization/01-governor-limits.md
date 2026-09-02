[Home](../index.md) / [15 · Performance Optimization](index.md) / **Governor Limits**

# Governor Limits

6 topics · Series 15: Performance Optimization

**Topics on this page**

- [SOQL Limits](#soql-limits)
- [DML Limits](#dml-limits)
- [CPU Time](#cpu-time)
- [Heap Size](#heap-size)
- [Concurrent Requests](#concurrent-requests)
- [Callout Limits](#callout-limits)

## SOQL Limits

*100 sync / 200 async queries; 50k rows per transaction.*

### 🌱 Simple

*Beginner - plain language*

**SOQL governor limits** cap how many queries and rows you can use per transaction: **100 queries** (synchronous), **200** (asynchronous), and **50,000 total rows** retrieved. Exceeding them throws an uncatchable `LimitException`.

### 📏 Limits

*Governor & platform limits*

- 100 queries sync / 200 async; 50,000 rows per transaction.
- Shared with flows, validation rules and managed packages.
- Batch `start()` QueryLocator is exempt up to 50 million rows.
- Custom Metadata and Custom Setting `getInstance()` cost nothing.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## DML Limits

*150 DML statements; 10,000 rows processed per transaction.*

### 🌱 Simple

*Beginner - plain language*

**DML governor limits** cap data changes per transaction: **150 DML statements** and **10,000 rows** processed total across insert/update/delete/upsert. Exceeding throws a `LimitException`.

### 📏 Limits

*Governor & platform limits*

- 150 statements and 10,000 rows per transaction.
- Cascade deletes and approval submissions count.
- Setup and non-setup objects cannot be mixed in one transaction.
- Partial-success DML does not raise the limits.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## CPU Time

*10s sync / 60s async CPU limit per transaction.*

### 🌱 Simple

*Beginner - plain language*

**CPU time limit** caps the compute time of a transaction: **10,000 ms (10s) synchronous** and **60,000 ms (60s) asynchronous**. It measures Apex execution time (not callout/DB wait).

### 📏 Limits

*Governor & platform limits*

- 10,000 ms sync / 60,000 ms async.
- Excludes database and callout wait time.
- Shared with all flows, workflow rules, roll-ups and validation rules.
- The exception is uncatchable - the transaction rolls back.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Heap Size

*6MB sync / 12MB async in-memory limit.*

### 🌱 Simple

*Beginner - plain language*

**Heap size limit** caps the memory a transaction holds at once: **6 MB synchronous**, **12 MB asynchronous**. Holding too many records/large strings in memory throws a `LimitException`.

### 📏 Limits

*Governor & platform limits*

- 6 MB sync / 12 MB async.
- SOQL for-loops stream 200 records at a time and are the standard fix.
- Base64 inflates binary data by ~33%.
- `Database.Stateful` state counts in every batch chunk.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Concurrent Requests

*10 long-running (>5s) synchronous requests per org.*

### 🌱 Simple

*Beginner - plain language*

The **concurrent request limit** allows only **10 synchronous Apex requests running longer than 5 seconds** at the same time per org. The 11th is rejected until a slot frees.

### 📏 Limits

*Governor & platform limits*

- 10 concurrent synchronous requests over 5 seconds, org-wide.
- 25 concurrent API requests over 20 seconds.
- Exceeding either affects all users, not just the caller.
- Continuations do not count while waiting.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Callout Limits

*100 callouts; 120s total callout time per transaction.*

### 🌱 Simple

*Beginner - plain language*

**Callout limits** cap external HTTP/web-service calls per transaction: **100 callouts** and **120 seconds** total callout time, with a per-callout timeout you can set (up to 120s).

### 📏 Limits

*Governor & platform limits*

- 100 callouts per transaction; 120s cumulative callout time.
- Maximum 120s per callout (10s default).
- 6 MB sync / 12 MB async body size.
- Blocked after uncommitted DML.

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
