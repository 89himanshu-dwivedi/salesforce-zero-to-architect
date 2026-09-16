# Apex Architecture Classes — Utility, Service, Helper, Wrapper, DTO

## 1. Utility Classes

### 🌱 Simple
A utility class is a **common toolbox** where we keep reusable methods.

### Main Characteristics
- Methods are usually **static**
- The class doesn't maintain any object state
- Methods are reusable
- Ideally, methods should be **pure**
- Common examples: string manipulation, date calculation, collection manipulation, JSON formatting

> A utility class is defined as a collection of **stateless, reusable, static helper methods**.

### Why a `private constructor`?
We don't want anyone to be able to instantiate the utility class.

### What Should Go Into a Utility Class?

**Good:**
```
StringUtils
DateUtils
CollectionUtils
JsonUtils
```

**Bad:**
```
GodUtils
EverythingUtils
CommonUtils
```

### Key Patterns

**1. Utility Should Be Pure & Stateless**
Methods should transform input → output with no hidden state.

**2. Avoid Shared Mutable Static State**
Mutable static state can create race conditions and unpredictable behavior.

**3. Don't Put I/O Inside a Utility**
I/O means: SOQL, DML, Callouts.

| Layer | Job |
|---|---|
| Utility | transform / calculate / format |
| Selector | query |
| Service | business orchestration |

Maintaining this separation is important.

**4. Don't Build a "God Utility Class"**
Don't dump every unrelated helper method into one class.

### Static vs Instance
Not everything needs to be static. If the behavior needs to be:
- Mocked
- Dependency-injected
- Polymorphic

then:
```
Interface
   ↓
Instance implementation
```
can be better than a static method.

### Limits
- A static-only class holds no object state
- Static members can consume heap for the duration of the transaction
- The Apex heap limit is mentioned as 6 MB
- Utility methods are difficult to mock
- For behavior with dependencies, interface + instance is a better fit
- Apex code counts toward the org's 6 MB character limit

### Best Option
Small, cohesive, pure static utility classes with a private constructor.

But if:
- Mocking is required
- OR Polymorphism is required
- OR Dependencies are required

then: an interface + instance-based implementation is better.

**Reason:** Maximizes reusability and testability without hidden dependencies.

---

## 2. Service Classes

### 🌱 Simple
A Service Class is the central place for business logic.

The Service decides: *"What steps and rules should the business apply when submitting an order?"*

A Service:
- Handles business logic
- Can coordinate queries
- Coordinates calculations
- Coordinates DML
- Exposes a bulk-safe API

> A Service is defined as the business logic/use-case orchestration layer.

### Enterprise Architecture — Service Layer
```
Trigger
LWC Controller
Batch
REST API
      ↓
    SERVICE
      ↓
 ┌────┼──────┐
 ↓    ↓      ↓
Selector Domain UoW
```

| Layer | Job |
|---|---|
| Selector | Retrieves data |
| Domain | Handles record-related behavior |
| Unit of Work | Manages DML/commit |
| Service | Coordinates everything |

**Important:** The Service is an orchestrator, not a dumping ground.

### Key Patterns

**1. One Entry Point Per Use Case**
Every business use case should have a clear, single entry-point method.

**2. Bulk-First API**
Service methods should accept a List/collection, not a single record.

**3. Orchestration, Not Detail**
| Layer | Question It Answers |
|---|---|
| Service | "I need to submit the order" |
| Selector | "How do I fetch the data?" |
| Domain | "What is this record's behavior?" |
| UoW | "How do I commit the DML?" |

The Service coordinates everyone — it shouldn't implement the detail itself.

**4. Transaction Boundary**
A Service can define a logical transaction:
```
Start
 ↓
Validate
 ↓
Process
 ↓
Update records
 ↓
Commit
```
On failure, a rollback strategy can be used.

**5. Stateless & Injectable**
A Service shouldn't maintain unnecessary state across calls. Injecting dependencies through interfaces is useful for testing.

**6. Security Context**
In service architecture:
- Explicitly declare `sharing`
- Enforce CRUD/FLS
- Use the appropriate `USER_MODE`

Security shouldn't be accidentally ignored.

**7. Coarse-Grained Operations**
Service methods should expose large, meaningful business operations — not small fragmented calls.

### Limits
- Service methods should accept collections
- Salesforce governor limits apply to the complete transaction the service is called through
- The savepoint limit is 5 per transaction
- Exposing `@AuraEnabled` directly on the service is worse than keeping explicit security checks in the controller layer

### Best Option
A coarse-grained + bulk-safe + injectable Service class.

**Architecture:**
```
Service
 ├── Selector
 ├── Domain
 └── UnitOfWork
```

**Reason:** Centralized business rules, reusable, bulk-safe, clean layering.

---

## 3. Helper Classes

### 🌱 Simple
A Helper is a class that supports a particular feature/class.

Example:
```
AccountTriggerHandler
        ↓
AccountTriggerHelper
```

### Helper vs Service
| Helper | Service |
|---|---|
| Feature-specific | Reusable business use case |

### Key Patterns

**1. Feature-Scoped**
A Helper should stay focused around a specific feature/component. If the logic gets reused across multiple features, promote it:
```
Helper
  ↓
Service / Utility / Domain
```

**2. Thin Trigger Handler**
Ideal:
```
Trigger
 ↓
Handler
 ↓
Helper/Service
```
Don't turn the trigger/handler into an 800-line monster.

**3. Don't Turn the Helper Into a Dumping Ground**
Don't dump unrelated cross-feature logic into a Helper.

**4. Helper vs Domain**
In enterprise architecture, record-specific behavior is generally a better fit for a Domain class. A Helper can be a lightweight approach for smaller/simpler projects.

### 🐞 Errors & Gotchas
- A low-cohesion, catch-all Helper
- Business logic permanently stuck in the helper
- Fat trigger handlers
- A Helper's scope that keeps growing

### 📏 Limits
- Static mutable state can bleed data across records
- Static state doesn't persist beyond the transaction
- Don't depend on static state across Batch chunks
- Helper sprawl can make debugging/call graphs complex
- Don't make hot paths unnecessarily deep

### Best Option
A thin Helper for feature-local support. If the logic naturally fits:
- Reusable business logic → Service
- Generic pure logic → Utility
- Record behavior → Domain

then move it there.

**Reason:** The Helper stays focused and doesn't become a "Helper dumping ground."

---

## 4. Wrapper Classes

### 🌱 Simple
A Wrapper simply means: packing related data into a custom object.

Example:
```
Account + selected = true
```
The standard Account object doesn't have a `selected` field.

Build a wrapper:
```apex
public class RowVM {
    public Account record;
    public Boolean selected;
}
```

A Wrapper is useful when data:
- Doesn't fit into a single sObject
- Needs to travel alongside UI state
- Needs to aggregate multiple objects
- Needs a custom response shape for an LWC

### Key Patterns

**1. UI View Model**
A Wrapper's strong use case:
```
Database Model
      ↓
   Wrapper
      ↓
     UI
```

**2. `@AuraEnabled`**
When using a wrapper with an LWC, `@AuraEnabled` on the exposed properties is important.

**3. Aggregation**
A Wrapper can combine multiple related objects/values into one shape (e.g., summary card data).

**4. Comparable**
If a custom sort order is needed, the wrapper can implement `Comparable`.

**5. Wrapper vs DTO**
| Wrapper | DTO |
|---|---|
| Often: data + UI state/behavior | Usually: pure data transfer |

### Limits
- Visualforce wrappers contribute to view state
- The 170 KB view-state figure applies, unless marked `transient`
- Large wrapper lists can create heap problems
- Sync/async heap figures are 6 MB / 12 MB
- `equals()` and `hashCode()` may need to be overridden for Set/Map membership
- LWC-exposed members must have `@AuraEnabled`

### ✅ Best Option
Use a Wrapper as a serializable View Model/Aggregator at the UI/API boundary.

Use it for:
```
Record + UI state + Aggregated information
```
But don't put business/domain logic inside the wrapper.

**Reason:** Keeps UI-specific concerns separate from the domain model.

---

## 5. DTO Classes

### 🌱 Simple
DTO = Data Transfer Object. Remember it by the name: a packet for transferring data.

Example — from an external API:
```json
{
  "paymentId": "123",
  "amount": 500,
  "status": "SUCCESS"
}
```

DTO:
```apex
public class PaymentDTO {
    public String paymentId;
    public Decimal amount;
    public String status;
}
```

### Advanced
A DTO generally represents the request/response shape of an external API.

**Main benefit:** Don't let an external API's schema leak directly into your internal Salesforce model.

### Key Patterns

**1. Anti-Corruption Layer**
This is the single most important architect concept for a DTO.
```
External system: Third Party Schema
Internal system: Your Domain Model
Between them:    DTO + Mapping
```
If the external API changes:
```
customer_name
      ↓
client_name
```
Then ideally only the DTO/mapping changes — protecting the core domain from external schema changes.

**2. Typed Deserialization**
Better:
```apex
JSON.deserialize(body, PaymentDTO.class);
```
rather than everything as `Map<String, Object>`. A typed DTO gives structured/typed access.

**3. Field-Name Mapping**
If external field names don't match your internal naming convention, use a mapping layer.

**4. Versioning**
When the API version changes, create a new DTO (e.g., `PaymentDTOv2`) and deprecate the old one.

**5. No Business Logic in a DTO**
A DTO should only hold data — calculation/validation/business rules belong in the Service/Domain.

**6. Outbound DTO**
You can also build a separate DTO for outgoing requests, so request/response shapes can evolve independently.

### Limits
- Large JSON serialization/deserialization can be CPU-intensive
- Callout request/response bodies have a 6 MB sync / 12 MB async limit
- `JSON.deserializeStrict` can throw an exception on unknown fields
- Reserved Apex words can cause issues as field names

### Important Practical Point
If a vendor adds a new field to the JSON tomorrow and you're using `JSON.deserializeStrict(...)`, it can fail because of the unknown field. So use strict deserialization consciously.

### Best Option
Typed DTO + Anti-Corruption Boundary + Mapping Layer.

**Architecture:**
```
External API
     ↓
 Typed DTO
     ↓
 Mapping Layer
     ↓
Stable Domain Model
     ↓
 Service
```

**Reason:** Isolated external schema, type safety, easier testing, API versioning, domain decoupling.

---

## 6. One-Glance Summary Table

| Class | Simple Meaning | Main Job |
|---|---|---|
| **Utility** | Toolbox 🧰 | Common pure reusable functions |
| **Service** | Business Manager 👨‍💼 | Business use case/orchestration |
| **Helper** | Personal Assistant 🤝 | Supporting logic for a specific feature |
| **Wrapper** | Data Packet 📦 | Data + UI state/aggregation |
| **DTO** | Courier 🚚 | Transferring data across system/layer boundaries |
