# Apex Transaction Control — Savepoint, Rollback & Transaction Boundaries (English)

**Topics:** Savepoint · Rollback · Transaction Control

**Savepoint** = a checkpoint in the middle of a transaction.
**Rollback** = taking the database back to a checkpoint or to the whole-transaction state.
**Transaction Control** = deciding what stays one atomic unit, what goes async, and when irreversible side effects happen.

> **Golden rule:** The DB can roll back, but treating static state, consumed governor limits, completed callouts, and already-sent emails as "rolled back" is dangerous — they are NOT undone.

---

## 1. Savepoint

A rollback marker capturing database state mid-transaction.

### 🌱 Simple
A Savepoint marks a point in the transaction you can roll back to:
```apex
Savepoint sp = Database.setSavepoint();
...
Database.rollback(sp);
```
It undoes DML done after the savepoint.

### 📘 Advanced
`Database.setSavepoint()` returns a marker; `Database.rollback(sp)` restores the database to that point, discarding later DML (but not resetting non-database state like static variables or already-sent emails/callouts). Used for conditional rollback — try some DML, and if a later step fails, revert just that portion while keeping earlier work.

### Example Flow
```apex
Savepoint sp = Database.setSavepoint();

Account a = new Account(Name = 'Demo');
insert a;

// If a problem arises later:
Database.rollback(sp);
```

```
Start Transaction
      ↓
Create Savepoint
      ↓
Do DML
      ↓
Problem?
  ┌───┴────┐
 Yes      No
  ↓        ↓
Rollback  Continue
```
DML performed after the savepoint can be undone.

### Key Patterns

**1. Partial Rollback**
If the business requirement is: *"If Order A's risky operation fails, undo that step, but preserve the earlier valid work"* — a savepoint is useful. If the requirement is *"nothing partial should ever persist"* — uncaught-exception-based atomic rollback is simpler.

**2. Rollback Scope**
Design rollback as a **DB-only** undo:
```
Rollback
 ├── Database DML              → Reverted
 ├── Static variables          → NOT reverted
 ├── Consumed governor limits  → NOT refunded
 ├── Completed callouts        → NOT undone
 ├── Sent emails               → NOT undone
 └── Immediate side effects    → NOT automatically undone
```
So carefully order irreversible side effects to come *after* risky DML succeeds.

**3. Savepoint Cost**
Creating a savepoint blindly inside a loop is dangerous — each `setSavepoint()` counts as a DML statement, and there's a hard cap of 5 per transaction.

**4. Nested Savepoints**
```
SP1
 ↓
SP2
 ↓
SP3
```
Rolling back to SP1 invalidates SP2/SP3:
```
Rollback SP1
   ↓
SP2 / SP3 invalid
```

### 🐞 Errors & Gotchas
- Assuming statics/limits reset after rollback
- Savepoints in loops exhausting the DML limit
- Using invalidated Ids post-rollback

### 📏 Limits
- Maximum 5 savepoints per transaction
- Rolling back to a savepoint releases every savepoint created after it
- Savepoints do not roll back callouts, sent emails, or Platform Events published with Publish Immediately
- Each `setSavepoint()` counts as a DML statement against the 150-statement limit

### ✅ Best Option
Savepoints for selective partial rollback in services/batches; rely on uncaught-exception rollback for simple all-or-none.

**Why?** They enable controlled recovery while keeping the rest of the transaction's work intact.

---

## 2. Rollback

Reverting database changes to a savepoint or via an uncaught exception.

### 🌱 Simple
Rollback undoes database changes — either explicitly with `Database.rollback(savepoint)`, or automatically when an uncaught exception ends the transaction (all DML is reverted).

### 📘 Advanced
Apex transactions are atomic by default: if any unhandled exception escapes, all DML in the transaction rolls back. Explicit `Database.rollback(sp)` reverts to a savepoint for selective undo. As with savepoints, rollback reverts only database state — not statics, emails, callouts, or consumed limits.

### Key Patterns

**1. Whole-Transaction Atomicity**
An uncaught exception rolls back everything, which is often the desired safety net (no partial writes). Deliberately letting exceptions propagate is a valid all-or-none strategy.
```apex
insert account1;
insert account2;

// Unhandled exception
throw new CustomException('Something failed');
// Both account1 and account2 → rolled back
```

**2. Selective Rollback**
`Database.rollback(sp)` reverts only post-savepoint DML for partial recovery.

**3. Side Effects Persist**
Emails/callouts already executed, Platform Events published immediately, and static state are **NOT** undone. Guard irreversible side effects until after the risky DML succeeds (e.g., send an email only after success, or use Platform Events with publish-after-commit).

**4. Limits Not Refunded**
Rolled-back DML still counts against governor limits.

**5. Mixed DML / Async**
Enqueued async jobs may still run unless the transaction fails before commit — publish-after-commit semantics matter here.

**6. Test Isolation**
Test data rolls back automatically after each test.

### 🐞 Errors & Gotchas
- Sent emails/callouts not undone on rollback
- Assuming partial writes exist after an exception (there are none — it's all-or-none)
- Limits still counted post-rollback

### 📏 Limits
- `Database.rollback(sp)` reverts DML but not Id values already assigned to in-memory sObjects
- Cannot undo callouts, emails, or immediately-published Platform Events
- An uncaught exception rolls back the whole transaction automatically
- Governor limits consumed before the rollback are not restored

### ✅ Best Option
Rely on atomic uncaught-exception rollback for all-or-none; savepoints for selective undo; defer irreversible side effects until after risky DML succeeds.

**Why?** It guarantees no partial DB writes while preventing duplicate/irreversible side effects.

---

## 3. Transaction Control

Designing transaction boundaries, atomicity, and async hand-offs.

### 🌱 Simple
Transaction control is managing what counts as one atomic unit of work — where it commits, where it rolls back, and when work is deferred to a new transaction (async).

### 📘 Advanced
An Apex transaction spans a synchronous execution context (trigger + all its automation, or a controller action), sharing one set of governor limits and committing atomically at the end. Tools: savepoints/rollback for partial control, async (Future/Queueable/Batch) to start a new transaction with fresh limits, and the Unit of Work pattern to batch and order DML. Callouts can't follow uncommitted DML in the same transaction.

### Async Boundary
```
Sync Transaction
      ↓
enqueue Queueable
      ↓
Commit
      ↓
Queueable starts
      ↓
New Transaction + Fresh Limits
```
Doing a callout directly after uncommitted DML in the same transaction can give a "You have uncommitted work pending" error.

### Key Patterns

**1. Boundary Definition**
The synchronous entry point (trigger/controller/batch-execute), possibly using a Unit of Work that orders DML by dependency and dedupes.

**2. Atomicity Strategy**
Let exceptions propagate for all-or-none, or use savepoints for partial recovery — choose per business rule.

**3. Async = New Transaction**
Future/Queueable/Batch each run in a fresh transaction with new limits; use them to split large work and to do callout-after-DML (since you can't call out after uncommitted DML).

**4. Order of Execution**
Understand how triggers/flows/rollups cascade within the transaction to avoid recursion and unexpected re-entry (static guards).

**5. Publish-After-Commit**
Platform Events with `AFTER_COMMIT` fire only if the transaction commits — the safe way to trigger side effects:
```
DML
 ↓
Business validation
 ↓
Commit succeeds
 ↓
Publish/trigger downstream work
```
If the transaction rolls back, a publish-after-commit event never misleadingly signals downstream.

**6. Locking**
Keep transactions short to reduce lock contention:
```
Long transaction
     ↓
Locks held longer
     ↓
More contention
     ↓
Higher failure/retry risk
```

**7. Idempotency**
New transaction ≠ automatically "exactly once." Design async retry/re-entry to be idempotent.

### Situation → Approach

| Situation | Approach |
|---|---|
| All-or-none DB operation | Uncaught exception / normal atomic rollback |
| Only later risky step needs undoing | Savepoint |
| External side effect | Carefully defer/order |
| Large async workload | Async transaction boundary |
| Downstream notification only after commit | Publish-after-commit semantics |

### 🐞 Errors & Gotchas
- "You have uncommitted work pending" — callout after DML
- Recursion/re-entry across cascading automation
- Non-idempotent async failing on retry
- Duplicate business actions from retrying without idempotency (e.g., calling an external API twice on retry)

### 📏 Limits
- Apex has no explicit manual `COMMIT`; the transaction commits when the outermost context completes successfully
- Async boundaries (future, Queueable, Batch) create new transactions with fresh limits and separate commits
- Callouts are blocked once uncommitted DML exists
- Platform Events with Publish After Commit are the safe way to signal downstream systems

### ✅ Best Option
Unit of Work for ordered atomic commits, deliberate atomicity (exceptions vs savepoints), and async + publish-after-commit for callouts/side effects/scale.

**Why?** It keeps transactions consistent and short while handling limits, callouts, and side effects safely.

---

## 4. AI Engineering Connection

Apex transaction concepts directly map to AI/agentic system design:

1. **Agent Tool Execution** — treat a multi-step tool call sequence like a transaction: know what's reversible and what isn't.
2. **AI Workflow Compensation** — AI agents often execute multi-step actions; a failed later step may need a compensating action rather than a true "rollback" for already-executed external effects.
3. **RAG / AI Pipeline** — index/write operations that feed a pipeline should be ordered so irreversible steps (e.g., sending a result to a user) happen only after all data is safely committed.
4. **LLM Tool Calling** — a tool call that hits an external API is like a callout: it can't be "rolled back," so design idempotent retries.

### Practical Architecture Pattern
```
                    ┌────────────────────┐
                    │  Sync Transaction  │
                    └─────────┬──────────┘
                              │
                       Validate Input
                              │
                              ↓
                        Database DML
                              │
                    ┌─────────┴─────────┐
                    │                   │
                 Success              Failure
                    │                   │
                    ↓                   ↓
                  Commit             Rollback
                    │
                    ↓
             Publish After Commit
                    │
                    ↓
             Async / Queueable
                    │
                    ↓
              External API / AI
                    │
                    ↓
              Idempotent Result
```

---

## 5. Cheat Sheet

| Concept | Easy Meaning | Main Use |
|---|---|---|
| Savepoint | Checkpoint | Selective rollback |
| Rollback | Undo DB changes | Recovery |
| Uncaught Exception | Fail whole transaction | All-or-none |
| Async | New transaction | Scale/callouts |
| Queueable | Async worker | External integration |
| Publish After Commit | Signal after commit | Safe downstream event |
| Idempotency | Safely repeat the same request | Retries |
| Unit of Work | Ordered DML coordination | Complex services |

---

## 6. Final Summary

- Savepoint = checkpoint.
- Rollback = reverting DB state to a previous point.
- Unhandled exception = whole-transaction DB rollback.
- Rollback does NOT rewind static variables, consumed limits, sent emails, or completed callouts.
- Savepoints are limited; avoid unnecessary savepoints, especially in loops.
- A callout after uncommitted DML in the same transaction can cause an issue.
- Async execution provides a new transaction and fresh governor limits.
- Publish-after-commit is useful for triggering downstream side effects only after a successful commit.
- In high-scale architecture, short transactions, clear boundaries, locking awareness, and idempotency are critical.
- The same principle applies to AI/agent systems: DB rollback ≠ rollback of external-world side effects.

---

## 7. Examples — Code for Every Concept

### Savepoint — Selective Partial Rollback
```apex
Savepoint sp = Database.setSavepoint();

try {
    Account acc = new Account(Name = 'Acme Corp');
    insert acc; // earlier valid work

    Opportunity opp = new Opportunity(
        Name = 'Big Deal',
        AccountId = acc.Id,
        StageName = 'Prospecting',
        CloseDate = Date.today().addMonths(1)
    );
    insert opp;

    // A later risky step fails validation
    if (opp.Amount == null) {
        throw new OpportunityException('Amount is required for this stage');
    }
} catch (OpportunityException e) {
    Database.rollback(sp); // undo everything since the savepoint
    System.debug('Rolled back due to: ' + e.getMessage());
}
```

### Rollback — Whole-Transaction Atomicity via Uncaught Exception
```apex
public void createRelatedRecords() {
    insert new Account(Name = 'Account One');
    insert new Account(Name = 'Account Two');

    // No try/catch here — let it propagate
    throw new CustomException('Business rule failed');
    // Both inserts above roll back automatically
}
```

### Rollback Scope — What Is NOT Undone
```apex
Savepoint sp = Database.setSavepoint();
Boolean emailSent = false;

try {
    insert new Account(Name = 'Test Account');

    // BAD: sending email before we know the whole operation succeeded
    // Messaging.sendEmail(...); emailSent = true;

    throw new CustomException('Later step failed');
} catch (Exception e) {
    Database.rollback(sp); // Account insert is undone
    // But if the email had already been sent above, it is NOT undone
    System.debug('DB rolled back, but any already-sent email is not.');
}
```

### Transaction Control — Publish After Commit
```apex
public class OrderService {
    public static void submitOrder(Order__c order) {
        insert order;

        // Fire a Platform Event that only delivers after successful commit
        EventBus.publish(new Order_Submitted__e(OrderId__c = order.Id));
        // If wired as "Publish After Commit", subscribers only see this
        // if the surrounding transaction actually commits.
    }
}
```

### Transaction Control — Async Boundary for Callout After DML
```apex
public class PaymentService {
    public static void chargeCustomer(Id orderId) {
        Order__c order = [SELECT Id, Status__c FROM Order__c WHERE Id = :orderId];
        order.Status__c = 'Processing';
        update order; // uncommitted DML

        // Calling out directly here would throw:
        // "You have uncommitted work pending"

        // Instead, defer the callout to a new transaction:
        System.enqueueJob(new ChargeCustomerQueueable(orderId));
    }
}

public class ChargeCustomerQueueable implements Queueable, Database.AllowsCallouts {
    private Id orderId;
    public ChargeCustomerQueueable(Id orderId) { this.orderId = orderId; }

    public void execute(QueueableContext context) {
        // New transaction, fresh limits — callout is safe here
        HttpResponse res = new Http().send(new HttpRequest());
        // Design this idempotently in case the Queueable retries
    }
}
```
