# Apex DML — Complete Notes (English)

**Series 7: Apex Fundamentals — 9 Topics**
Insert · Update · Delete · Undelete · Upsert · Merge · Database.insert · Database.update · allOrNone

---

## 1. Insert

A plain `insert list;` follows **all-or-none** behavior. If one record in the list fails, the entire DML operation can roll back.

Insert can trigger Salesforce validation, before/after triggers, flows/other automation, rollups/related processing, and the commit sequence. So consider automation side-effects in your architecture.

### All-or-None Default
Strict behavior:
```apex
insert records;
```
When partial success is needed:
```apex
Database.insert(records, false);
```

### Errors & Gotchas
- **`Too many DML statements: 151`** — caused by insert-in-a-loop
- **`REQUIRED_FIELD_MISSING`** — a required field is missing
- Validation failure — with all-or-none insert, the whole list can roll back
- **`Too many DML rows: 10001`** — transaction row limit exceeded

### 📏 Limits
- **150 DML statements / transaction**
- **10,000 DML rows / transaction**
- With the plain DML keyword, one row failure can fail the whole operation
- Mixing setup and non-setup objects in the same transaction can cause a **Mixed DML** issue
- Triggers generally process records in **200-record chunks**

### ✅ Best Option
Insert the list in bulk, capture returned Ids, insert parents before children. Use **Batch Apex** for large volumes. Use `Database.insert(list, false)` only when partial success is genuinely needed.

**Why?** Keeps DML limits safe and makes parent-child creation clean and scalable.

---

## 2. Update

If parallel transactions are updating children, parent contention can cause `UNABLE_TO_LOCK_ROW`.

**Mitigation:**
- DML ordering
- Reducing parent contention
- Suitable batching
- `FOR UPDATE` where appropriate
- Retry strategy

### Stale Data / Lost Updates
Transaction A read an old value while Transaction B updated the same record in the meantime. If A updates with stale data, it can cause a lost-update type problem. In concurrency-sensitive workflows, re-querying/optimistic checks are useful.

### Errors & Gotchas
- `UNABLE_TO_LOCK_ROW` under concurrency
- Update-in-a-loop → "Too many DML statements"
- Recursive update triggers
- Stale data / lost updates
- Updating unnecessary fields

### 📏 Limits
- 150 DML statements / transaction
- 10,000 DML rows / transaction
- Updating a master-detail child can cause parent locking
- Formula, roll-up, auto-number, and system audit fields can't be directly updated
- Under concurrent lock contention, `UNABLE_TO_LOCK_ROW` can occur after approximately 10 seconds

### ✅ Best Option
Bulk update + only changed fields + recursion guard + lock-contention mitigation.

**Why?** Reduces unnecessary processing, trigger churn, and locking, and is safer under concurrency.

---

## 3. Delete

`delete` removes records and, in the normal soft-delete behavior, sends them to the Recycle Bin. Deleted records generally remain recoverable in the Recycle Bin for roughly **15 days**, subject to platform/storage conditions.

**Restore:**
```apex
undelete acct;
```

At delete time, the following can apply:
- `before delete` trigger
- `after delete` trigger
- Cascade behavior
- Dependency/restrict rules

When a master-detail parent is deleted, child records can cascade-delete.

### Recycle Bin vs Hard Delete
```
Normal:  delete → Recycle Bin
Hard-delete scenario: emptyRecycleBin / supported hard-delete mechanism
```
Hard-deleted data cannot be recovered.

### Delete Triggers
- `before delete` can validate/block deletion
- `after delete` is useful for cleanup/related processing

### Partial Success
```apex
Database.delete(records, false);
```
Good records can be deleted while failed records are reported.

### Errors & Gotchas
- `DELETE_FAILED` due to dependencies/restrict rules
- Unexpected cascade deletion
- Delete-in-a-loop → "Too many DML statements"
- Rollup/locking side effects

### 📏 Limits
- Recycle Bin retention roughly 15 days
- Recycle Bin capacity is 25× data storage
- Cascade-deleted children can contribute to the DML row count
- `Database.emptyRecycleBin` — 10,000 records per call
- Delete can fail due to a required lookup/dependency or an active approval

---

## 4. Undelete

`undelete` restores soft-deleted records from the Recycle Bin.
```apex
undelete acct;
```
Bulk:
```apex
undelete accounts;
```

### 📘 Advanced
Undelete is only possible while the record is still available in the Recycle Bin. The recovery window is generally roughly 15 days. Cascade-deleted children can also be restored.

Undelete automation provides `before undelete` / `after undelete` trigger contexts.

### `after undelete`
Useful after a restore for:
- Rebuilding external references
- Refreshing search/index
- Rebuilding derived data
- Integration resync

### Recovery Window
If the Recycle Bin is emptied/hard-deleted: ❌ undelete is impossible.

### Key Patterns
- **Idempotency** — design recovery logic to be repeat-safe
- **Permissions** — restore requires appropriate access

### 🐞 Errors & Gotchas
- Record hard-deleted → cannot undelete
- Recovery window has passed
- Missing `after undelete` logic → derived/external state can stay stale
- Ignoring cascade restoration

### 📏 Limits
- Recycle Bin availability window is roughly 15 days
- Hard-deleted records can't be undeleted
- Undelete triggers can fire
- Cascade-restored children can count toward DML row limits

### ✅ Best Option
Soft delete + `after undelete` recovery logic for recoverable data. Hard delete only for data that should genuinely never be restored.

---

## 5. Upsert

### 🌱 Simple
Upsert = Insert OR Update.
```
Record exists?
   ↓
YES → UPDATE
NO  → INSERT
```
```apex
upsert accounts;
```
By External Id:
```apex
upsert accounts External_Id__c;
```

Upsert matches a record by: record Id, or the specified External Id/Unique field.
- Match found → UPDATE
- No match → INSERT

This is ideal for integrations/data loads when you don't know whether the record already exists. A bulk upsert's result lets you identify created/updated status and errors.

### One DML Statement
Upsert counts as one DML statement, even though internally it may do insert or update work.

### Partial Success
```apex
Database.upsert(records, External_Id__c.getSObjectField(), false);
```

### Cross-Object Relationships
External-Id-based relationship mapping enables patterns for relating child records to a parent without querying for the parent Id.

### Errors & Gotchas
- `DUPLICATE_EXTERNAL_ID` when the match is ambiguous/non-unique
- Blindly putting mixed null/non-null External Id values through the same processing path can be problematic
- Wrong match field can create duplicate records
- Ignoring External Id uniqueness/design

### 📏 Limits
- The match field needs appropriate External Id/Unique configuration
- If one value matches multiple existing records, it errors
- Upsert counts as one DML statement, but rows still count against normal DML row limits
- A non-unique matching field is not a supported use case

### ✅ Best Option
Upsert on a unique External Id.

**Why?**
- Prevents duplicate-on-retry
- Idempotent integration
- Reduces query-then-branch logic
- Handles new + existing records in one operation

---

## 6. Merge

### 🌱 Simple
`merge` combines duplicate records.
```
Master + Duplicate + Duplicate
      ↓
One surviving Master
```
Supported objects: Account, Contact, Lead, Case.

**Maximum 3 total records per merge call:** 1 Master + 2 duplicates.

Merge:
- Retains the master record
- Removes duplicate records
- Can reparent related records to the master
- Works on supported object types

**Example:**
```
Account A → Master
Account B → Duplicate
Account C → Duplicate

Merge
 ↓
Account A survives
 ↓
B/C removed
 ↓
Supported related records → A
```
Field survivorship rules apply, so choose the master deliberately.

### Key Patterns

**1. Reparenting**
A major advantage of merge is that supported related records can automatically reparent under the master.

**2. Trigger Cascade / `Trigger.isMerge` + `MasterRecordId`**
Merge-aware logic can consider merge context and information like the deleted records' `MasterRecordId`.

**3. Maximum 3**
One call: 1 Master + 2 duplicates = 3. Use Batch Apex and small merge groups for large dedup jobs.

**4. Field Survivorship**
Choosing the most complete / most recent / most trusted record as the survivor is a practical strategy.

**5. Data Loss Risk**
Data that isn't automatically reparented can be lost when the duplicate is removed. Consolidate/capture important data pre-merge.

**6. Locking**
Merge can take locks on related records, so contention is possible in large parallel dedup operations.

### 🐞 Errors & Gotchas
- Unsupported object
- More than 3 records in one merge
- Incorrect processing if triggers aren't merge-aware
- Losing important non-reparented data
- Wrong master selection

### 📏 Limits
- Supported objects: Account, Contact, Lead, Case
- Maximum 3 total records per merge call
- Records must be the same object type
- Related record reparenting depends on relationship/platform rules
- Merge should not be considered reversible

### ✅ Best Option
Batch-based dedup process + carefully selected survivor + small merge groups + merge-aware triggers + pre-merge data consolidation.

**Why?** Relationships consolidate safely and large dedup jobs stay manageable.

---

## 7. Database.insert

### 🌱 Simple
`Database.insert()` does DML like a normal insert, but gives extra control.
```apex
Database.insert(records, false); // false = partial success allowed
```

### 📘 Advanced
`Database.*` methods (`insert`, `update`, `upsert`, `delete`, etc.) can be used with the `allOrNone` flag and suitable `DMLOptions`.
```apex
Database.SaveResult[] results = Database.insert(records, false);
```

### Key Patterns

**1. Index-Aligned Results**
`SaveResult[]` is aligned with the order of the input List:
```
records[0] ↔ results[0]
records[1] ↔ results[1]
```
So you can identify the exact failed record and capture its Id, error message, fields, and status code.

**2. DMLOptions**
Database methods can provide additional DML options: assignment rules, duplicate-rule behavior, email headers/options, and other supported DML controls.

**3. Governor Parity**
`Database.insert()` doesn't bypass governor limits.

**4. Keyword vs Database**
Use `insert records;` when strict all-or-none behavior is desired. Use `Database.insert(records, false);` when partial success/resilience is required.

**5. Structured Error Handling**
In enterprise bulk jobs it's useful to capture failures into custom error records, logs, Platform Events, or retry queues.

### 🐞 Errors & Gotchas
- Not inspecting `SaveResult` → silent failures
- Assuming rollback when using `allOrNone=false`
- Using the plain keyword when resilience was actually needed
- Not logging/retrying failed records

### 📏 Limits
- `Database.insert(list, false)` supports partial success
- `SaveResult[]` must be inspected manually
- The same 150 DML statements / 10,000 rows limits apply
- `DMLOptions` can control assignment, duplicate-rule, and email-related behavior

### ✅ Best Option
`Database.insert(list, false)` + structured result handling for resilient bulk/integration DML. The `insert` keyword for strict atomic DML.

**Trade-off:**
```
allOrNone=true  → Strong atomicity, one failure can fail the operation
allOrNone=false → Better resilience, error handling is the developer's responsibility
```

---

## 8. Database.update

### 🌱 Simple
`Database.update(records, false)` updates existing records with partial success.
```apex
Database.update(accounts, false);
```
Valid records can be updated while failed records are reported in `SaveResult[]`.

### 📘 Advanced
```apex
Database.SaveResult[] results = Database.update(accounts, false);
```
Useful for: Batch Apex, integrations, large bulk jobs, resilient processing.

### Key Patterns

**1. Partial Success in Batches**
If one record in a Batch scope is invalid, `allOrNone=false` avoids unnecessarily discarding the successful records.

**2. Lock-Aware Retry**
Handle transient failures like `UNABLE_TO_LOCK_ROW` with a:
```
Detect → Log → Retry
```
pattern.

**3. Index-Aligned Results**
```
scope[i] ↔ results[i]
```
identifies the exact failure.

**4. DMLOptions**
Useful for controlling supported duplicate-rule/DML behavior.

**5. Idempotency**
Design retry-safe updates so repeating the same update doesn't corrupt data.

**6. Monitoring**
Emit failures to logs/Platform Events/custom error records to improve operational visibility.

**7. Database vs Keyword**
```
update → strict transactional behavior
Database.update(..., false) → resilient, partial success
```

### 🐞 Errors & Gotchas
- Ignoring partial failures
- Not having retry logic for `UNABLE_TO_LOCK_ROW`
- Blindly retrying non-idempotent updates
- Ignoring `SaveResult`

### 📏 Limits
- `allOrNone=false` provides partial success
- Same 150 statements / 10,000 rows limits
- Partial success doesn't automatically undo already-successful rows
- Record locks still apply
- Parent grouping/ordering can reduce contention

### ✅ Best Option
`Database.update(list, false)` + per-record error capture + idempotent logic + lock retry.

**Why?** Improves throughput, resilience, and observability without forcing every record to succeed together.

---

## 9. allOrNone

### 🌱 Simple
`allOrNone` decides DML behavior:

**`true`** — all records succeed, or the entire DML operation follows fail/rollback behavior.

**`false`** — good records are saved and failed records are reported in the results.
```apex
Database.insert(records, false);
```

The main architectural trade-off: **data integrity/atomicity vs resilience**

### 🐞 Errors & Gotchas
- `false` + ignoring results → silent failures
- `false` on an atomic business unit → risk of an inconsistent partial state
- Assuming that after `false`, a later uncaught exception won't roll back the transaction
- Ignoring parent-child dependencies

### 📏 Limits
- `false` saves successful rows and reports failures
- `true` gives strict all-or-none semantics
- A later uncaught exception can still roll back the transaction
- Explicitly inspect/log failures
- `allOrNone` doesn't change the 150 DML statements / 10,000 rows limits

### ✅ Best Option

| Scenario | Choice |
|---|---|
| Atomic business unit | `allOrNone = true` |
| Independent bulk load | `allOrNone = false` + SaveResult handling + error logging + retry strategy |

**Why?** Transactional semantics should be chosen based on the actual business requirement.

---

## 10. Quick Reference Table

| Operation | Meaning | Key Point |
|---|---|---|
| **insert** | Create new record | Generated Id gets populated |
| **update** | Modify existing record | Valid Id required |
| **delete** | Remove/soft-delete record | Recycle Bin + cascade awareness |
| **undelete** | Restore deleted record | Requires Recycle Bin |
| **upsert** | Insert OR Update | External Id = integration/idempotency |
| **merge** | Combine duplicate records | Max 3 total per call |
| **Database.insert** | Controlled insert | Partial success + results |
| **Database.update** | Controlled update | Partial success + results |
| **allOrNone** | Atomicity decision | true = atomic, false = resilient |

---

## 11. 🧠 2-Minute Interview Revision

```
INSERT
→ Bulk List
→ Generated Ids
→ Parent first, children second
→ Batch for huge volume

UPDATE
→ Id required
→ Bulk
→ Minimal fields
→ Recursion guard
→ Lock handling

DELETE
→ Recycle Bin
→ Cascade awareness
→ Batch for large purge

UNDELETE
→ Restore from Recycle Bin
→ after undelete
→ Hard delete cannot restore

UPSERT
→ Insert OR Update
→ External Id
→ Idempotency
→ Safe retries

MERGE
→ Deduplicate
→ Survivor/master selection
→ Reparenting
→ Merge-aware triggers
→ Max 3

Database.*
→ More DML control
→ Partial success
→ SaveResult
→ DMLOptions

allOrNone
→ true = atomic
→ false = resilient
→ false means result handling is mandatory
```

---

## 12. Examples — Every DML Operation with Code

### Insert — Bulk + Parent-Child
```apex
List<Account> accountsToInsert = new List<Account>{
    new Account(Name = 'Acme Corp'),
    new Account(Name = 'Globex Inc')
};
insert accountsToInsert; // Ids get populated after insert

List<Contact> contactsToInsert = new List<Contact>();
for (Account a : accountsToInsert) {
    contactsToInsert.add(new Contact(LastName = 'Rep', AccountId = a.Id));
}
insert contactsToInsert; // children after parents
```

### Update — Bulk + Minimal Fields
```apex
List<Account> toUpdate = new List<Account>();
for (Account a : [SELECT Id, Rating FROM Account WHERE Rating = null LIMIT 200]) {
    a.Rating = 'Warm'; // only the changed field
    toUpdate.add(a);
}
update toUpdate;
```

### Delete + Undelete
```apex
List<Account> staleAccounts = [SELECT Id FROM Account WHERE LastActivityDate < LAST_N_DAYS:365];
delete staleAccounts; // goes to the Recycle Bin

// Deleted by mistake? Restore it:
undelete staleAccounts;
```

### Upsert — External Id
```apex
List<Account> externalAccounts = new List<Account>{
    new Account(External_Id__c = 'EXT-001', Name = 'Acme Corp'),
    new Account(External_Id__c = 'EXT-002', Name = 'Globex Inc')
};
upsert externalAccounts External_Id__c; // matched → update, else → insert
```

### Merge — Duplicate Accounts
```apex
Account master = [SELECT Id FROM Account WHERE Name = 'Acme Corp' LIMIT 1];
Account duplicate1 = [SELECT Id FROM Account WHERE Name = 'ACME Corp.' LIMIT 1];
Account duplicate2 = [SELECT Id FROM Account WHERE Name = 'Acme Co' LIMIT 1];

merge master duplicate1 duplicate2; // max 1 master + 2 duplicates
```

### Database.insert — Partial Success
```apex
List<Account> accounts = new List<Account>{
    new Account(Name = 'Valid Account'),
    new Account() // missing Name — required field error
};

Database.SaveResult[] results = Database.insert(accounts, false);

for (Integer i = 0; i < results.size(); i++) {
    if (!results[i].isSuccess()) {
        System.debug('Failed record index: ' + i);
        for (Database.Error err : results[i].getErrors()) {
            System.debug('Error: ' + err.getMessage());
        }
    }
}
```

### Database.update — Lock-Aware Retry Pattern
```apex
Database.SaveResult[] results = Database.update(accountsToUpdate, false);

List<Account> retryList = new List<Account>();
for (Integer i = 0; i < results.size(); i++) {
    if (!results[i].isSuccess()) {
        for (Database.Error err : results[i].getErrors()) {
            if (err.getStatusCode() == StatusCode.UNABLE_TO_LOCK_ROW) {
                retryList.add(accountsToUpdate[i]); // transient failure — worth retrying
            }
        }
    }
}
if (!retryList.isEmpty()) {
    Database.update(retryList, false); // simple retry example
}
```

### allOrNone — true vs false
```apex
// Atomic business unit — order + order items, all or nothing
try {
    insert order;      // allOrNone = true (default keyword behavior)
    insert orderItems;
} catch (DmlException e) {
    System.debug('Rolled back: ' + e.getMessage());
}

// Independent bulk load — some records can fail while the rest proceed
Database.SaveResult[] loadResults = Database.insert(bulkLeads, false);
for (Database.SaveResult sr : loadResults) {
    if (!sr.isSuccess()) {
        // log / add to retry queue
    }
}
```
