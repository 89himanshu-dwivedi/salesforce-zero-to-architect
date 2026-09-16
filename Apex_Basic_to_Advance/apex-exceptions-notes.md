# Apex Exceptions — Complete Notes (English)

**Topics:** DmlException · QueryException · NullPointerException · LimitException · Custom Exception · Try Catch Finally

---

## 1. DmlException

The error thrown when a DML operation fails.

### 🌱 Simple
A `DmlException` is thrown when insert/update/delete etc. fails — e.g., a validation rule, required field, or duplicate value. Catch it to handle the failure gracefully.

### 📘 Advanced
`DmlException` carries rich detail: `getDmlMessage(i)`, `getDmlFieldNames(i)`, `getDmlStatusCode(i)`, and `getNumDml()` for each failed row. It's thrown by all-or-none DML on any failure. To process partial failures without an exception, use `Database.*` with `allOrNone=false` and inspect results instead of catching.

### Key Patterns

**1. Catch vs Partial Success**
Use try/catch for atomic, all-or-none operations; use `Database.*(..., false)` + `SaveResult` inspection for independent/bulk records where some can fail without aborting the rest.

**2. Rich Diagnostics**
Pull `getDmlFieldNames()`/`getDmlStatusCode()` to build actionable messages and route by status code (e.g., `DUPLICATE_VALUE` vs `FIELD_CUSTOM_VALIDATION_EXCEPTION`).

**3. User-Facing**
In triggers, prefer `addError()` over throwing — it shows a clean message on the record rather than a raw stack trace.

**4. Don't Swallow**
Never catch `{}` silently — always at least log.

**5. Locking**
`UNABLE_TO_LOCK_ROW` surfaces as a `DmlException`; handle it with retry.

**6. Transaction Impact**
An uncaught `DmlException` rolls back the transaction.

### 🐞 Errors & Gotchas
- `FIELD_CUSTOM_VALIDATION_EXCEPTION` from validation rules
- `DUPLICATE_VALUE` on unique fields
- Swallowed `DmlException` hiding failures

### 📏 Limits
- Exposes `getNumDml()`, `getDmlMessage(i)`, `getDmlId(i)`, and `getDmlStatusCode(i)`
- Catching it does not un-consume the DML statement from the limit
- Some failures (validation rules, duplicate rules) surface here rather than as their own type
- `UNABLE_TO_LOCK_ROW` arrives as a `DmlException` and is worth retrying; most others are not

### ✅ Best Option
Catch + log + translate for atomic ops; partial-success results for independent records; `addError()` for trigger UX.

**Why?** It matches handling to whether failure is exceptional or expected, with precise messages.

---

## 2. QueryException

### 🌱 Simple
A `QueryException` is thrown by query problems — most famously "List has no rows for assignment to SObject" when a single-record query (`Account a = [SELECT...]`) returns zero (or more than one) rows.

### 📘 Advanced
Common causes: assigning a query to a single sObject when 0 or 2+ rows return; `Database.getQueryLocator` misuse; or non-selective queries on LDV timing out. Fix the single-row case by querying into a List and checking `isEmpty()`/`size()` before access, or guard with `LIMIT 1` plus a size check.

### Key Patterns

**1. Single-Row Assignment Risk**
Assigning `[SELECT ...]` directly to a single sObject variable throws when the result isn't exactly one row.

**2. Defensive Pattern**
Query into a `List<SObject>` and check `isEmpty()`/`size()` instead.

**3. Aggregate/Locator**
Improper use of query locators or aggregate results can throw; respect their result types.

**4. LDV Selectivity**
Non-selective queries on large objects can throw timeouts surfaced as query errors; ensure indexed filters.

**5. Bulk Context**
Never single-row query inside a loop — map results instead.

**6. Don't Catch to Mask**
Catching `QueryException` to hide a cardinality bug just delays the real fix.

### 🐞 Errors & Gotchas
- "List has no rows for assignment to SObject" (0 rows)
- Single-row query returning 2+ rows
- Non-selective LDV query timing out

### 📏 Limits
- Thrown when assigning a query to a single sObject and the result is 0 or more than 1 row
- Also thrown for invalid dynamic SOQL built at runtime
- The query still counts against the 100-query limit even when it throws
- Query into a List and check `isEmpty()` to avoid it entirely

### ✅ Best Option
Query into a List with explicit empty/multiple handling (or `LIMIT 1` + size check); fix cardinality assumptions rather than catching.

**Why?** It avoids both the `QueryException` and the follow-on NPE structurally.

---

## 3. NullPointerException

### 🌱 Simple
A `NullPointerException` (NPE) happens when you access a member of a null value — e.g., `a.Name` when `a` is null, or `map.get(k).Field` when the key is missing.

### 📘 Advanced
Apex variables default to `null`, and `Map.get(missingKey)`, unqueried relationships, and empty query results all yield `null`. Guard with null checks, the safe-navigation operator (`a?.Name`), `containsKey` before `get`, and initializing collections. NPEs are runtime errors that crash the transaction if uncaught.

### Key Patterns

**1. Safe Navigation**
`?.` collapses null-checks on a chained access.

**2. Map Access**
Always check `containsKey()` before `get()` when a missing key is possible, or handle the resulting `null`.

**3. Relationship/Field Nulls**
Parent lookups and optional fields can be `null` even when queried — never assume they're populated.

**4. Initialize Collections**
Instantiate Lists/Maps at declaration so iteration/add never NPEs.

**5. Fail-Fast vs Tolerate**
Validate required inputs early (guard clauses) and decide whether `null` is a bug (throw a clear message) or a valid "absent" state (handle it).

**6. Avoid Blanket `catch(NullPointerException)`**
It masks logic flaws — fix the null source instead.

**7. Defaults**
Ternary/coalesce patterns supply fallbacks.

### 🐞 Errors & Gotchas
- NPE from `map.get(missing).field`
- NPE on uninitialized List/Map
- NPE on null parent/optional fields

### 📏 Limits
- Fields omitted from a `SELECT` are `null` — there is no separate "not loaded" error
- Safe navigation `?.` works on fields and method calls but not on list indexing
- An uncaught NPE rolls back the entire transaction
- In triggers it surfaces to users as an "unexpected exception" page, not a friendly message

### ✅ Best Option
Prevent via safe navigation, initialized collections, `containsKey` checks, and fail-fast validation — not `catch(NullPointerException)`.

**Why?** NPEs signal a missing null assumption; structural prevention beats masking them.

---

## 4. LimitException

The uncatchable error thrown when a governor limit is exceeded.

### 🌱 Simple
A `LimitException` is thrown when you exceed a governor limit (e.g., too many SOQL queries, DML rows, or CPU time). It generally cannot be caught and aborts the transaction.

### 📘 Advanced
Unlike normal exceptions, `LimitException` is uncatchable in practice — you can't try/catch your way around hitting a limit. The fix is prevention: bulkify, monitor with `Limits` methods (`getQueries()`/`getLimitQueries()`), and proactively defer work to async before limits are reached. Common triggers: SOQL/DML in loops, CPU-time, heap, callouts.

### Key Patterns

**1. Uncatchable by Design**
You cannot rely on catching `LimitException` to recover — the only robust strategy is to not hit limits.

**2. Proactive Monitoring**
Compare `Limits.getQueries()` to `getLimitQueries()` (and DML/CPU/heap equivalents) and branch before the limit — e.g., enqueue a Queueable to continue remaining work.

**3. Bulkification**
The root cause is usually per-record SOQL/DML; fix with collection-based patterns.

**4. Async Chunking**
Batch Apex/Queueable reset limits per execution — route large workloads there.

**5. CPU-Time**
The sneaky one (no I/O) — optimize hot loops, avoid O(n²), reduce work.

**6. Defensive Design**
Selectors/handlers should respect limits as fixed constraints.

**7. Don't Confuse with Catchable Exceptions**
`LimitException` behaves very differently from `DmlException`/`QueryException`.

### 🐞 Errors & Gotchas
- "Too many SOQL queries/DML rows"
- "Apex CPU time limit exceeded"
- Trying to catch/retry a `LimitException`

### 📏 Limits
- Cannot be caught in a way that lets the transaction continue — it always rolls back
- Applies to CPU, heap, SOQL, DML, callouts, and query rows
- Only a Queueable Finalizer can reliably run after a limit exception
- Use `Limits.getX()` to check headroom defensively before expensive operations

### ✅ Best Option
Prevent via bulkification, proactive `Limits` monitoring, and async chunking — never rely on catching `LimitException`.

**Why?** Limits are uncatchable by design, so prevention is the only reliable strategy.

---

## 5. Custom Exception

User-defined exception types for domain-specific error handling.

### 🌱 Simple
A custom exception is your own exception class:
```apex
public class OrderException extends Exception {}
```
Throw it with `throw new OrderException('msg');` to signal domain-specific errors.

### 📘 Advanced
Apex custom exceptions must extend `Exception` and the class name must end in "Exception". They inherit constructors (message, cause). Use them to represent business-rule violations distinctly from system errors, enabling targeted `catch (OrderException e)` handling and clearer intent than generic exceptions. You can add fields/methods to carry context.

### Key Patterns

**1. Semantic Clarity**
`InsufficientInventoryException` communicates intent far better than a generic message, and lets callers catch specifically what they can handle while letting others propagate.

**2. Carry Context**
Add fields (recordId, errorCode, failed items) so handlers/logs have actionable detail without string-parsing.

**3. Exception Hierarchy**
A base `AppException` with subtypes enables catching broad or narrow as needed and consistent logging.

**4. Business vs System**
Throw custom exceptions for rule violations; don't use them for expected flow (prefer return values for non-exceptional cases — exceptions are costly and obscure logic if overused).

**5. Boundaries**
Translate to `AuraHandledException` at LWC boundaries (clean client messages) and to friendly responses at API boundaries; log the original.

**6. Testing**
Assert specific exception types in tests.

### 🐞 Errors & Gotchas
- Compile error if the name doesn't end in "Exception"
- Overusing exceptions for normal control flow
- Leaking raw exceptions to LWC/API clients

### 📏 Limits
- Must extend `Exception` and the class name must end in `Exception`
- Cannot be defined as an inner class of another Apex class in all contexts
- Throwing a custom exception in a trigger shows the raw message to the user — use `addError()` instead
- For LWC, wrap in `AuraHandledException` or the client sees a stack trace

### ✅ Best Option
A small domain exception hierarchy carrying context, thrown for genuine rule violations and translated to `AuraHandledException`/friendly responses at boundaries.

**Why?** It makes error handling precise and intent-revealing without overusing exceptions for normal flow.

---

## 6. Try Catch Finally

The structured error-handling block — guarded code, recovery, and cleanup.

### 🌱 Simple
```apex
try {
    risky();
} catch (Exception e) {
    handle(e);
} finally {
    cleanup();
}
```
Runs guarded code, catches errors, and `finally` always runs (success or failure) for cleanup.

### 📘 Advanced
You can have multiple typed catch blocks (most specific first: `catch (DmlException e)` then `catch (Exception e)`). `finally` always executes — even after a `return` — for cleanup/logging. Don't catch what you can't handle; rethrow or wrap. Note `LimitException` isn't reliably catchable, and an uncaught exception rolls back the transaction.

### Key Patterns

**1. Catch Specifically**
Order catch blocks specific → general; only catch exceptions you can meaningfully handle, and let others propagate (catching `Exception` broadly can hide bugs).

**2. Never Swallow**
Empty catch blocks are a top anti-pattern; at minimum log to a durable store; ideally translate and rethrow.

**3. Wrap & Enrich**
Catch low-level exceptions and rethrow a domain exception with context (`throw new OrderException('...', e);`) preserving the cause.

**4. `finally` for Cleanup**
Release locks, reset static flags, flush logs — runs even on return/throw.

**5. Savepoints**
In catch, roll back to a savepoint for logical recovery (partial transaction integrity).

**6. Boundaries**
Translate to `AuraHandledException` (LWC) / friendly API responses at the top, log technical detail below.

**7. Don't Guard Non-Exceptional Flow**
Use conditionals, not exceptions, for expected cases.

Architects place try/catch at meaningful boundaries (service/controller).

### 🐞 Errors & Gotchas
- Swallowed exceptions hiding failures
- Catching too broadly, masking bugs
- Assuming `LimitException` is catchable

### 📏 Limits
- `finally` always runs except on an uncatchable `LimitException` rollback
- Catching `Exception` broadly hides governor failures and makes production errors invisible
- A caught exception still leaves consumed limits consumed
- DML performed before the exception is rolled back unless a Savepoint is used

### ✅ Best Option
Specific catches at boundaries with logging, wrap/rethrow with context, `finally` for cleanup, and savepoints for recovery.

**Why?** It handles only what you can, preserves diagnostics, and keeps transactions consistent.

---

## 7. No-Skip Revision

**DmlException**
DML fails → `DmlException`. Detailed diagnostics: `getDmlMessage(i)`, `getDmlFieldNames(i)`, `getDmlStatusCode(i)`, `getNumDml()`. For independent records, use `Database.*(..., false)` + `SaveResult` pattern.

**QueryException**
Occurs when a query result/cardinality assumption is unsafe. Safe pattern: `List<SObject>` + `isEmpty()`/`size()`; if exactly one row is expected, use an appropriate `LIMIT 1`.

**NullPointerException**
Accessing a null object/reference → NPE. Prevention: `?.`, null checks, `containsKey()`, initialized collections, guard clauses.

**LimitException**
Exceeding a governor limit aborts the transaction. Don't rely on catching it — use bulkification, `Limits` monitoring, async processing, and chunking.

**Custom Exception**
For business/domain-specific errors. Extend `Exception` and end the class name in "Exception". Context fields and a meaningful hierarchy are useful.

**Try Catch Finally**
Catch specific exceptions, never swallow silently, log/translate, use `finally` for cleanup, and consider Savepoint/rollback in recoverable transaction scenarios.

---

## 8. 🏆 Golden Rules

1. Don't turn an expected business condition into exception-based control flow.
2. For partial DML failure, use `Database.*(..., false)` and inspect `SaveResult`.
3. Handle atomic DML failures at a meaningful boundary.
4. Explicitly design for query cardinality.
5. Prevent NPEs rather than catching them.
6. Treat governor limits as architectural constraints.
7. Bulkify + async/chunk large workloads.
8. Give custom exceptions meaningful domain semantics.
9. Don't silently swallow errors with empty catch blocks.
10. Translate technical errors into friendly/secure messages at user-facing boundaries.

---

## 9. Examples — Code for Every Exception Type

### DmlException — Rich Diagnostics
```apex
try {
    insert accounts; // all-or-none
} catch (DmlException e) {
    for (Integer i = 0; i < e.getNumDml(); i++) {
        System.debug('Message: ' + e.getDmlMessage(i));
        System.debug('Fields: ' + e.getDmlFieldNames(i));
        System.debug('Status: ' + e.getDmlStatusCode(i));
    }
}

// Partial success instead of catching:
Database.SaveResult[] results = Database.insert(accounts, false);
for (Database.SaveResult sr : results) {
    if (!sr.isSuccess()) {
        for (Database.Error err : sr.getErrors()) {
            System.debug(err.getStatusCode() + ': ' + err.getMessage());
        }
    }
}
```

### QueryException — Defensive Pattern
```apex
// Risky — throws if 0 or 2+ rows
// Account a = [SELECT Id FROM Account WHERE Name = 'Acme'];

// Safe pattern
List<Account> matches = [SELECT Id FROM Account WHERE Name = 'Acme' LIMIT 1];
if (matches.isEmpty()) {
    System.debug('No matching account found');
} else {
    Account a = matches[0];
}
```

### NullPointerException — Safe Navigation & Guards
```apex
Account acc = [SELECT Id, Owner.Name FROM Account LIMIT 1];
String ownerName = acc?.Owner?.Name; // returns null instead of throwing

Map<Id, Account> accountMap = new Map<Id, Account>(); // always initialize
if (accountMap.containsKey(someId)) {
    Account a = accountMap.get(someId);
}
```

### LimitException — Proactive Monitoring
```apex
if (Limits.getQueries() < Limits.getLimitQueries() - 5) {
    // safe headroom — proceed with another query
    List<Account> more = [SELECT Id FROM Account LIMIT 100];
} else {
    // near the limit — defer remaining work
    System.enqueueJob(new RemainingWorkQueueable());
}
```

### Custom Exception — Domain-Specific
```apex
public class InsufficientInventoryException extends Exception {
    public Id productId;
    public Integer requested;
    public Integer available;
}

public void reserveStock(Id productId, Integer qty, Integer available) {
    if (qty > available) {
        InsufficientInventoryException ex = new InsufficientInventoryException(
            'Not enough stock for product ' + productId
        );
        ex.productId = productId;
        ex.requested = qty;
        ex.available = available;
        throw ex;
    }
}

// Caller:
try {
    reserveStock(prodId, 10, 3);
} catch (InsufficientInventoryException e) {
    System.debug('Requested: ' + e.requested + ', Available: ' + e.available);
}
```

### Try Catch Finally — Specific Catches + Savepoint + Cleanup
```apex
Savepoint sp = Database.setSavepoint();
try {
    insert order;
    insert orderItems;
} catch (DmlException e) {
    Database.rollback(sp);
    System.debug('DML failed, rolled back: ' + e.getMessage());
    throw new OrderException('Could not place order', e); // wrap & rethrow with context
} catch (Exception e) {
    Database.rollback(sp);
    System.debug('Unexpected error: ' + e.getMessage());
    throw e;
} finally {
    System.debug('Cleanup: releasing recursion guard');
    isProcessing = false; // reset static flag, runs even after return/throw
}
```
