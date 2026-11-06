
# Think Like an Architect — Episode 3
# Design a High-Scale Multicloud Incident Journey
## English Notes | Complete Source-Preserving Edition

> **Source vs additional content:** Sections 1–15 explain and organize the episode. Section 16 contains 2026 Salesforce documentation checks and clearly identifies what has changed or needs validation. Section 17 contains additional architecture recommendations and illustrative code. The complete original transcript is appended verbatim at the end. Speech-to-text errors and original wording are intentionally retained in that transcript.

## 1. Episode objective

This episode designs a high-scale multicloud incident journey for severe-weather events that cause electricity-grid outages. The architecture must:
- Receive outage events from SCADA / grid-monitoring systems.
- Identify affected customers across multiple legacy systems.
- Send personalized notifications based on real-time outage information.
- Log and update incidents in Salesforce.
- Enable two-way customer communication through WhatsApp and a website.
- Route incoming customer interactions to the right teams.
- Provide coordinated, context-aware updates throughout the incident lifecycle.

### Speakers / expertise (as stated in the transcript)
- Mary/Miriam McCabe — Salesforce Architect Evangelism.
- Scott Ratliff — MuleSoft and Agentforce specialization.
- Kunal Modi — marketing, data and Salesforce solution architecture.

> The transcript contains inconsistent name spellings in places due to speech recognition. The original wording is preserved in the transcript section below.

## 2. The WHAT–HOW–WHY approach

| Step | Meaning | Architect output |
|---|---|---|
| WHAT | Read the business requirement, highlight key phrases and translate them into high-level requirements (HLRs). | Scope, business goals, constraints and non-functional requirements |
| HOW | Align solution components to the HLRs. Draw the architecture and record questions, assumptions, limitations and decisions. | High-level architecture diagram |
| WHY | Justify the decisions taken during the HOW step. | Architecture Decision Records (ADRs) and stakeholder rationale |

### 2.1 WHAT — business requirement to HLRs
The episode derives these HLRs:

1. Instantly detect unplanned grid outages and notify Salesforce.
2. Identify affected parties across multiple legacy systems.
3. Send automated personalized notifications based on real-time SCADA data.
4. Log incidents, enable two-way customer communication and route to the right team.
5. Provide updates and context-aware engagement throughout the incident lifecycle, including WhatsApp and the website.

### 2.2 HOW — key architecture questions
- Which system detects the grid / SCADA outage?
- Can the SCADA system publish events?
- What are the expected event volume and latency?
- Where is the customer-to-grid-area relationship stored in legacy systems?
- How will customer identity be unified across systems?
- How will impacted individuals be derived from affected areas?
- Where are channel preferences and consent records stored?
- Which Marketing Cloud product fits the outbound messaging use case?
- Should there be one country-wide Incident or area-level Incidents?
- How should customer Cases relate to the parent Incident?
- What is the source of truth for incident status and restoration updates?
- How will inbound conversations be routed across WhatsApp and the website?
- How will Agentforce handle high-volume customer questions?
- What orchestration / transformation value does MuleSoft add?
- What are the budget, licensing, existing-product and team-capability constraints?

### 2.3 WHY — ADR / decision record
For each major decision, capture:
- Business requirement
- Question
- Assumption
- Constraint / platform limitation
- Selected solution
- Alternatives
- Rationale / trade-off
- Validation owner and next action

## 3. Proposed architecture from the episode

```text
Grid Sensors / Smart Meters
          |
          v
       SCADA
  - Grid status
  - Outage / restoration events
          |
          v
       MuleSoft
  - Event/API ingestion
  - Transformation
  - Orchestration across legacy systems
          |
          +-------------------------------+
          |                               |
          v                               v
 Legacy Systems                    Data 360
 Customer-to-area mapping          Unified Individual
 Account / service / asset data    Identity resolution
          |                               |
          +---------------+---------------+
                          |
                          v
                 Impacted individuals
                          |
                          v
            Marketing Cloud Advanced / Next
                  On-Demand Flow
                          |
             Personalized outbound messages
                 (Email / supported channels)

MuleSoft
   |
   +---- Create / Update ----> Salesforce Incident
                                  |
                                  +---- Related customer Cases
                                  |
                                  +---- Incident status / updates
                                  |
                                  v
                         Latest incident context
                                  |
                  +---------------+---------------+
                  |                               |
                  v                               v
          Agentforce Service Agent         Service Teams
          WhatsApp / Website               Omni-Channel / queues
                  |
                  v
       Customer two-way communication
```

This is the episode's logical pattern. A production architecture must separately design event transport, API security, retries, ordering, idempotency, identity quality, consent policy, channel-provider constraints, monitoring and disaster recovery.

## 4. Outage detection — SCADA + MuleSoft

### 4.1 SCADA's role
SCADA (Supervisory Control and Data Acquisition) is the control-system context used to collect operational telemetry and status from grid / industrial systems. The episode assumes:
- Grid sensors / devices send status to SCADA.
- SCADA can detect an outage.
- SCADA can publish status events.
- Events can be high-volume and low-latency.

### 4.2 MuleSoft's role
MuleSoft is used as the integration layer between SCADA and Salesforce / other clouds.

Expected responsibilities:
- Receive SCADA events.
- Retrieve additional information from legacy systems.
- Transform a raw technical event into business context.
- Map a transformer / feeder / grid area to an impacted service area.
- Orchestrate the systems needed to calculate affected customers.
- Create / update Salesforce Incidents.
- Invoke Marketing Cloud On-Demand Flows.

**Important:** MuleSoft is the example selected in the episode, but the presenter explicitly says another middleware could be used depending on the conditions.

### 4.3 Example event
```json
{
  "eventId": "GRID-OUTAGE-2026-00042",
  "eventType": "OUTAGE_DETECTED",
  "gridAssetId": "TRANSFORMER-778",
  "areaCode": "AREA-12",
  "occurredAt": "2026-11-05T10:30:00Z",
  "severity": "HIGH",
  "status": "OPEN"
}
```

This is an illustrative schema, not a payload supplied by the episode.

## 5. Identifying affected customers — legacy systems + Data 360

### 5.1 Episode assumption
The presenter assumes:
- Data has already been preprocessed / unified to some degree.
- A Unified Individual / unified customer identity is available.
- Legacy systems maintain the relationship between customers and impacted grid areas.
- MuleSoft can retrieve the relevant customer / party list based on affected areas.

If unified identity is not already available, building it from scratch during an outage is not a safe assumption. Identity preparation is treated as a pre-incident prerequisite.

### 5.2 Data 360's role
The episode uses Data 360 to locate unified customer identities:
- Correlate customer identity across multiple data sources.
- Relate legacy-system records to a unified individual.
- Support identification of customers in affected areas.

The transcript mentions zero-copy or MuleSoft as possible approaches to data unification, but it does not specify a detailed implementation.

### 5.3 Customer-to-area mapping
Identity and network relationship are two different data problems:
- **Identity resolution:** do multiple source records represent the same person?
- **Service-area mapping:** which grid area is connected to the person's service account / premise / meter?

A Unified Individual does not automatically provide the grid area. Both data domains need a reliable relationship.

## 6. Personalized notifications — Marketing Cloud Advanced / On-Demand Flow

### 6.1 Episode decision
The presenter explores Marketing Cloud Advanced (MCA) for time-sensitive, high-volume personalized outbound communication. On-Demand Flow is described as a transactional / time-sensitive flow pattern invoked through a REST API.

High-level flow:
1. The outage event reaches MuleSoft.
2. MuleSoft retrieves affected areas / parties from legacy systems.
3. Data 360 helps identify the corresponding individuals.
4. MuleSoft invokes the Marketing Cloud On-Demand Flow.
5. The flow delivers personalized notifications.

### 6.2 Consent and channel preference — episode assumption
The episode initially treats outage notifications as non-promotional / service communications and assumes marketing consent is not needed. It also assumes channel preference is captured.

**Architecture caution:** This is not a universal legal conclusion. The legal basis for emergency / service notifications, opt-out rules, privacy obligations, telecom rules, WhatsApp policy and local regulations must be validated for the jurisdiction and message type. Do not treat channel preference, contactability, legal basis and channel-specific opt-in as the same field.

### 6.3 WhatsApp capability — limitation at the time of the episode
At the time of recording, the presenter says WhatsApp was not yet supported by On-Demand Flow and was expected in a few weeks. As a workaround for the then-current state, she describes using a Data Cloud Action to trigger a Flow.

This is a statement about the feature status at the time of the episode. The current documented state is checked separately in Section 16.

## 7. Incident Management — Incident + Cases

### 7.1 Episode pattern
The presenter wants to leverage out-of-the-box Incident Management.

Proposed record model:
- Create or update an Incident for each affected area / outage situation.
- Consider area-level Incidents rather than one enormous Incident for a country-wide outage.
- An overarching incident / parent context may also exist.
- When a customer contacts support, create a Case for that interaction.
- Relate multiple customer Cases to the affected Incident.

### 7.2 Incident vs Case
| Record | Purpose |
|---|---|
| Incident | Tracks a widespread disruption affecting multiple customers / teams |
| Case | Tracks an individual customer's question, complaint, request or interaction |
| Relationship | Many Cases can relate to one Incident |

### 7.3 Create vs update an Incident
The MuleSoft interface needs to support both operations:
- New outage / new area impact → create an Incident.
- Update to an existing outage / restoration status → update the related Incident.

The latest incident status must be available to Agentforce and notification flows.

### 7.4 Why area-level Incidents?
The Q&A raises the concern of data skew / account locking. If customers across an entire country update one Incident / related record / parent record, hot-record contention or skew may occur.

Area-level partitioning can:
- Reduce record contention.
- Localize operations.
- Support regional team ownership.
- Simplify customer-to-incident mapping.

The partitioning model must still be validated against business incident hierarchy, reporting and cross-area dependencies.

## 8. Agentforce — inbound customer support

### 8.1 Episode design
A major outage can cause many customers to ask questions simultaneously. Agentforce is proposed to handle inbound volume.

Agentforce should:
- Access the latest Incident information.
- Respond using customer context.
- Have an action to create a Case for the interaction.
- Be available on WhatsApp and the website / Experience channel.
- Route complex queries to the correct service team.

### 8.2 Agent actions
The episode explicitly calls out a Case-creation action. Practical actions include:
- Get latest incident status for a service area.
- Get estimated restoration information (only if a trusted source provides it).
- Create a Case linked to the relevant Incident.
- Update / append customer interaction context.
- Route to a service team / human agent.
- Provide a safe fallback when current incident data is unavailable.

### 8.3 Grounding / source of truth
The agent should be grounded in the latest Incident data. Static prompt text should not be the source of truth for outage / restoration status.

If an estimated restoration time is unavailable, the agent must not invent one. It should say that the latest confirmed information is unavailable and offer escalation.

## 9. WhatsApp vs Digital Engagement — episode discussion

The episode asks why WhatsApp is being used instead of Salesforce Digital Engagement.

The expert's answer:
- The use case includes large-scale, time-sensitive outbound notification broadcasts.
- On-Demand Flow / Marketing Cloud is discussed for personalized outbound notifications to the affected population.
- After the message is sent, a WhatsApp messaging session can be created and the customer can reply.
- Digital Engagement / service messaging is relevant for inbound two-way support, agent routing and conversations.

**Architecture takeaway:** Outbound mass notification and inbound service conversation are different workloads. Do not conflate them under one product-feature label. A unified channel design may be possible, but outbound orchestration, templates, opt-in, inbound routing, session ownership and agent handoff must be configured and validated separately.

## 10. Why MuleSoft? Why not call Marketing Cloud directly?

The experts' arguments:
- A SCADA event may contain only a technical asset ID; identifying the affected customer area may require querying multiple legacy systems.
- MuleSoft orchestration can combine data from multiple systems.
- Data transformation can convert payloads into the format expected by the destination system.
- The presence of legacy systems makes middleware / an integration layer valuable.
- If the landscape were purely Salesforce-based, simpler alternatives could be considered.

**Decision rule:** Do not add MuleSoft merely as a pass-through proxy. Justify it when orchestration, transformation, API management, reuse, governance or legacy integration creates real value.

## 11. Budget, existing products and team skills

### 11.1 Budget
High-level design should understand business throughput / performance needs. Detailed licensing, API limits, token usage, capacity and operating costs can be quantified during subsequent design and validation.

Episode speakers' points:
- Architects should not ignore budget.
- In real projects, licensing decisions may already have been made before the architect joins.
- Agentforce token consumption and product costs become important in later design stages.
- Consider products the customer already owns; avoid unnecessary license purchases for one use case.

### 11.2 Existing customer stack
Check:
- Marketing Cloud Engagement / ExactTarget already present?
- Marketing Cloud Advanced / Marketing Cloud Next entitlement?
- Data 360 availability?
- MuleSoft licenses / runtime already deployed?
- Service Cloud / Incident Management capability?
- WhatsApp business number, provider and channel setup?
- Existing identity / consent data?
- Existing integration / event infrastructure?

The episode explicitly says MCA is not the only possible route. The existing Marketing Cloud Engagement platform could also be evaluated for similar outcomes.

### 11.3 Team skills
The episode's discussion:
- Do not automatically constrain the best architecture to current team skills.
- Required specialist skills can be hired, trained or brought in.
- Long-term customer support capability matters.
- Low-code tools such as Flow can be adopted by business architects.
- Architects should understand their knowledge gaps and involve domain experts.

## 12. Data flows and bidirectional integration

An audience question asks whether all integration arrows are bidirectional.

The presenter clarifies that the diagram was simplified. Not every integration is bidirectional. There may be an acknowledgement / status response, but that does not mean full business-data synchronization occurs in both directions.

**Best practice:** Document each interface separately:
- Source → target direction
- Request / response
- Event acknowledgement
- Business status callback
- Data ownership
- Retry and replay
- Error response
- Correlation ID

## 13. Risks and architectural considerations

1. **Data skew / hot records:** one country-wide Incident or common Account may create lock contention under extreme updates.
2. **Identity mismatch:** duplicate / incorrect identity can send outage alerts to the wrong customers.
3. **Stale grid mapping:** outdated customer-to-meter / premise / area mapping can produce an incorrect affected population.
4. **Duplicate notifications:** event replay / retry can send the same alert multiple times.
5. **Out-of-order events:** a restoration event processed before an earlier outage event can produce an incorrect Incident status.
6. **Consent / compliance:** validate the legal basis and channel-specific rules for emergency communications.
7. **WhatsApp template / session policy:** validate outbound initiation and customer-reply rules against provider policy.
8. **Agent hallucination:** do not invent unconfirmed restoration times or service status.
9. **Integration outage:** design retries / fallback for MuleSoft, SCADA, Data 360, Marketing Cloud or Salesforce failure.
10. **Peak burst:** outbound notifications and inbound queries can spike simultaneously after an outage.
11. **Observability:** monitor event-to-notification trace, delivery status, failed customer resolution and Case creation.
12. **Privacy:** expose customer location, service address, phone number and outage details on a least-privilege basis.

## 14. Interview-ready architecture walkthrough

**Question: Design a high-scale multicloud incident journey for a national electricity outage.**

“First, I would decompose the requirements into outage detection, impacted-customer identification, personalized outbound notifications, incident and case management, and inbound two-way support.

I would assume the grid monitoring / SCADA platform detects outages and can publish events, but I would validate its event interface, volume and latency. MuleSoft would ingest and normalize those events and orchestrate legacy lookups to map grid assets and affected areas to service accounts and customers.

Data 360 can help unify customer identities across sources, provided identity resolution and customer-to-service-area relationships are prepared and current before the outage. MuleSoft can then invoke an API-triggered Marketing Cloud flow to send personalized notifications through supported channels, subject to legal basis, channel consent and WhatsApp template rules.

For service operations, I would create or update an Incident per appropriate outage / area and relate customer Cases to it. Agentforce would be grounded in the latest incident status and exposed on the website and WhatsApp, with actions to create Cases and hand off complex issues to the right team.

Finally, I would design idempotency, event ordering, retries, dead-letter handling, data-skew controls, delivery tracking, privacy controls and end-to-end observability. I would choose MuleSoft and the marketing product based on the existing customer stack, integration complexity, latency, scale and total cost.”

## 15. Interview Q&A — based on the episode

### Q1. Why MuleSoft?
Because SCADA and legacy systems may require orchestration, transformation and aggregation before Salesforce or Marketing Cloud receives the data. If it is only a direct Salesforce-to-Salesforce flow, alternatives may be simpler.

### Q2. Why Data 360?
To support unified customer identity across legacy / Salesforce data sources. Identity resolution does not itself create the customer-to-grid-area relationship; that mapping must also exist.

### Q3. Why Marketing Cloud Advanced On-Demand Flow?
The episode uses it for API-triggered, personalized, time-sensitive outbound communication at scale. Current channel support and entitlements must be validated.

### Q4. Why not use only Digital Engagement?
The episode distinguishes large outbound notification broadcasts from inbound two-way service conversations. Digital Engagement / Messaging is useful for handling customer replies and agent routing.

### Q5. Why both Incident and Case?
An Incident represents a widespread disruption affecting many customers. A Case represents an individual customer's interaction. Many Cases can relate to one Incident.

### Q6. Why create an Incident per area?
To provide operational granularity and reduce the risk of hot-record / data-skew contention that could arise if all updates target one record.

### Q7. How does Agentforce get current outage information?
Ground the agent in the latest authoritative Incident / outage data and expose controlled actions. Do not rely only on static prompt content.

### Q8. Is consent always unnecessary for outage alerts?
No. The presenter makes an episode assumption that outage messages are service notifications. Legal basis, opt-out rules, privacy obligations and WhatsApp policy must be checked for the jurisdiction and message type.

### Q9. Are all integration arrows bidirectional?
No. The presenter explicitly says the diagram was simplified. Each interface's direction, acknowledgement and business callback must be documented separately.

### Q10. Should an architect consider budget during HLD?
Yes, at least as a design constraint and product-selection input. Detailed license, API, token and operating-cost estimates can be refined as the design progresses.

### Q11. Should architecture be constrained by current team skills?
Not automatically. Bring in or develop skills where needed, while considering long-term supportability and the customer's operating model.

### Q12. What if the customer already owns Marketing Cloud Engagement?
Evaluate it. The episode says MCA is not the only possible product and the existing ExactTarget / Marketing Cloud Engagement platform may support similar outcomes.

### Q13. What is the output of HLD?
It depends on the customer and audience: architecture diagram, slide deck, formal design document, decision log, Q&A, workshop outputs or a combination. There should be a usable artifact for review and future reference.

## 16. 2026 Salesforce Documentation Check — episode claims vs current state

> These are additional current-documentation checks, not statements made in the episode. Product names, feature availability, editions and entitlements can change. Confirm the target org, region, licenses and release before implementation.

| Topic | Episode statement | 2026 documentation check / correction |
|---|---|---|
| On-Demand Flow | API-triggered high-scale transactional flow in Marketing Cloud Advanced. | Current Salesforce Help documents On-Demand Flows as REST API-triggered flows available with Marketing Cloud Growth or Advanced, and on supported Data 360 editions. A documented invocation pattern is `POST /services/data/v65.0/actions/custom/flow/FLOW_API_NAME`. Check required access for the triggering user and OAuth/API permissions. |
| WhatsApp in On-Demand Flow | At episode recording time, WhatsApp support was not yet available; the presenter expected it in a few weeks and described a Data Cloud Action workaround. | Current Salesforce Help lists the Send WhatsApp Message Flow element as available in automation event-triggered, broadcast, on-demand and segment-triggered flows for Marketing Cloud Growth / Advanced editions. Therefore, the episode-time limitation is no longer current in the documented feature set. Confirm exact org entitlement, Unified Messaging setup, WhatsApp templates/session-message content, sender and communication subscription / opt-in. |
| Marketing Cloud product naming | Presenter calls the product Marketing Cloud Advanced (MCA) and compares it with ExactTarget / Marketing Cloud Engagement. | Current Salesforce documentation uses Marketing Cloud Next and Marketing Cloud Growth / Advanced terminology in relevant docs, while Marketing Cloud Engagement remains a separate product. Do not treat these names as interchangeable; validate the specific SKU and feature set. |
| Data 360 identity resolution | Assumes a Unified Individual / unified customer identity is already available. | Data 360 identity resolution uses configured match and reconciliation rules. Salesforce Help states rulesets process source profiles and run at least once a day per ruleset (unless no source/config changes). Do not assume batch identity resolution automatically delivers sub-second identity updates during a grid outage. Precompute / maintain the needed identity and service-area mapping, or design a separate real-time lookup strategy. |
| Zero-copy | Mentioned as one possible way to access / unify data. | Zero-copy federation is not the same as identity resolution or a guaranteed real-time customer-to-grid-area join. Validate source support, query behavior, permissions, latency and freshness. |
| Incident vs Case | Episode uses an Incident per outage / area and customer Cases for two-way interaction. | Salesforce's current Cases and Incidents guidance describes one Incident to many Cases for widespread disruption. This supports the episode's conceptual model. Confirm the exact Customer Service Incident Management feature, required setup and licensing in the target org. |
| Incident record availability | Presenter says she wants to leverage out-of-the-box Incident Management. | Current Salesforce Help has Customer Service Incident Management sample flows and separately documents Agentforce IT Service incidents. These are distinct product contexts; do not confuse IT Service incident features / entitlements with customer-service incident management. |
| Agentforce on WhatsApp / website | Episode proposes Agentforce across WhatsApp and Experience Cloud / website. | Current Help documents Agentforce Service Agent routing for enhanced WhatsApp and deployment to Experience Cloud sites. Check supported editions, Digital Engagement / Messaging prerequisites, channel type, agent readiness, routing and fallback queue. |
| Consent / preference | Episode assumes outage alerts do not need consent, but channel preference is captured. | Treat this as a scenario assumption, not legal advice or a universal Salesforce rule. Current WhatsApp and marketing messaging configurations can include communication subscriptions / opt-in controls. Determine the lawful basis and channel policy with legal/compliance teams. |
| MuleSoft | Used for SCADA events, legacy orchestration and transformation. | This remains an architectural pattern rather than a mandatory Salesforce product choice. Choose based on integration complexity, existing investment, throughput, latency, operational skills and total cost. |

### Official Salesforce references checked
1. On-Demand Flows: https://help.salesforce.com/s/articleView?id=platform.flow_concepts_trigger_on_demand.htm&language=en_US&type=5
2. Send WhatsApp Message Flow Element: https://help.salesforce.com/s/articleView?id=platform.flow_ref_elements_mktg_send_whatsapp_message.htm&language=en_US&type=5
3. Send Transactional Messages with an On-Demand Flow: https://developer.salesforce.com/docs/marketing/marketing-cloud-growth/guide/mc-manage-transactional-messages-on-demand-flow.html
4. Data 360 Architecture: https://developer.salesforce.com/docs/data/data-cloud-dev/guide/dc-architecture.html
5. Identity Resolution Match Rules: https://help.salesforce.com/s/articleView?id=data.c360_a_match_rules.htm&language=en_US&type=5
6. Identity Resolution Terminology: https://help.salesforce.com/s/articleView?id=data.c360_a_identity_resolution_terms.htm&language=en_US&type=5
7. Cases and Incidents: https://help.salesforce.com/s/articleView?id=incident_mgmt_case_vs_incident.htm&language=en_US&type=5
8. Customer Service Incident Management Sample Flows: https://help.salesforce.com/s/articleView?id=service.incident_mgmt_sample_flows.htm&language=en_US&type=5
9. Enhanced WhatsApp Channel: https://help.salesforce.com/s/articleView?id=service.messaging_whatsapp_enhanced.htm&language=en_US&type=5
10. Route Messages Directly to Agentforce: https://help.salesforce.com/s/articleView?id=service.messaging_route_to_agentforce.htm&language=en_US&type=5
11. Deploy Agent to Experience Cloud Site: https://help.salesforce.com/s/articleView?id=service.support_deploy_the_agent_to_your_experience_cloud_site.htm&language=en_US&type=5
12. Unified WhatsApp routing options: https://help.salesforce.com/s/articleView?id=mktg.um_default_cloud.htm&language=en_US&type=5

## 17. Additional Points Added — recommended production design

### 17.1 Event reliability
- Use a unique event ID and correlation ID end to end.
- Make Incident create/update idempotent.
- Store event sequence / source timestamp to reject stale updates.
- Handle duplicate, delayed and out-of-order events.
- Retry transient failures with backoff.
- Send poison messages to a dead-letter / quarantine path.
- Periodically reconcile SCADA current state against Salesforce Incident state.

### 17.2 Notification reliability
- Create a durable notification request per affected individual and outage.
- Use a deduplication key such as `outageId + individualId + notificationType`.
- Track queued, submitted, delivered, failed and suppressed states.
- Respect channel-specific template, opt-in, quiet-hour and opt-out requirements.
- Do not send a restoration message unless an authoritative restoration event is confirmed.

### 17.3 Suggested data model
- GridAsset / Transformer
- ServiceArea
- Premise / ServicePoint
- ServiceAccount
- Individual / Contact
- OutageEvent
- Incident
- Case
- NotificationRequest
- NotificationDelivery
- CustomerChannelPreference

These are suggested conceptual entities, not a claim that every one is a standard Salesforce object.

### 17.4 Security
- Authenticate SCADA-to-MuleSoft integration using an approved machine identity.
- Use OAuth / Named Credentials or a supported secure integration mechanism for downstream APIs.
- Avoid exposing grid topology / sensitive infrastructure data to public agents.
- Agent actions should return only customer-authorized incident information.
- Restrict customer data to the minimum necessary fields.
- Audit changes to incident severity, restoration status and customer communication.

### 17.5 Observability KPIs
- SCADA event-to-MuleSoft ingestion latency.
- Event-to-Incident create/update latency.
- Affected-party identification accuracy.
- Event-to-notification submission latency.
- Notification delivery / failure rate by channel.
- Duplicate notification rate.
- Agent containment / escalation rate.
- Case creation success rate.
- Incident update freshness.
- Backlog depth and retry count.

### 17.6 Failure-mode table
| Failure | Expected behavior |
|---|---|
| SCADA event source unavailable | Alert operations; do not fabricate outage status; use approved fallback telemetry |
| MuleSoft unavailable | Durable upstream buffering / replay; preserve event IDs |
| Data 360 identity lookup stale | Use prevalidated service-area mapping or controlled fallback; flag uncertainty |
| Marketing Cloud send failure | Retry safely; deduplicate; expose delivery state |
| WhatsApp unavailable | Use an approved alternate channel if preference / policy allows |
| Agentforce unavailable | Static status page / human support / queue fallback |
| Incident update out of order | Compare source sequence / timestamps; reject stale update |
| Central Salesforce outage | Queue events and notifications; recover and reconcile after restoration |

## 18. Illustrative implementation patterns (not production-ready code)

### Python — idempotent event handler concept
```python
def handle_outage_event(event, store, incident_api):
    event_id = event["eventId"]

    if store.was_processed(event_id):
        return {"status": "duplicate_ignored"}

    incident_key = f'{event["gridAssetId"]}:{event["areaCode"]}'

    if event["eventType"] == "OUTAGE_DETECTED":
        incident_api.create_or_update(
            key=incident_key,
            status="Open",
            source_event_id=event_id
        )
    elif event["eventType"] == "RESTORATION_CONFIRMED":
        incident_api.create_or_update(
            key=incident_key,
            status="Restored",
            source_event_id=event_id
        )

    store.mark_processed(event_id)
    return {"status": "processed"}
```

### JavaScript — invoke an API-triggered flow concept
```javascript
async function triggerNotificationFlow(accessToken, instanceUrl, flowApiName, payload) {
  const endpoint =
    `${instanceUrl}/services/data/v65.0/actions/custom/flow/${flowApiName}`;

  const response = await fetch(endpoint, {
    method: "POST",
    headers: {
      "Authorization": `Bearer ${accessToken}`,
      "Content-Type": "application/json"
    },
    body: JSON.stringify(payload)
  });

  if (!response.ok) {
    throw new Error(`Flow invocation failed: ${response.status}`);
  }

  return await response.json();
}
```

### Java — event consumer concept
```java
public void process(OutageEvent event) {
    if (processedEventRepository.exists(event.getEventId())) {
        return; // idempotency guard
    }

    IncidentUpdate update = incidentMapper.toUpdate(event);
    incidentService.createOrUpdate(update);

    processedEventRepository.markProcessed(event.getEventId());
}
```

### TypeScript — typed outage event
```typescript
type OutageEvent = {
  eventId: string;
  eventType: "OUTAGE_DETECTED" | "OUTAGE_UPDATED" | "RESTORATION_CONFIRMED";
  gridAssetId: string;
  areaCode: string;
  occurredAt: string;
  sequenceNumber: number;
  severity: "LOW" | "MEDIUM" | "HIGH" | "CRITICAL";
};
```

> These snippets are conceptual patterns. Production implementation needs authentication, schema validation, durable transactions / outbox, retry policy, secrets management, API-version governance and vendor-specific SDKs. The Salesforce On-Demand Flow endpoint and inputs must match the active org's flow definition.

## 19. Quick revision summary

- WHAT: national grid outage, affected customers, real-time SCADA data, personalized alerts, incident logging, two-way channels and lifecycle updates.
- HOW: SCADA → MuleSoft → legacy data + Data 360 → impacted individuals → Marketing Cloud On-Demand Flow; MuleSoft also creates/updates Incidents; Cases link customer conversations; Agentforce supports WhatsApp / website.
- WHY: integration orchestration, identity unification, rapid outbound communication, coordinated incident handling and scalable inbound support.
- Main design trade-offs: MCA vs existing MCE, MuleSoft vs simpler integration, one national Incident vs area-level Incidents, outbound broadcast vs inbound Digital Engagement.
- 2026 correction: the WhatsApp message action is now documented as supported in On-Demand Flows for eligible Marketing Cloud Growth / Advanced editions; the episode-time limitation is historical.
- Key caution: Data 360 identity resolution is ruleset-driven and is not automatically a sub-second outage-time identity lookup.

## 20. No-Skip Checklist

- [ ] Opening severe-weather / grid-outage scenario
- [ ] Presenters and their expertise
- [ ] Episode agenda and WHAT–HOW–WHY
- [ ] Original business requirement
- [ ] All five high-level requirements
- [ ] SCADA detection assumptions
- [ ] Grid sensors / status events
- [ ] High-volume and low-latency assumptions
- [ ] MuleSoft integration choice
- [ ] Legacy systems and customer-to-area mapping
- [ ] Data 360 / Unified Individual assumption
- [ ] Zero-copy vs MuleSoft unification mention
- [ ] Affected-party identification
- [ ] Consent and channel-preference assumptions
- [ ] Marketing Cloud product selection discussion
- [ ] MCA / On-Demand Flow
- [ ] Episode-time WhatsApp limitation and Data Cloud Action workaround
- [ ] Incident Management / area-level Incident
- [ ] Parent Incident and customer Cases
- [ ] Incident create vs update
- [ ] Latest incident-status requirement
- [ ] Agentforce inbound volume
- [ ] Case-creation action
- [ ] WhatsApp and Experience Cloud channels
- [ ] On-Demand Flow explanation from expert
- [ ] Assumption vs question discussion
- [ ] Budget and token-cost discussion
- [ ] Team skills and low-code discussion
- [ ] Bidirectional integration clarification
- [ ] Data skew / account-locking concern
- [ ] WhatsApp vs Digital Engagement discussion
- [ ] MuleSoft vs direct / broadcast Flow discussion
- [ ] Marketing Cloud Engagement / ExactTarget alternative
- [ ] Existing products and licensing discussion
- [ ] Incident Management readiness
- [ ] HLD deliverables discussion
- [ ] Architect knowledge breadth and use of experts
- [ ] Current 2026 documentation checks
- [ ] Additional reliability / security recommendations
- [ ] Full original transcript appended verbatim

## 21. Board-Level Answer

For a national grid outage, SCADA detects the disruption and publishes events to an integration layer. MuleSoft enriches and transforms those events using legacy customer-to-service-area relationships. Data 360 supports customer identity unification, provided identity and service-area mappings are prepared ahead of the incident. The integration layer invokes an API-triggered Marketing Cloud flow for personalized outbound alerts. Salesforce Incident Management tracks each outage / affected area, while customer Cases capture individual interactions. Agentforce, exposed through supported web and WhatsApp channels, uses current incident data to answer questions, create Cases and escalate to service teams. Reliability depends on idempotent event processing, ordering, retries, dead-letter handling, data-skew controls, consent controls and end-to-end observability.

---

## Original Transcript — Preserved Verbatim

The transcript below is appended exactly as supplied in the uploaded text file. Apparent transcription errors, repeated words, incomplete final phrase and original wording are intentionally retained.

