# Think Like an Architect – Episode 2
## Aligning Integration Patterns to Use Cases
### Salesforce Architecture | Complete English Learning Notes

> **Source:** User-provided episode transcript (427 lines).  
> **Purpose:** Preserve the source, explain the architecture, complete missing conceptual flows, and provide architect/interview-level revision.  
> **Separation:** “Original Transcript — Preserved” reproduces the supplied transcript. Explanations and “Additional Technical Flow” are editorial additions.

---

# Part 1 — Learning Notes

## 1. Episode Objective and Context

The episode demonstrates how an architect maps business requirements to integration patterns. The key lesson is not to begin with a favorite product or technology. Start with the business requirement, identify constraints, evaluate options, document assumptions, and justify the selected architecture.

### 1.1 Speakers and context
- Mary McCabe — Salesforce CTA and leader of the Architect Evangelism team.
- Gulal Kumar — Salesforce Software Engineering Architect with enterprise integration and platform architecture experience.
- Topic: Aligning integration patterns to use cases.

## 2. The Three-Step Architectural Thinking Framework — WHAT, HOW, WHY

### 2.1 WHAT — Understand the requirement
Extract meaningful signals from the business statement:
- What business problem is being solved?
- Which users and systems are involved?
- Where does the data reside?
- What is the data volume?
- Is read access, write access, or both required?
- Is the latency requirement real time, near real time, or batch?
- Is replication permitted?

Output: Clear, testable high-level requirements.

### 2.2 HOW — Design the solution
- Map suitable patterns to requirements.
- Consider requirements holistically; one design choice can affect another.
- Draw the architecture.
- Record questions, assumptions, limitations and decisions.
- Evaluate out-of-the-box capabilities and Total Cost of Ownership (TCO).

### 2.3 WHY — Explain and record decisions
Capture why the selected pattern is appropriate, why alternatives were rejected, and which assumptions or limitations influenced the decision. This supports stakeholder alignment and future maintenance. Record important choices in an Architectural Decision Record (ADR).

## 3. Business Case — Shipment Management

### 3.1 Requirement decomposition

| Business statement | Signal | Architectural implication |
|---|---|---|
| Service representatives need a 360-degree view | 360-degree view | Unified Salesforce user experience |
| 50 million historical shipment records | 50 million | Volume, query performance, limits and scalability matter |
| Records reside in an on-premise database | On-premise | Network boundary, connectivity and security |
| View and edit directly in Salesforce UI | Read + Write | Read-only virtualization is insufficient |
| Do not replicate into Salesforce | No replication | Evaluate data virtualization/external data access |
| Shipment Dispute records are created and managed in Salesforce | Salesforce-owned operational data | Native Salesforce records |
| Notify a legacy Data Warehouse whenever a record changes | Record-level notification | Event-driven integration |
| Near real time | Near-real-time | Asynchronous events are a strong candidate |

### 3.2 High-level requirements

**HR-1:** Expose 50 million shipment records from the on-premise database in Salesforce without replicating them into Salesforce storage.

**HR-2:** Provide read/write access to those shipment records directly through the Salesforce UI.

**HR-3:** When Salesforce-managed Shipment Dispute records change, notify the Data Warehouse of the record-level change in near real time.

### 3.3 Separate the two data domains

1. **Historical Shipment Records:** The on-premise database remains the system of record; the data must not be copied into Salesforce.
2. **Shipment Dispute Records:** These are created and managed in Salesforce; their changes must be communicated to the downstream Data Warehouse.

The two data domains have different requirements and do not need to use the same integration pattern.

## 4. Requirements 1 and 2 — Exposing 50 Million External Shipment Records

### 4.1 Pattern selection: Data Virtualization

The phrase “without being replicated into Salesforce” points toward data virtualization.

**Data virtualization** presents data from its source to a consuming application without requiring a full persistent copy in that application. In this case, the shipment data remains in the on-premise database while users access it through Salesforce.

### 4.2 Proposed architecture

```text
Service Representative
        |
        v
Salesforce UI
        |
        v
Salesforce Connect
        |
        v
External Object
        |
        | OData 4.0 request
        v
MuleSoft
        |
        | API / JDBC / ODBC / supported connector
        v
On-Premise Database
        |
        v
50 Million Historical Shipment Records
```

### 4.3 Component responsibilities

**Salesforce UI**
- Provides the unified user experience.
- Users can access shipment information without necessarily opening the legacy application.

**Salesforce Connect**
- Surfaces external-system data in Salesforce through External Objects.
- External Objects can participate in Salesforce UI experiences, but their data remains external.
- External Object API names commonly use the `__x` suffix, while custom Salesforce objects use `__c`.

**OData 4.0**
- Provides a standardized HTTP-based interface for accessing external resources.
- The episode selected OData 4.0 to align with the read/write requirement.
- The endpoint must implement the operations and metadata behavior expected by the Salesforce Connect integration.
- Selecting a protocol does not automatically guarantee full CRUD compatibility.

**MuleSoft**
- Acts as the mediation and integration layer between Salesforce and the legacy database.
- Can abstract backend systems and provide protocol mediation, transformation, orchestration, security, monitoring and error handling.
- Can help avoid exposing the on-premise database directly to Salesforce.

**On-premise database**
- Remains the source of truth for the 50 million shipment records.
- Read and write operations ultimately reach this system.

### 4.4 End-to-end read flow

1. A service representative opens a shipment record in Salesforce.
2. Salesforce Connect prepares an external-object request.
3. The request is sent to the OData endpoint.
4. MuleSoft receives and validates the request.
5. MuleSoft retrieves the requested data from the on-premise database.
6. MuleSoft returns an OData-compatible response.
7. Salesforce displays the external record in the UI.

### 4.5 End-to-end write flow

1. The user edits an external shipment record.
2. Salesforce Connect issues a supported OData update operation.
3. MuleSoft authenticates and validates the request.
4. MuleSoft executes the corresponding backend operation.
5. The on-premise database persists the update.
6. A success or failure response travels back through MuleSoft.
7. Salesforce presents the result to the user.

> **Important:** Write-back is possible only when the selected external data source, OData implementation, Salesforce Connect configuration and backend operations support the required write operations. Validate this in a proof of concept.

## 5. Data 360 / Data Cloud Option — Why It Was Rejected in the Episode

The speaker initially considered using Data 360 as a virtualization route:
- Virtualize on-premise data through zero-copy.
- Expose it back to the Salesforce experience.

The episode cited two limitations at the time of recording:
1. The required zero-copy support for on-premise databases was not available.
2. The discussed Data 360 path was read-oriented and did not satisfy the write requirement.

Therefore, the option was rejected for this particular case.

**Version warning:** These statements describe product capabilities at the time of the episode. Do not assume they represent the 2026 product position. Re-verify current connectors, zero-copy federation, write-back support, source coverage, licensing and regional availability before implementation.

## 6. External Object Sharing and Security

The episode made an assumption that external-object sharing differs from normal Salesforce custom-object sharing and used broad public read/write visibility as an example.

Validate:
- Which users can view shipment records?
- Which users can edit them?
- Is access restricted by customer, account, region or territory?
- Does the backend enforce authorization?
- Is Salesforce user identity propagated to MuleSoft and the source?
- Are credentials shared or per-user?
- What are the CRUD, field-level, row-level and audit requirements?

**Security principle:** Hiding a button or field in the UI is not a substitute for authorization at a trusted server/backend layer.

If restrictive record-level access is required, the broad public-access assumption may not work. The episode mentioned Apex and Lightning Web Components as a possible alternative for custom access management.

## 7. OData 4.0, JDBC and ODBC

### 7.1 What is OData?
OData (Open Data Protocol) is an HTTP-based protocol for exposing resources with standardized query and data-operation semantics.

Common concepts include:
- Entity and Entity Set
- Metadata document
- `$filter`
- `$select`
- `$orderby`
- `$top`
- `$skip` (subject to implementation-specific limitations)
- CRUD operations

### 7.2 Why OData 4.0?
The episode selected OData 4.0 to align with read/write access. An arbitrary OData 4.0 endpoint is not automatically fully compatible with Salesforce Connect. Validate the compatibility matrix and supported operations.

### 7.3 JDBC / ODBC
- JDBC and ODBC are database connectivity technologies.
- They are not direct replacements for Salesforce Connect.
- MuleSoft or another integration service can use them to access a legacy database.
- The actual choice depends on the database, drivers, network, security and supported MuleSoft connectors.

## 8. Requirement 3 — Notify the Data Warehouse of Salesforce Dispute Changes

### 8.1 Pattern selection: Fire-and-Forget / Event-Driven

The requirement contains important signals:
- Whenever a record is updated.
- Record-level change.
- Notify the Data Warehouse.
- Near real time.

These signals support an asynchronous event-driven approach.

**Fire-and-forget** means the source publishes a message/event without making the end user wait for downstream processing to complete. It does not mean delivery is guaranteed without reliability controls. Retries, persistence, replay, monitoring and reconciliation are still required.

### 8.2 Why not request-reply?
Request-reply is appropriate when the caller needs a response in the same interaction. Here, the service representative does not need to wait for the Data Warehouse to finish processing.

### 8.3 Why not batch?
A scheduled batch introduces waiting time until the next run. Batch is appropriate when the business accepts hourly, daily or weekly latency, or when bulk reconciliation is required.

## 9. Snapshot vs Delta Payloads

| Type | Meaning | Example |
|---|---|---|
| Snapshot | Full current record state after the update | Status=Resolved, Amount=500, Owner=User A |
| Delta | Only the fields/values that changed | Status: Open → Resolved |

The episode interpreted “what has changed” as a delta requirement.

### 9.1 Delta benefits
- Smaller payloads.
- Identifies precisely which fields changed.
- Avoids transferring unchanged values.

### 9.2 Delta risks
- Consumers may need previous state.
- Missed or out-of-order events can leave target state inconsistent.
- Some recovery processes need a full snapshot.
- Deletes, null values, omitted fields and relationship changes require careful semantics.

Confirm with stakeholders:
- Delta or full post-update snapshot?
- Are delete events required?
- Are old values required?
- Is field-specific business mapping/filtering needed?
- Does event order matter?
- Can the Data Warehouse perform idempotent upserts?

## 10. Change Data Capture (CDC) vs Platform Events

### 10.1 Change Data Capture
CDC publishes change events when Salesforce records change, for supported record operations.

For this use case:
1. A Shipment Dispute record is updated.
2. Salesforce generates a CDC event.
3. The event is made available through the Salesforce event bus.
4. MuleSoft subscribes to the event.
5. MuleSoft transforms/routes the event.
6. The Data Warehouse is updated.

CDC is a strong fit when the requirement is straightforward notification of actual record changes and custom event-payload logic is not required.

### 10.2 Platform Events
Platform Events represent custom business events with a developer-defined event contract.

Example: `ShipmentDisputeEscalated__e`
- DisputeId
- ShipmentId
- EscalationReason
- Priority
- RequestedBy

A Platform Event can be published through supported mechanisms such as Flow, Apex or APIs.

### 10.3 Decision matrix

| Requirement | Strong starting candidate |
|---|---|
| Publish actual Salesforce record field changes | CDC |
| Define a custom business event/payload | Platform Event |
| Emit an event only when a specific business condition occurs | Platform Event / Flow / Apex |
| Automatically notify subscribers of record changes | CDC |
| Distribute a domain-level event to multiple consumers | Platform Event / messaging layer |
| Consume changed-field information | CDC |

**Nuance:** Do not assume CDC and Platform Event allocations are always identical or always different. Verify current org edition, event type, subscriber mechanism, add-ons and allocation documentation.

## 11. CDC End-to-End Processing Flow

```text
User updates Shipment Dispute in Salesforce
                  |
                  v
        Salesforce transaction commits
                  |
                  v
       Change Data Capture event created
                  |
                  v
          Salesforce Event Bus
                  |
                  v
       MuleSoft CDC subscriber
       (Pub/Sub API or Streaming API)
                  |
                  v
        Validate + transform event
                  |
                  v
      Idempotency / duplicate check
                  |
                  v
       Retry + error management
                  |
                  v
       Data Warehouse API / load
                  |
                  v
        Target acknowledges success
                  |
                  v
       Persist processing checkpoint
```

### 11.1 Transaction and event
A change event becomes available to subscribers in connection with the committed record transaction. Interpret ordering using the event-stream and transaction metadata.

### 11.2 Subscribe
MuleSoft acts as an external subscriber to the CDC channel. Salesforce supports CDC subscriptions through Pub/Sub API and Streaming API.

### 11.3 Process
- Decode the event schema.
- Identify the object and record.
- Inspect `ChangeEventHeader` metadata.
- Identify changed fields.
- Apply the required mapping.
- Send the result to the target.

### 11.4 Acknowledge and checkpoint
Advance a durable processing checkpoint only after successful target processing. The exact acknowledgement/checkpoint implementation depends on the connector and consumer architecture.

## 12. Streaming API (CometD) vs Pub/Sub API (gRPC)

The speakers explain that external clients can consume CDC and Platform Events through Streaming API or Pub/Sub API.

| Feature | Streaming API | Pub/Sub API |
|---|---|---|
| Protocol | CometD / Bayeux | gRPC over HTTP/2 |
| Payload | JSON | Apache Avro binary payload |
| Subscription model | Push-oriented | Client requests event volume; flow controlled |
| Flow control | Client does not control delivery volume in the same way | Client controls requested event count |
| Schema | JSON event structure | Schema retrieval through Pub/Sub API |
| Publish result | Traditional Streaming API publishing model does not provide the same final publish result | Final publish result is available |
| Multiplexing | Does not provide HTTP/2 multiplexing | Benefits from HTTP/2 multiplexing |
| Episode recommendation | Existing/available option | Preferred modern interface where supported |

### 12.1 Pub/Sub API flow control
1. The client subscribes.
2. The client specifies how many events it can receive.
3. Salesforce delivers the requested events.
4. The client processes them.
5. The client requests more when it has capacity.

This helps prevent uncontrolled event delivery to a slow consumer.

### 12.2 Technical clarification
The shorthand “CDC + gRPC = Pub/Sub API” and “CDC + CometD = Streaming API” is conversational. CDC is the event model; Pub/Sub API and Streaming API are interfaces/protocols for accessing event streams. They operate at different layers.

### 12.3 MuleSoft connector
The episode states that MuleSoft offers connectors for both approaches. Verify the exact connector/version, Pub/Sub API support, acknowledgement behavior, replay handling and deployment compatibility for the implementation.

## 13. Replay ID, Retention and Recovery

### 13.1 What is Replay ID?
Replay ID identifies a position in the event stream. It is opaque; do not treat it as a sequential integer or assume arithmetic continuity.

### 13.2 Recovery flow
1. Receive an event.
2. Process it.
3. Persist the last successfully processed Replay ID/checkpoint.
4. If the consumer disconnects or crashes, reconnect.
5. Resume from the saved position.
6. Replay missed events while they remain within the retention window.

### 13.3 Retention
Current Salesforce developer documentation describes 72-hour/three-day event-bus retention for CDC and Platform Events. Availability beyond 72 hours is not guaranteed. Long-outage recovery therefore needs reconciliation/backfill, not replay alone.

## 14. Idempotency, Retry, Persistence and Error Handling

### 14.1 Idempotency
Repeated processing of the same event/request must not create duplicate or incorrect side effects.

Example:
- Event: Dispute `D-100` changes from Open to Resolved.
- The same event is delivered three times.
- The target must not create three duplicate transactions.
- Use a stable event identity/business key and versioning strategy to detect duplicates.

### 14.2 Retry strategy
- Transient error: retry with exponential backoff and jitter.
- Permanent validation error: do not retry indefinitely; route to a dead-letter/error store.
- Rate limiting: respect Retry-After/backoff.
- After retry exhaustion: alert and initiate controlled recovery.

### 14.3 Persistence
Persist in-flight/failed events, payload references, processing state, retry count, error details and checkpoints in durable storage.

### 14.4 Ordering and sequencing
- Does order matter per record or across the whole object?
- Parallel processing increases throughput but can introduce ordering risks.
- A partition key such as Shipment/Dispute ID can route the same record's events to the same ordered lane.
- Global ordering can constrain throughput and increase complexity.
- Kafka/Heroku Kafka can provide partitioning and multiple consumers, but adds infrastructure and operational cost.

### 14.5 Watermark
A watermark/checkpoint records processing progress. It allows a consumer to resume safely after restart. A CDC Replay ID can be part of the checkpoint strategy; target-side watermarking and reconciliation can also be useful.

## 15. MuleSoft as an Integration Layer

MuleSoft is more than a connector or pipe. It can:
- Abstract backend systems.
- Mediate between Salesforce-facing APIs and legacy protocols.
- Implement authentication and security boundaries.
- Transform and orchestrate messages.
- Subscribe to CDC and distribute events downstream.
- Centralize retry, idempotency, persistence, monitoring and error handling.

### 15.1 MuleSoft + Kafka pattern

```text
Salesforce CDC
     |
     v
MuleSoft Subscriber
     |
     v
Kafka Topic / Partitions
     |             |
     v             v
Data Warehouse   Other Consumer
```

A broker can reduce the need for multiple independent systems to subscribe directly to Salesforce. MuleSoft can consume once and distribute through the broker. Partition keys can help maintain per-record ordering. This adds operational complexity and cost.

## 16. Alternatives Discussed in the Q&A

### 16.1 Batch every minute
- Not automatically equivalent to near-real-time event processing.
- Consider Data API limits, job overhead, latency and operational complexity.
- Use batch when the business accepts the latency.

### 16.2 Canvas / iframe / UI mashup
- Can embed or mash up an existing external web UI inside Salesforce.
- Requires a suitable web UI and appropriate security/integration design.
- The episode considered it less suitable than the Salesforce Connect external-object experience for this scenario.

### 16.3 AWS EventBridge
- An option for distributing events to AWS-hosted consumers.
- Relevant in an AWS-centric landscape.
- Validate Salesforce-to-AWS connectivity, transformation, security and delivery guarantees.

### 16.4 Data 360
- Rejected in the episode because of the zero-copy on-premise limitations discussed at that time.
- Having ingestion connectors and having zero-copy federation are different capabilities.

### 16.5 A2A and MCP
- A2A: Agent-to-agent communication and orchestration.
- MCP: A protocol for connecting AI applications/agents to tools and resources.
- Data virtualization: Accessing source data without copying it into the consuming platform.
- These are not competing synonyms; they serve different purposes/layers.

## 17. Architecture Decision and Assumption Register

| ID | Type | Decision / assumption | Validation |
|---|---|---|---|
| D-01 | Decision | Data virtualization for shipment history | No-replication requirement |
| D-02 | Decision | Salesforce Connect + External Objects | UI/CRUD compatibility |
| D-03 | Decision | OData 4.0 interface | Exact supported operations |
| D-04 | Decision | MuleSoft mediation layer | Connectivity/security/network |
| A-01 | Assumption | Legacy DB is accessible through API/JDBC/ODBC | Database discovery |
| A-02 | Assumption | OData 4.0 write operations are supported | Proof of concept |
| A-03 | Assumption | Broad external-record access is acceptable | Security/business sign-off |
| D-05 | Decision | CDC for dispute change notification | Simple record delta |
| A-04 | Assumption | Data Warehouse needs delta payload | Stakeholder confirmation |
| A-05 | Assumption | No custom field-selection logic is required | Payload mapping review |
| D-06 | Decision | MuleSoft consumes CDC and updates warehouse | Connector/API design |
| D-07 | Principle | Prefer out-of-the-box capability and lower TCO | Cost/complexity assessment |

## 18. Additional Enterprise-Level Production Readiness

The following additions complete the discussion for a production architecture.

### 18.1 Security
- TLS/mTLS where applicable.
- OAuth/client credentials or an approved identity mechanism.
- Store secrets in a secrets manager.
- Least-privilege service accounts.
- Network allowlisting/private connectivity where supported.
- Data classification and encryption in transit/at rest.
- Audit logs and access reviews.
- Explicit decision on per-user versus named-principal identity.

### 18.2 External data performance
Fifty million records does not mean retrieving fifty million records in one request.
- Selective filters and indexed search keys.
- Pagination and server-side query execution.
- Narrow field selection.
- Query timeout and concurrency testing.
- Source database indexes and query plans.
- Caching only when freshness/consistency permits.
- Test Salesforce list/search/UI patterns against representative volumes.

### 18.3 Data consistency
- Identify the source of truth.
- Handle concurrent updates and optimistic locking/version checks.
- Handle uncertain write outcomes after timeouts.
- Test read-after-write consistency.
- Make repeated update requests safe.

### 18.4 Observability
Measure:
- Event lag (commit to consumer).
- End-to-end delivery latency.
- Throughput and backlog.
- Retry and dead-letter counts.
- Subscriber disconnects.
- Replay/recovery activity.
- External-object query latency.
- Backend error rate.
- Data reconciliation mismatches.

### 18.5 Disaster recovery and reconciliation
- Reconcile source and target after outages exceeding the event retention window.
- Compare records using updated timestamps/watermarks.
- Maintain a backfill API/bulk extract process.
- Back up Replay ID checkpoints.
- Handle poison messages.
- Define runbooks and alert ownership.

### 18.6 Non-functional requirements
Define:
- p95/p99 latency targets.
- Peak events per second.
- Maximum acceptable backlog.
- Recovery Time Objective (RTO).
- Recovery Point Objective (RPO).
- Availability/SLA.
- Data residency/compliance.
- Maintenance windows.
- Cost/TCO.

## 19. Exam and Interview Traps

1. **No replication** → investigate data virtualization/external data access; do not default to replication.
2. **Read + Write** → do not select read-only federation without validation.
3. **50 million records** → test selectivity, pagination, indexing, concurrency and source performance.
4. **Near real time** → consider event-driven integration; it does not mean zero latency.
5. **Automatic record-change notifications** → CDC is a strong candidate.
6. **Custom business payload** → Platform Events are a strong candidate.
7. **CDC vs Pub/Sub API** → CDC is the event model; Pub/Sub API is an interface.
8. **Replay ID** → opaque checkpoint, not an arithmetic sequence.
9. **Fire-and-forget** → not reliable without retry, persistence, monitoring and reconciliation.
10. **Idempotency** → makes duplicate delivery/retry safe.
11. **Batch every minute** → not automatically equivalent to event-driven near real time.
12. **External object security** → validate actual row-level/backend authorization.
13. **MuleSoft** → abstraction, orchestration, reliability and distribution layer—not just a connector.
14. **Kafka** → ordering is scoped to partitions/keys; global ordering is not automatic.
15. **A2A/MCP/data virtualization** → different purposes and layers.

## 20. Recommended Architecture — Case Study Answer

### Historical Shipment Records
`Salesforce UI → Salesforce Connect → External Object → OData 4.0 → MuleSoft → On-Premise Database`

Why:
- Avoids duplicating 50 million records in Salesforce.
- Surfaces external records in the Salesforce UI.
- Supports the read/write requirement through a compatible connector and endpoint.
- MuleSoft provides backend abstraction, security, transformation and monitoring.

### Shipment Dispute Change Notification
`Salesforce Dispute Update → CDC → Event Bus → MuleSoft (Pub/Sub API preferred where supported) → Idempotency/Retry/Persistence → Data Warehouse`

Why:
- Automatic record-change notification.
- Asynchronous near-real-time delivery.
- No stated need for custom event-payload logic.
- MuleSoft manages reliability, transformation and downstream integration.

**Architect's caveat:** This is a logical target architecture, not production sign-off. Validate OData write compatibility, Salesforce Connect behavior, event allocations, security, volume, latency and recovery through a proof of concept and load testing.

## 21. Interview Questions and Answers

**Q1. Why Salesforce Connect instead of storing 50 million shipment records in Salesforce?**  
The requirement prohibits replication. Salesforce Connect can expose external data through External Objects while the source remains on-premise.

**Q2. What is data virtualization?**  
Presenting and accessing data from its source without creating a full persistent copy in the consuming platform.

**Q3. Why place MuleSoft between Salesforce and the on-premise database?**  
It mediates protocols, abstracts backend systems, enforces security, transforms requests/responses, monitors integration and manages failures.

**Q4. Why OData 4.0?**  
The episode selected it to align with read/write external-object access. Actual CRUD support must be validated against the endpoint and Salesforce Connect compatibility.

**Q5. CDC vs Platform Events?**  
CDC is suited to publishing Salesforce record changes. Platform Events are suited to custom business events and custom payloads.

**Q6. Why not request-reply for the Data Warehouse update?**  
The service representative does not need to wait for warehouse processing. Asynchronous event-driven integration better matches the requirement.

**Q7. What is Replay ID?**  
An opaque identifier representing an event's position in the event stream, used to resume consumption within the retention window.

**Q8. What is idempotency?**  
Repeated processing of the same event does not create duplicate or incorrect side effects.

**Q9. How do you prevent event loss?**  
Use durable checkpointing, replay within retention, persistence, retries, dead-letter handling, monitoring and reconciliation/backfill.

**Q10. Why Pub/Sub API over Streaming API?**  
It uses gRPC/HTTP2, Avro payloads and client-controlled flow control. It is the preferred modern interface where supported.

**Q11. Can Kafka help?**  
Yes. It can buffer and fan out events to multiple consumers, with per-key partition ordering. It adds infrastructure and operational complexity.

**Q12. Is near real time the same as real time?**  
No. Near real time allows a small processing/delivery delay. Define an explicit latency SLA.

## 22. Summary — Quick Revision

- Begin with WHAT → HOW → WHY.
- Convert narrative requirements into measurable high-level requirements.
- “50 million + no replication” points toward data virtualization.
- Salesforce Connect exposes external records through External Objects.
- OData 4.0 was selected for read/write intent; verify compatibility.
- MuleSoft connects Salesforce-facing APIs to the on-premise database.
- Shipment Dispute records are managed in Salesforce.
- Near-real-time notification points toward asynchronous event-driven integration.
- CDC is preferred for straightforward record-change deltas.
- Platform Events support custom business-event payloads.
- Pub/Sub API is a modern gRPC/HTTP2 interface for event publishing/subscription.
- Idempotency, retries, persistence, replay, monitoring and reconciliation make the architecture reliable.
- Document assumptions and rejected alternatives in an ADR.

## 23. No-Skip Checklist

- [ ] WHAT/HOW/WHY framework
- [ ] Business requirement keyword extraction
- [ ] 360-degree view
- [ ] 50 million historical records
- [ ] On-premise database
- [ ] No replication
- [ ] Read/write UI access
- [ ] Shipment Dispute records in Salesforce
- [ ] Near-real-time Data Warehouse notification
- [ ] Data virtualization
- [ ] Data 360 zero-copy consideration and episode-era limitations
- [ ] Salesforce Connect and External Objects
- [ ] MuleSoft mediation
- [ ] OData 4.0 vs JDBC/ODBC
- [ ] External-object sharing/security assumption
- [ ] Out-of-the-box and TCO principle
- [ ] Fire-and-forget vs request-reply vs batch
- [ ] Snapshot vs delta
- [ ] CDC vs Platform Events
- [ ] Streaming API/CometD vs Pub/Sub API/gRPC
- [ ] HTTP/2, Avro, flow control and multiplexing
- [ ] Event allocation/limits discussion
- [ ] Kafka/Heroku Kafka and partition ordering
- [ ] Batch-every-minute trade-off
- [ ] On-premise exposure/security boundary
- [ ] Canvas/iframe/UI mashup
- [ ] AWS EventBridge
- [ ] CDC enrichment/filtering
- [ ] External-object write-back
- [ ] MuleSoft subscription and fan-out
- [ ] Idempotency, retries, persistence and error handling
- [ ] Watermark and Replay ID
- [ ] A2A, MCP and data virtualization distinction
- [ ] ADR and final architecture

## 24. Board-Level Answer (60–90 seconds)

“We have two distinct integration requirements. For 50 million historical shipment records, the business explicitly prohibits replication but requires read/write access in Salesforce. I would evaluate Salesforce Connect with External Objects over an OData 4.0 endpoint, with MuleSoft mediating access to the on-premise database. This preserves the source as the system of record and provides a unified Salesforce UI, subject to validating CRUD support, security, query performance and connector compatibility.

For Shipment Dispute records managed natively in Salesforce, the Data Warehouse needs near-real-time notification of record changes. I would use Change Data Capture and have MuleSoft subscribe through Pub/Sub API where supported, transform and route the delta, and update the warehouse asynchronously. I would add idempotency, durable checkpoints, retry/backoff, dead-letter handling, monitoring and reconciliation. I would document assumptions, event-volume limits, latency SLAs and recovery behavior in ADRs.”

---

# Part 2 — Additional Technical Flow and Missing Concepts

## 25. External Object Design Checklist

Before implementation:
1. Confirm Salesforce Connect entitlement/edition and external data source support.
2. Confirm OData version and supported CRUD/query operations.
3. Validate metadata discovery and field type mapping.
4. Decide authentication model and user identity propagation.
5. Define external-object relationships and UI usage.
6. Test filters, sorting, pagination, search and list views.
7. Verify backend indexes and query plans.
8. Test concurrent updates and timeout/partial-failure cases.
9. Validate row-level authorization at the source.
10. Run load tests with representative data and concurrency.

## 26. CDC Event Processing Pseudocode

```text
subscribe(channel, savedReplayPosition)

for each event:
    eventKey = deriveStableEventIdentity(event)

    if alreadyProcessed(eventKey):
        continue

    persist(event, status="RECEIVED")

    try:
        transformed = mapToWarehouseContract(event)
        sendToWarehouse(transformed)
        markProcessed(eventKey)
        saveCheckpoint(event.replayId)
    catch transientError:
        retryWithBackoff(event)
    catch permanentError:
        moveToDeadLetterStore(event)
        alertSupportTeam()
```

**Implementation note:** Derive stable event identity from actual event metadata and business keys. Do not assume Replay ID alone is a globally permanent business identifier.

## 27. Snapshot and Delta Example

State before update:
```json
{
  "disputeId": "DSP-100",
  "status": "Open",
  "priority": "Normal",
  "owner": "Agent A"
}
```

State after update:
```json
{
  "disputeId": "DSP-100",
  "status": "Resolved",
  "priority": "Normal",
  "owner": "Agent A"
}
```

Illustrative delta-style downstream message:
```json
{
  "disputeId": "DSP-100",
  "changedFields": {
    "status": {
      "oldValue": "Open",
      "newValue": "Resolved"
    }
  }
}
```

This is an illustrative downstream contract, not a literal CDC payload schema.

## 28. Architecture Decision Record Template

| Field | Example |
|---|---|
| Context | 50M external shipments + Salesforce disputes |
| Decision | Salesforce Connect for history; CDC for dispute changes |
| Alternatives | Data 360 zero-copy, custom Apex/LWC, Platform Events, batch |
| Drivers | No replication, read/write, near real time, TCO |
| Assumptions | OData compatibility, backend connectivity, access model |
| Risks | Query latency, event allocation, ordering, outage recovery |
| Validation | POC, load test, security review, failure test |
| Owner | Architecture / integration team |
| Review trigger | Product capability, volume, SLA or security changes |

## 29. Salesforce Documentation — 2026 Verification Notes

These checks are added separately from the original episode statements:

- Salesforce documents Pub/Sub API as a gRPC/HTTP2 interface for publishing and subscribing to Platform Events and CDC events, with Avro event payloads and client-controlled event flow. See [Pub/Sub API overview](https://developer.salesforce.com/docs/platform/pub-sub-api/guide/intro.html) and [Subscribe with Pub/Sub API](https://developer.salesforce.com/docs/platform/change-data-capture/guide/cdc-subscribe-pubsub-api.html).
- Salesforce's API comparison describes Streaming API as CometD/Bayeux with JSON and Pub/Sub API as gRPC/protocol buffers with Avro and flow control. See [Streaming API vs Pub/Sub API](https://developer.salesforce.com/docs/platform/api-streaming/guide/pubsub-api-streaming-api-comparison.html).
- Salesforce documents CDC and Platform Event retention as 72 hours/three days, with Replay ID representing an opaque stream position. See [Event Message Durability](https://developer.salesforce.com/docs/platform/pub-sub-api/guide/event-message-durability.html) and [Change Event Storage and Delivery](https://developer.salesforce.com/docs/platform/change-data-capture/guide/cdc-subscribe-delivery.html).
- Salesforce documents event delivery allocations shared by high-volume Platform Events and CDC for API subscribers. Allocations depend on org entitlement and add-ons; do not reuse a generic remembered limit without checking the target org. See [CDC Allocations](https://developer.salesforce.com/docs/platform/change-data-capture/guide/cdc-allocations.html).
- Salesforce documents stream filtering and event usage monitoring. See [Monitor CDC Usage](https://developer.salesforce.com/docs/platform/change-data-capture/guide/cdc-monitor-usage.html).
- The transcript's Data 360 zero-copy and write-access statements are episode-era context. Re-check current Data 360 connector/federation documentation for the exact source, deployment region, licensing and write-back requirement before making a 2026 architecture decision.

---

