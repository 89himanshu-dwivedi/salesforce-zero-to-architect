# Salesforce HLD Interview Master Guide
## 60–70+ LPA Senior / Lead / Architect-Level Preparation

> **Core formula:**  
> **Salesforce HLD = Business Requirements + Scale + Salesforce Limits + Distributed Systems + Integration + Failure Handling + Security + Trade-offs**

---

# 1. How to Approach Any Salesforce HLD Question

Never start by drawing boxes immediately.

Use this order:

```text
1. Requirements
      ↓
2. Functional + Non-Functional Requirements
      ↓
3. Scale Estimation
      ↓
4. Identify System of Record
      ↓
5. High-Level Architecture
      ↓
6. Salesforce Components
      ↓
7. Integration Architecture
      ↓
8. Data Model
      ↓
9. Sync vs Async
      ↓
10. Security
      ↓
11. Failure Handling
      ↓
12. Limits & Scalability
      ↓
13. Observability
      ↓
14. Trade-offs
      ↓
15. Deep Dive
```

---

# 2. Ask Questions Before Designing

## What the interviewer expects

Before selecting Salesforce objects, Apex, Platform Events, or APIs, clarify the problem.

### Business questions

- What is the business use case?
- Who are the users?
- What is the expected user journey?
- What is the system of record?
- Which operations are business-critical?
- What data must be immediately consistent?

### Scale questions

- Number of users?
- Daily transactions?
- Peak transactions per second?
- Number of records?
- Data growth per year?
- Read-heavy or write-heavy?
- Peak traffic pattern?

### Integration questions

- Which external systems exist?
- ERP?
- Payment?
- Data warehouse?
- Marketing platform?
- Identity provider?
- Is integration synchronous or asynchronous?
- What are the external system SLAs?

### Reliability questions

- What happens if Salesforce is unavailable?
- What happens if an external system is unavailable?
- Can requests be retried?
- Can duplicate requests occur?
- Is data loss acceptable?
- Is eventual consistency acceptable?

### Salesforce-specific questions

- Standard objects or custom objects?
- Large Data Volume?
- Sharing/security requirements?
- API volume?
- Governor-limit constraints?
- Platform Events / CDC required?
- Batch processing required?
- Experience Cloud / external users?
- Data retention requirements?

### Senior-level signal

Don't ask questions just to fill time.

Ask questions that **change your architecture**.

---

# 3. Quick Math / Capacity Estimation

Exact numbers are less important than showing your reasoning.

## Example

Suppose:

- 2 million orders/day
- 86,400 seconds/day

Average order rate:

```text
2,000,000 / 86,400 ≈ 23 orders/sec
```

Assume a 10x peak:

```text
23 × 10 ≈ 230 orders/sec
```

Now say:

> "I would not design for the average rate. I'd consider the peak rate and expected growth, then decide whether Salesforce can handle the workload directly or whether asynchronous ingestion, middleware, or an external system is required."

## Estimate these

- Daily active users
- Requests/day
- Requests/sec
- Peak requests/sec
- Records created/day
- Storage growth/year
- Payload size
- Network bandwidth
- Event volume
- Queue depth
- Processing time

## Salesforce angle

Always connect the math to:

- Governor limits
- API limits
- Event volume
- Concurrent processing
- Data storage
- Large Data Volume
- Query selectivity
- Batch/async capacity

---

# 4. Database Trade-offs

A common mistake is:

> "Everything should be stored in Salesforce."

That is not an architect-level answer.

## Think in terms of data ownership

```text
                 ┌─────────────────┐
                 │    Salesforce   │
                 │ CRM / Workflow  │
                 └────────┬────────┘
                          │
          ┌───────────────┼────────────────┐
          ↓               ↓                ↓
       ERP/OMS        Payment System   Data Platform
```

## Salesforce is a strong choice for

- CRM data
- Customer/business relationships
- Workflow
- Sales/service processes
- Salesforce-native security
- Transactional business operations

## External database may be better for

- Very high-volume transactional workloads
- Specialized workloads
- Systems already owned by another domain
- Large analytical datasets
- Workloads requiring a specialized database model

## Analytics

Don't use Salesforce as your analytical warehouse by default.

Consider:

```text
Salesforce
    ↓
CDC / Events / ETL
    ↓
Data Platform / Warehouse
    ↓
Analytics
```

---

# 5. Sharding / Partitioning

Traditional database sharding is usually not something you implement directly inside Salesforce's underlying database.

Salesforce abstracts its database infrastructure.

So if asked:

> "How would you shard Salesforce?"

A strong answer is:

> "I wouldn't implement traditional database sharding directly because Salesforce abstracts the underlying database. For large-scale data, I'd focus on selective queries, indexing strategy, data archival, Big Objects, External Objects, asynchronous processing, data ownership, and potentially moving high-volume workloads to an external platform."

## Related Salesforce concepts

- Large Data Volume
- Selective SOQL
- Indexes
- Data archival
- Big Objects
- External Objects
- Async processing
- Data ownership

## Watch for hot data

Even without traditional sharding, ask:

- Is one account/customer receiving disproportionate traffic?
- Is one queue or partition becoming overloaded?
- Is one integration producing a burst?
- Are retries creating a retry storm?

---

# 6. Caching

Caching is useful for frequently accessed data that doesn't change every second.

## Salesforce example

```text
LWC
 ↓
Apex
 ↓
Platform Cache
 ↓ cache miss
Salesforce Database
```

## Important questions

Never say only:

> "We'll use cache."

Also explain:

- What is cached?
- TTL?
- Cache invalidation?
- What happens when cache is unavailable?
- How stale can the data be?
- Is stale data acceptable?

## Good candidates for caching

- Configuration
- Reference data
- Frequently accessed non-transactional data
- Expensive-to-compute results

## Avoid careless caching for

- Payment status
- Critical inventory
- Security-sensitive rapidly changing state
- Data where stale values can cause business errors

### Key principle

> **Caching is a consistency trade-off, not just a performance feature.**

---

# 7. Load Balancing

For Salesforce itself, infrastructure-level load balancing is largely abstracted by the platform.

Don't design Salesforce like a fleet of EC2 servers unless the question specifically includes external services.

## Better Salesforce architecture discussion

```text
External Clients
       ↓
API Gateway / Middleware
       ↓
Rate Limiter / Router
       ↓
Salesforce
```

For external services, discuss:

- Load balancing
- Routing
- Health checks
- Failover
- Rate limiting
- Connection management
- Circuit breakers

## Consistent hashing

Useful when you need related traffic/data routed consistently to a particular backend or cache node.

---

# 8. CAP Theorem

Don't simply recite:

> "CAP means Consistency, Availability, Partition tolerance."

Show where the trade-off appears.

## Salesforce + ERP example

```text
Salesforce ←→ Middleware ←→ ERP
```

Suppose ERP is down.

### Synchronous approach

```text
User
 ↓
Salesforce
 ↓
ERP
 ↓
Response
```

Advantages:

- Immediate downstream confirmation
- Easier to know whether ERP accepted the request

Disadvantages:

- ERP failure affects user experience
- Higher latency
- Tight coupling

### Asynchronous approach

```text
Salesforce
    ↓
Platform Event
    ↓
Middleware
    ↓
ERP
```

Advantages:

- Loose coupling
- Better resilience
- Faster user response
- Natural buffering

Disadvantages:

- Eventual consistency
- More complex retry/replay handling
- Harder user-facing status management

### Interview answer

> "For user-facing operations requiring immediate confirmation, I'd consider synchronous integration. For downstream propagation such as analytics, notifications, or non-critical synchronization, I'd prefer asynchronous processing where eventual consistency is acceptable."

---

# 9. Queues and Asynchronous Processing

Use asynchronous processing when work does not need to block the user's request.

## Salesforce tools

### Apex

- Queueable Apex
- Batch Apex
- Scheduled Apex
- Future methods for limited/legacy scenarios

### Event-driven

- Platform Events
- Change Data Capture
- Pub/Sub API

## Example

```text
Order Created
      ↓
Salesforce
      ↓
Platform Event
      ↓
Middleware
   ┌──┼───────────┐
   ↓  ↓           ↓
  ERP Email   Analytics
```

## Why async?

- Reduce request latency
- Decouple services
- Absorb traffic spikes
- Improve resilience
- Enable independent consumers

## Key question

Ask:

> "Does this operation need to complete before I return a response to the user?"

If **no**, async is a candidate.

---

# 10. Rate Limiting

Rate limiting protects the platform from overload and abuse.

## Generic pattern

```text
Clients
   ↓
API Gateway
   ↓
Rate Limiter
   ↓
Salesforce
```

## Salesforce-specific considerations

Think about:

- API limits
- Governor limits
- Concurrent requests
- Integration user load
- Burst traffic
- Retry storms
- Bulk ingestion

## Common algorithms

- Token bucket
- Leaky bucket
- Fixed window
- Sliding window

### Senior-level point

Rate limiting is not only for malicious users.

It also protects the system from:

- Accidental traffic spikes
- Misconfigured integrations
- Retry storms
- One client consuming disproportionate capacity

---

# 11. CDN

CDN is useful for globally distributed static/public content.

Example:

```text
User
 ↓
CDN
 ↓
Images / JS / CSS / Public Assets
```

Transactional Salesforce data can continue through Salesforce:

```text
User
 ↓
Salesforce
 ↓
Transactional Data
```

## Think before using CDN

Ask:

- Is the content static?
- Is it public/cacheable?
- Does it need global low latency?
- How frequently does it change?

Don't force a CDN into every Salesforce design.

---

# 12. Design for Failure

This is one of the biggest differences between mid-level and senior HLD answers.

Suppose:

```text
Salesforce
    ↓
Middleware
    ↓
ERP
```

ERP is unavailable.

Weak answer:

> "We'll retry."

Senior answer:

```text
Salesforce
    ↓
Event / Message
    ↓
Middleware
    ↓
ERP ❌
    ↓
Retry with Backoff
    ↓
Retry
    ↓
Still Failing
    ↓
Dead Letter Queue
    ↓
Alert
    ↓
Manual Replay
```

## Mention these

### 1. Timeout

Never wait forever.

### 2. Retry

Retry transient failures.

### 3. Exponential backoff

Avoid hammering an unavailable system.

### 4. Idempotency

Prevent duplicate processing.

Example:

```text
Order ID = ORD123
```

If ORD123 arrives twice, ERP should not create two orders.

### 5. Dead Letter Queue

Move repeatedly failed messages for investigation/replay.

### 6. Failover

Have an alternative path where appropriate.

---

# 13. Idempotency

This deserves special attention in Salesforce integrations.

## Problem

```text
Salesforce
   ↓
Create Order
   ↓
ERP
```

Network timeout occurs.

Salesforce doesn't know whether ERP created the order.

Salesforce retries.

Now ERP may receive:

```text
ORD123
ORD123
```

Without idempotency:

```text
2 orders created ❌
```

With idempotency:

```text
ORD123 → already processed → return existing result
```

## Possible idempotency key

Use a stable business identifier such as:

```text
OrderId
ExternalOrderId
TransactionId
```

depending on the business model.

### Interview phrase

> "Any operation that may be retried should have an idempotency strategy."

---

# 14. Observability

Don't just say:

> "We'll add logging."

Discuss three major areas:

## Metrics

Track:

- Request rate
- Latency
- Error rate
- Queue depth
- Queue lag
- Retry count
- Event processing latency
- API consumption
- Integration failures

## Logs

Capture:

- Correlation ID
- Request ID
- Timestamp
- Integration response
- Error code
- Failure reason
- Processing status

## Tracing

Example:

```text
LWC
 ↓
Apex
 ↓
Middleware
 ↓
ERP
```

A correlation ID should help trace the same business operation across the entire path.

## Salesforce-related observability

Consider:

- Debug Logs
- Event Monitoring
- Platform/integration logs
- Middleware monitoring
- API metrics
- Event processing metrics

---

# 15. Security

Security should be part of HLD, not an afterthought.

## Salesforce-specific areas

Think about:

- Authentication
- Authorization
- Sharing
- CRUD/FLS
- Profiles / Permission Sets
- Named Credentials
- OAuth
- Integration users
- Least privilege
- Encryption
- Sensitive data
- Audit requirements

## Integration security

```text
Salesforce
    ↓
OAuth / Secure Credential
    ↓
Middleware
    ↓
External System
```

Never expose secrets directly in:

- LWC
- Apex code
- Custom settings that aren't designed for secrets
- Source control

---

# 16. Trade-offs

There is no perfect architecture.

A senior engineer explains:

1. What was chosen?
2. Why?
3. What alternative existed?
4. Why wasn't it chosen?
5. What trade-off are we accepting?

## Example

### Platform Event vs REST

Don't say:

> "Platform Events are better."

Say:

> "I'd choose Platform Events when the producer should not synchronously depend on downstream consumers. This gives loose coupling and resilience, but introduces eventual consistency and more complex replay/error handling."

---

# 17. Salesforce Sync vs Async Decision Framework

Use this quick mental model:

```text
Does the user need the result immediately?
              │
        ┌─────┴─────┐
       YES          NO
        │            │
   Synchronous     Async
        │            │
 REST / Apex      Queueable
 Callout          Batch
                  Platform Event
                  CDC
                  Pub/Sub
```

## Prefer synchronous when

- Immediate response is required
- User cannot continue without the result
- Operation is short-lived
- Stronger consistency is important

## Prefer asynchronous when

- Work can happen later
- High volume
- Long-running operation
- Downstream system may be unreliable
- Multiple consumers need the event
- User shouldn't wait

---

# 18. Large Data Volume (LDV)

For Salesforce HLD, LDV is a major topic.

## Think about

- Record count
- Query selectivity
- Indexes
- Data skew
- Sharing calculations
- Ownership
- Archiving
- Async processing
- Bulk APIs
- Big Objects
- External Objects

## Bad design

```text
10M records
    ↓
Non-selective SOQL
    ↓
Slow / limit problems
```

## Better thinking

```text
Selective query
    +
Proper indexing
    +
Async processing
    +
Archival
    +
Externalization where appropriate
```

---

# 19. Data Skew

Data skew can create performance and sharing problems.

Watch for:

- One parent with huge numbers of children
- One user owning huge volumes of records
- One account becoming a hotspot
- Excessive sharing relationships

### HLD question to ask yourself

> "Is the workload evenly distributed, or is one record/user/partition becoming a hotspot?"

---

# 20. System of Record

One of the first architectural questions should be:

> **Who owns this data?**

Example:

```text
Customer / Sales Process
        ↓
    Salesforce

Inventory
        ↓
       ERP

Payment
        ↓
Payment Platform

Analytics
        ↓
Data Warehouse
```

Avoid unnecessary duplication.

If data must exist in multiple systems, define:

- Source of truth
- Synchronization direction
- Update ownership
- Conflict resolution
- Event propagation
- Eventual consistency

---

# 21. Integration Patterns

Know these patterns well.

## Request-Reply

```text
Salesforce → External API → Response
```

Use when immediate response is required.

## Fire-and-Forget

```text
Salesforce → Event → Consumer
```

Use when Salesforce doesn't need to wait.

## Event Notification

```text
Salesforce
    ↓
Platform Event / CDC
    ↓
Multiple Consumers
```

## Batch Data Synchronization

```text
Salesforce
    ↓
Bulk / ETL
    ↓
External System
```

Useful for large-volume data movement.

---

# 22. Salesforce HLD Component Cheat Sheet

| Requirement | Possible Salesforce choice |
|---|---|
| UI | LWC |
| Business logic | Apex |
| Simple synchronous processing | Apex |
| Long-running async work | Queueable / Batch |
| Scheduled work | Scheduled Apex |
| Event-driven communication | Platform Events |
| Record change propagation | CDC |
| High-volume API ingestion | Bulk API / appropriate event/API pattern |
| External API call | REST/SOAP callout |
| Event/API integration | Pub/Sub API where appropriate |
| Frequently accessed cacheable data | Platform Cache |
| Very large specialized datasets | Big Objects / external platform depending on use case |
| External data without copying | External Objects / integration approach |
| CRM data | Salesforce objects |
| Analytics at large scale | Data platform / warehouse |

> **Important:** Don't choose a component because you memorized it. Choose it based on requirements, scale, consistency, limits, and failure behavior.

---

# 23. Complete Example — Salesforce Order Management

## Requirement

Design a scalable Order Management System using Salesforce.

Assumptions:

- Salesforce handles customer/order workflow
- ERP owns fulfillment/inventory
- Payment is external
- Notifications are external
- Analytics is handled by a data platform
- Order volume can spike significantly

## High-Level Architecture

```text
                         Users
                           │
                           ↓
                         LWC
                           │
                           ↓
                     Salesforce
                    ┌──────┴──────┐
                    │ Apex / Data │
                    └──────┬──────┘
                           │
                    Order Transaction
                           │
                  ┌────────┴─────────┐
                  ↓                  ↓
             Payment API        Platform Event
                                      │
                                      ↓
                                  Middleware
                              ┌───────┼────────┐
                              ↓       ↓        ↓
                             ERP   Notification Analytics
```

## Why this architecture?

### Salesforce

Owns:

- Customer context
- Order workflow
- CRM-related information
- User interaction
- Business rules

### Payment

External because payment processing has specialized security and domain requirements.

### ERP

Owns:

- Inventory
- Fulfillment
- Warehouse processes

### Platform Event

Decouples Salesforce from downstream consumers.

### Middleware

Handles:

- Routing
- Transformation
- Retry
- Authentication
- Throttling
- Error handling
- Observability

---

# 24. Order Failure Scenario

## ERP is down

Don't block the user unnecessarily.

```text
Order Created
      ↓
Salesforce
      ↓
Platform Event
      ↓
Middleware
      ↓
ERP ❌
      ↓
Retry
      ↓
Exponential Backoff
      ↓
Retry limit reached
      ↓
DLQ
      ↓
Alert
      ↓
Replay after recovery
```

## Duplicate event

Use:

```text
ExternalOrderId = ORD123
```

Consumer checks whether ORD123 has already been processed.

---

# 25. How to Answer Deep-Dive Questions

## Interviewer: "Why Platform Event?"

Answer:

> "Because the downstream systems don't need to block the user transaction. It gives us loose coupling and allows multiple consumers. The trade-off is eventual consistency and additional replay/error-handling complexity."

---

## Interviewer: "Why not direct REST to ERP?"

Answer:

> "If ERP confirmation is a strict business requirement, synchronous REST can be justified. But if ERP is only a downstream fulfillment process, direct synchronous coupling increases latency and makes ERP availability part of the user transaction. I'd prefer asynchronous integration in that case."

---

## Interviewer: "What if ERP is down?"

Answer:

> "The event is retained in the messaging/integration path, the consumer retries transient failures with exponential backoff, and messages that repeatedly fail move to a DLQ. We alert operations and support replay after the ERP recovers."

---

## Interviewer: "What if the event is delivered twice?"

Answer:

> "The consumer must be idempotent. I'd use a stable business identifier such as the external order ID or transaction ID and ensure processing the same identifier twice doesn't create duplicate business state."

---

## Interviewer: "What happens at 10x traffic?"

Answer:

> "I'd first identify the bottleneck using capacity estimates. Then I'd consider asynchronous buffering, bulk ingestion, middleware throttling, selective queries, caching where appropriate, externalizing workloads that don't belong in Salesforce, and validating all of this against Salesforce governor/API/event limits."

---

# 26. The "Why?" Rule

For every box on your architecture diagram, be ready for:

> **Why this?**

Example:

```text
Platform Event
```

Why?

> Loose coupling + async + multiple consumers.

Then:

> **Why not REST?**

Because synchronous dependency may be unnecessary.

Then:

> **What do we lose?**

Immediate consistency.

Then:

> **How do we handle that?**

Expose processing status and implement retry/replay.

This chain of reasoning is what makes an answer senior-level.

---

# 27. How to Identify What to Use During a Problem

## Step 1 — Is it user-facing and immediate?

```text
YES → Consider synchronous
NO  → Consider async
```

## Step 2 — Is it high volume?

```text
YES → Think bulk/event/async/external processing
NO  → Normal transaction may be enough
```

## Step 3 — Are multiple systems interested in the change?

```text
YES → Event-driven architecture
NO  → Direct integration may be sufficient
```

## Step 4 — Can stale data be tolerated?

```text
YES → Cache/eventual consistency may be acceptable
NO  → Stronger consistency strategy
```

## Step 5 — Can the operation be retried?

```text
YES → Make it idempotent
NO  → Carefully design failure handling
```

## Step 6 — Can one Salesforce transaction handle it?

```text
YES → Synchronous Apex / transaction
NO  → Queueable / Batch / Event / external processing
```

## Step 7 — Is data huge?

```text
YES → LDV strategy
     → Selective SOQL
     → Indexing
     → Archival
     → Big Objects / External Objects
     → External platform where appropriate
```

---

# 28. Common HLD Mistakes

## Mistake 1

Starting with:

> "I'll create Account, Order and OrderItem."

Before understanding requirements.

### Better

First establish:

- Scale
- Ownership
- Consistency
- Integration
- SLA

---

## Mistake 2

Using Salesforce for everything.

### Better

Define system of record for each domain.

---

## Mistake 3

Synchronous integration everywhere.

### Better

Ask:

> "Does the user really need to wait?"

---

## Mistake 4

Saying "retry" without idempotency.

### Better

```text
Retry + Idempotency + Backoff + DLQ
```

---

## Mistake 5

Ignoring Salesforce limits.

### Better

Always consider:

- Governor limits
- API limits
- Data volume
- Query selectivity
- Event volume
- Concurrency

---

## Mistake 6

Saying "use caching" without invalidation.

### Better

Mention:

```text
Cache + TTL + Invalidation + Staleness tolerance
```

---

## Mistake 7

Ignoring security.

### Better

Include:

- Authentication
- Authorization
- Sharing
- CRUD/FLS
- Least privilege
- Secrets
- Encryption
- Audit

---

## Mistake 8

No observability.

### Better

```text
Metrics + Logs + Traces + Alerts
```

---

# 29. Senior-Level HLD Checklist

Before finishing an HLD answer, mentally check:

### Requirements

- [ ] Functional requirements
- [ ] Non-functional requirements
- [ ] Scale
- [ ] SLA/SLO
- [ ] Consistency requirements

### Salesforce

- [ ] Data model
- [ ] Governor limits
- [ ] API limits
- [ ] LDV
- [ ] Bulkification
- [ ] Async processing
- [ ] Sharing/security

### Distributed Systems

- [ ] Caching
- [ ] Queues
- [ ] Events
- [ ] Rate limiting
- [ ] Retry
- [ ] Timeout
- [ ] Idempotency
- [ ] Failure handling
- [ ] Eventual consistency

### Integration

- [ ] System of record
- [ ] Sync vs async
- [ ] REST/SOAP/Bulk/API/Event strategy
- [ ] Middleware
- [ ] Error handling
- [ ] Replay

### Operations

- [ ] Metrics
- [ ] Logs
- [ ] Tracing
- [ ] Alerts
- [ ] Disaster recovery
- [ ] Scalability

### Architecture

- [ ] Why?
- [ ] Why not alternative?
- [ ] Trade-offs
- [ ] Future growth

---

# 30. 30-Second HLD Opening Template

When the interviewer gives you a design problem, start like this:

> "Before I jump into the architecture, I'd like to clarify the scale, users, read/write pattern, consistency requirements, latency/SLA, integrations, and failure expectations. Then I'll estimate the traffic, identify the system of record, propose the high-level architecture, and finally deep-dive into Salesforce components, integration, scalability, security, and failure handling."

This immediately gives your answer structure.

---

# 31. 2-Minute Architecture Explanation Template

Use this structure:

### 1. Requirements

> "The main requirements are..."

### 2. Scale

> "Assuming X users and Y transactions/day, peak traffic is approximately Z..."

### 3. System of Record

> "Salesforce owns..., while ERP owns..."

### 4. Architecture

> "The request comes through..., Salesforce handles..., and downstream processing is..."

### 5. Async

> "I use Platform Events/async processing because..."

### 6. Failure

> "If the downstream service fails, we..."

### 7. Security

> "Authentication/authorization is handled through..."

### 8. Trade-off

> "The main trade-off is..."

---

# 32. Final Mental Model

When solving any Salesforce HLD problem, ask these questions:

```text
                    ┌──────────────────┐
                    │  WHAT IS NEEDED? │
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ HOW MUCH SCALE?  │
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ WHO OWNS DATA?   │
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ SYNC OR ASYNC?   │
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ HOW DOES IT FAIL?│
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ HOW DOES IT SCALE│
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ HOW IS IT SECURE?│
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │ WHAT ARE THE     │
                    │ TRADE-OFFS?      │
                    └──────────────────┘
```

---

# 33. The Ultimate Salesforce HLD Formula

Remember this:

```text
REQUIREMENTS
     +
SCALE
     +
DATA OWNERSHIP
     +
SALESFORCE LIMITS
     +
SYNC / ASYNC
     +
INTEGRATION
     +
CONSISTENCY
     +
FAILURE HANDLING
     +
SECURITY
     +
OBSERVABILITY
     +
TRADE-OFFS
     =
STRONG SENIOR-LEVEL HLD ANSWER
```

## The biggest mindset shift

Don't try to prove:

> **"I know Salesforce components."**

Try to prove:

> **"I can make architecture decisions under scale, constraints, failures, and business requirements — and I can explain why."**

That is the mindset expected in strong **Senior / Lead / Architect-level Salesforce HLD interviews**.
