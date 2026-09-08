# Salesforce Developer Notes

## 1. Transaction / Savepoint & Rollback

```apex
Database.Savepoint sp = Database.setSavepoint();
Database.rollback(sp);
Database.insert(records, false);
```

### Order of Execution (Save Order of Operations)

```
Before Flow
    ↓
Before Trigger
    ↓
Validation
    ↓
Save
    ↓
After Trigger
    ↓
After Flow
    ↓
Workflow
    ↓
Rollups
    ↓
COMMIT
    ↓
Post-Commit
```

---

## 2. Governor Limits — DML

**Important Difference**
- **DML Statement Limit** = number of DML *calls* (e.g., `insert`, `update` statements)
- **DML Row Limit** = number of *records* processed across all DML

---

## 3. Heap Size

### Common Causes of Heap Issues
- Huge query result
- Large String
- JSON
- Blob/Base64
- Huge collections

### Heap Kam Karne Ke Tarike
- Only required fields query karo (avoid `SELECT *` equivalent, don't over-fetch)
- SOQL for-loop use karo (instead of loading everything into a list)
- Large variables ko `null` karo jab kaam ho jaye
- SOQL aggregation use karo
- Large processing → Batch Apex mein todo

---

## 4. CPU Time Limit

**CPU time mein count hota hai:**
- Apex logic
- Loops
- Flow processing
- Formula evaluation
- Validation rules
- Other automation

**CPU time mein count NAHI hoti:**
- DB wait time
- Callout wait time

---

## 5. Integration — Key Considerations

Integration design karte waqt important:
- Timeout handling
- Retry logic
- Idempotency
- Async processing

---

## 6. REST API

**Why use REST?**
- Simple JSON
- Stateless
- Web/mobile integrations ke liye easy
- Real-time, low/medium volume ke liye best

---

## 7. SOAP API

**Why use SOAP?**
Jab external system already:
- SOAP use karta ho
- WSDL contract require karta ho
- Java/.NET enterprise middleware par based ho

**🔥 Developer Level — Two Important WSDL Types**
- **Enterprise WSDL** → org-specific, strongly typed
- **Partner WSDL** → generic, loosely typed

---

## 8. Metadata API vs Tooling API

| API | Purpose |
|---|---|
| **Metadata API** | Deployment (moving metadata between orgs) |
| **Tooling API** | Developer tooling / inspection / debugging |

**Tooling API se kya milta hai?**
- Apex classes/triggers access
- Run tests
- Code coverage data
- Debug logs
- Symbol tables
- Checkpoints/heap dumps
- Fine-grained metadata access

---

## 9. Composite API

Composite se single round-trip mein dependent operations kar sakte ho.

**🔥 Types**

**Composite**
- Up to 25 sub-requests
- `referenceId` chaining support

**Composite Batch**
- Independent requests
- No chaining between requests

**Composite Graph**
- Up to 500 records
- Powerful for related object trees

---

## 10. QA vs UAT

- **QA** = Built correctly? (technical correctness)
- **UAT** = Right thing built? (business validation)

---

## 11. SDLC Stages (Interview Cheat Sheet)

```
Development  → Build it
Testing      → Does it work?
UAT          → Is it what business wanted?
Production   → Release it safely
```

---

## 12. Good Unit Test — Checklist

A good unit test should be:
- Isolated
- Fast
- Deterministic
- Have meaningful assertions
- Bulk tested with 200+ records
- Cover negative/error scenarios
- Use `@testSetup`
- Mock all callouts

---

## 13. Integration Testing / SIT (System Integration Testing)

**Why it matters:**
Most real production problems occur at boundaries:
- Salesforce ↔ Middleware
- Salesforce ↔ External API
- Data mapping
- Authentication
- Error handling
- Sequencing

**🧠 Problem Identification Pattern**
> "Apex mock mein pass hai but real partner API ke saath fail ho raha hai"
→ This is an **Integration Testing / SIT** issue, not a unit test issue.

**🔥 What SIT Checks**
- End-to-end flow
- Request/response contract
- Authentication
- Data mapping
- Timeout handling
- 4xx/5xx error handling
- Retry logic
- Idempotency
- Async/eventual consistency

---

## 14. Testing Types Summary

| Testing Type | Main Question |
|---|---|
| **Unit** | Ek piece of code sahi hai? |
| **Integration** | Sab systems/components saath mein sahi hain? |
| **Regression** | New change se old functionality to nahi tooti? |
