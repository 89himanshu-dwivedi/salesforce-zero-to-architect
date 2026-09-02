[Home](../index.md) / [07 · Apex Fundamentals](index.md) / **Exception Handling**

# Exception Handling

6 topics · Series 7: Apex Fundamentals

**Topics on this page**

- [DmlException](#dmlexception)
- [QueryException](#queryexception)
- [NullPointerException](#nullpointerexception)
- [LimitException](#limitexception)
- [Custom Exception](#custom-exception)
- [Try Catch Finally](#try-catch-finally)

## DmlException

*The error thrown when a DML operation fails.*

### 🌱 Simple

*Beginner - plain language*

A **DmlException** is thrown when `insert`/`update`/`delete` etc. fails — e.g., a validation rule, required field, or duplicate. Catch it to handle the failure gracefully.

### 📏 Limits

*Governor & platform limits*

- Exposes `getNumDml()`, `getDmlMessage(i)`, `getDmlId(i)` and `getDmlStatusCode(i)`.
- Catching it does not un-consume the DML statement from the limit.
- Some failures (validation rules, duplicate rules) surface here rather than as their own type.
- `UNABLE_TO_LOCK_ROW` arrives as a DmlException and is worth retrying; most others are not.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## QueryException

*The error thrown by SOQL assignment and query result issues.*

### 🌱 Simple

*Beginner - plain language*

A **QueryException** is thrown by query problems — most famously **"List has no rows for assignment to SObject"** when a single-record query (`Account a = [SELECT...]`) returns zero (or more than one) rows.

### 📏 Limits

*Governor & platform limits*

- Thrown when assigning a query to a single sObject and the result is 0 or more than 1 row.
- Also thrown for invalid dynamic SOQL built at runtime.
- The query still counts against the 100-query limit even when it throws.
- Query into a `List` and check `isEmpty()` to avoid it entirely.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## NullPointerException

*The error from dereferencing a null reference — Apex's most common bug.*

### 🌱 Simple

*Beginner - plain language*

A **NullPointerException (NPE)** happens when you access a member of a `null` value — e.g., `a.Name` when `a` is null, or `map.get(k).Field` when the key is missing.

### 📏 Limits

*Governor & platform limits*

- Fields omitted from a SELECT are null - there is no separate "not loaded" error.
- Safe navigation `?.` works on fields and method calls but not on list indexing.
- An uncaught NPE rolls back the entire transaction.
- In triggers it surfaces to users as an "unexpected exception" page, not a friendly message.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## LimitException

*The uncatchable error thrown when a governor limit is exceeded.*

### 🌱 Simple

*Beginner - plain language*

A **LimitException** is thrown when you exceed a governor limit (e.g., too many SOQL queries, DML rows, or CPU time). It generally **cannot be caught** and aborts the transaction.

### 📏 Limits

*Governor & platform limits*

- Cannot be caught in a way that lets the transaction continue - it always rolls back.
- Applies to CPU, heap, SOQL, DML, callouts and query rows.
- Only a Queueable `Finalizer` can reliably run after a limit exception.
- Use `Limits.getX()` to check headroom defensively before expensive operations.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Custom Exception

*User-defined exception types for domain-specific error handling.*

### 🌱 Simple

*Beginner - plain language*

A **custom exception** is your own exception class: `public class OrderException extends Exception {}`. Throw it (`throw new OrderException('msg');`) to signal domain-specific errors.

### 📏 Limits

*Governor & platform limits*

- Must extend `Exception` and the class name must end in `Exception`.
- Cannot be defined as an inner class of another Apex class in all contexts.
- Throwing a custom exception in a trigger shows the raw message to the user - use `addError()` instead.
- For LWC, wrap in `AuraHandledException` or the client sees a stack trace.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Try Catch Finally

*The structured error-handling block — guarded code, recovery, and cleanup.*

### 🌱 Simple

*Beginner - plain language*

`try { risky(); } catch (Exception e) { handle(e); } finally { cleanup(); }` runs guarded code, catches errors, and `finally` always runs (success or failure) for cleanup.

### 📏 Limits

*Governor & platform limits*

- `finally` always runs except on an uncatchable `LimitException` rollback.
- Catching `Exception` broadly hides governor failures and makes production errors invisible.
- A caught exception still leaves consumed limits consumed.
- DML performed before the exception is rolled back unless a `Savepoint` is used.

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
