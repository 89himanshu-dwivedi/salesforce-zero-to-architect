
# Think Like an Architect — Scaling Salesforce for the Big Game
## Episode Notes | English | Source-preserving edition

> **Source boundary:** The “Original Transcript — Preserved Verbatim” section contains the uploaded transcript appended as supplied. The structured notes above it are explanatory organization. Current Salesforce documentation checks are separated in Section 16.

## 1. Episode objective

This episode designs a Salesforce architecture for a high-burst consumer event: a televised prime-time guessing game.

### Business scenario — source figures
- Duration: **3 weeks**
- Total registrations: **8 million**
- Peak concurrent users: **1 million**
- Channel: **Experience Cloud**
- Eligible geography: **United States and Canada**
- Agent use case: a website conversational agent answers questions about rules and provides hints.
- Safety requirement: real-time toxicity detection for player and agent language.
- Availability: zero-downtime objective.
- Non-functional requirements: scale testing, load balancing and surge handling.

The architect's first task is to understand the requirements, not to select products prematurely.

## 2. The WHAT–HOW–WHY architecture method

| Step | Meaning | Expected output |
|---|---|---|
| WHAT | Understand the business requirement, identify key phrases and translate them into high-level requirements (HLRs). | Clear scope, constraints, scale, geography and availability targets. |
| HOW | Align solution components to requirements; diagram the design; record questions, assumptions, limitations and decisions. | Architecture diagram, assumptions and design decisions. |
| WHY | Justify each decision using the questions, assumptions and limitations identified during design. | Architecture Decision Records (ADRs) and stakeholder-ready rationale. |

### 2.1 WHAT — requirement decomposition
The presenter grouped the business statement into four HLR areas:

1. Support 8M total registrations and 1M concurrent users on Experience Cloud, limited to the US and Canada.
2. Provide a conversational agent to answer rules questions and offer hints.
3. Detect toxicity in real time.
4. Meet a zero-downtime objective with scale testing and load balancing.

Toxicity detection was grouped with the conversational-agent requirement. Load balancing was grouped with the traffic-scale requirement.

### 2.2 HOW — design questions
The architect asks:
- Should the design use one org or multiple orgs?
- Which Experience Cloud license model fits the access and sharing requirements?
- How will registration bursts be absorbed?
- How will US/Canada eligibility be checked?
- How will static content be served with low latency?
- How will a waiting room smooth traffic?
- How will a new user be assigned to an org?
- How will a returning user be routed to the same org?
- Where will rules, hints and conversation history live?
- How will agent safety and toxicity be addressed?
- How will scale testing be performed, and at what load?
- How will findings be observed and optimized?
- Which environment is appropriate for testing?

### 2.3 WHY — decision record
For each decision, capture:
- Requirement / business driver
- Question that triggered the decision
- Assumption
- Platform or product limitation
- Selected option
- Alternatives considered
- Rationale
- Risks and required validation

## 3. Proposed high-level architecture (episode pattern)

```text
Players (US / Canada)
        |
        v
CDN / Edge Protection
  - Geo/IP eligibility checks
  - Static content caching
  - Bot / threat filtering
  - Waiting room / traffic shaping
        |
        v
Pre-login routing layer (AWS in episode)
  - New-user round-robin shard assignment
  - Returning-user lookup
  - Shard registration store (DynamoDB in episode)
        |
        +-------------------+-------------------+
        |                   |                   |
        v                   v                   v
   Salesforce Org 1    Salesforce Org 2    Salesforce Org N
   Experience Cloud    Experience Cloud    Experience Cloud
   Registration        Registration        Registration
   User/session data   User/session data   User/session data
   Conversation hist.  Conversation hist.  Conversation hist.
        |                   |                   |
        +-------------------+-------------------+
                            |
                            v
                  Agentforce Service Agent
                  - Rules and hints grounding
                  - Prompt template deployed consistently
                  - Einstein Trust Layer controls
                            |
                            v
                    LLM gateway / provider

Org events/data
      |
      v
Aggregation / integration layer (MuleSoft example)
      |
      v
Central repository / analytics
      |
      v
Tableau / CRM leaderboard dashboards
```

This is a conceptual pattern. A production design must separately define identity, API authentication, trust boundaries, retries, idempotency, monitoring, data residency, disaster recovery and vendor contracts.

## 4. Org strategy, licensing and sharding

### 4.1 The episode decision
The presenter selected a multi-org strategy because the scenario involves very high concurrency, a large user population and burst registration. The episode emphasizes that Experience Cloud license types have different scalability and sharing implications.

The transcript uses “CC license” / legacy “Community Cloud” terminology. Do not treat that as a definitive current SKU recommendation. Validate the license against the actual access model, sharing requirements, user volume, roles, entitlements and Salesforce commercial agreement.

### 4.2 Why multi-org?
- Distribute user load across multiple orgs.
- Split registration processing into shards.
- Assign each user to one org.
- Route returning users back to their original org.
- Isolate per-org capacity and limits where appropriate.

### 4.3 Shard registration flow

**New user**
1. The user enters through the CDN / waiting room.
2. The pre-login routing layer performs eligibility and admission checks.
3. The routing service selects an available shard (round robin in the episode).
4. The user-to-org mapping is persisted (DynamoDB in the episode).
5. Registration occurs in the selected Salesforce org.

**Returning user**
1. The user returns to the website.
2. The routing layer looks up the shard mapping using a stable identity key.
3. The user is routed to the Salesforce org where the account/user was registered.

**Design note:** Round robin is simple, but production designs should consider shard capacity, health, queue depth, tenant affinity, failover and hot-shard protection. Mapping writes should be atomic and idempotent.

### 4.4 Multi-org governance
Multi-org is not merely traffic splitting. It introduces:
- Consistent metadata and configuration across orgs.
- Consistent Experience Cloud definitions and agent configuration.
- Packaging and CI/CD automation to prevent version drift.
- Org provisioning, monitoring, administration and release governance.
- Cross-org reporting / leaderboard aggregation.
- Clear identity mapping to prevent one user from being duplicated across orgs.

In the short-lived game described in the episode, users were unique to one org and there was no cross-org user-data synchronization. A long-lived enterprise design has a larger governance and operations burden.

## 5. CDN, waiting room and registration protection

### 5.1 CDN responsibilities in the episode
- US/Canada IP-based eligibility screening.
- Edge caching for static content such as rules and terms.
- Preventing all users from hitting Salesforce orgs at the same time.
- Releasing users at a controlled rate through a waiting room.
- Filtering bot / threat traffic before it reaches origin systems.

### 5.2 Business value of a waiting room
A waiting room is a graceful-degradation and traffic-shaping pattern. It can communicate admission status or an expected wait. A controlled wait can provide a better experience than widespread errors, timeouts and refresh storms.

### 5.3 Anti-bot and human-verification layers discussed
- CDN / edge threat detection.
- Email registration and a welcome link.
- CAPTCHA (the transcript's “capture” is likely a speech-recognition error).
- Email OTP.
- An additional CDN layer in front of Experience Cloud.
- AWS built-in threat detection / request-rate protections.
- Upload of verifiable identity documents, such as a driver's license — an example provided by an episode expert.
- Multi-factor authentication.

**Important:** CAPTCHA, OTP, document checks and IP filtering are different signals. None alone proves that a user is human. Use layered, risk-based controls with privacy and legal review and fraud monitoring.

### 5.4 Geographic restriction caution
IP geolocation is approximate. VPNs, proxies, mobile-carrier routing and corporate egress can create false positives and false negatives. If legal eligibility is strict, do not use IP checks as the sole proof; combine them with an approved identity / residency policy.

## 6. Making registration asynchronous

In the Q&A, the experts describe using Platform Events for an asynchronous registration pattern rather than creating users synchronously in the immediate request path. This can separate heavy user-creation work from the front-door interaction.

### Conceptual flow
1. The user completes pre-registration / eligibility.
2. The registration request is accepted.
3. An event is published.
4. A subscriber creates the Salesforce user and related records in the background.
5. Status is updated and the user receives progress / completion information.

### Platform Events engineering considerations
- Event-driven decoupling is useful, but asynchronous processing does not guarantee immediate completion.
- Design idempotent consumers and deduplication for at-least-once delivery semantics.
- Verify retry, dead-letter / failed-event handling, replay strategy, ordering, monitoring and event allocations.
- Do not equate publish acknowledgement with downstream business completion.
- An idempotency key on the registration API helps prevent duplicate users on retries.

The team had considered Platform Cache but selected Platform Events in the episode, citing familiarity with the concurrency/event-driven registration pattern and the short implementation timeline. This is not a universal rule: cache and events solve different problems.

## 7. Agentforce conversational agent

### 7.1 Episode design
- An Agentforce Service Agent was considered for the Experience Cloud scenario.
- The agent answers questions about rules and provides hints.
- Conversation history stays in the user's assigned org.
- Rules and hints must be consistent across all orgs.
- Given the assumption that the content is not a very large document corpus, deploying the same prompt template to all orgs was considered.
- The Einstein Trust Layer was included for safe LLM interactions / toxicity controls.

### 7.2 Multi-org knowledge strategy
The episode assumes rules and hints are not thousands of pages long, so deploying the same prompt template / configuration to each org may be a lightweight option.

If content is large, frequently updated, multilingual or centrally governed:
- Evaluate a centralized knowledge / retrieval source.
- Define content versioning and effective dates.
- Separate shared public game rules from org-specific user context.
- Automate prompt / agent version parity across orgs.
- Define cache invalidation and content rollout.

### 7.3 Conversation history
Keeping conversation history in the user's assigned org fits the episode's user-affinity model. Retention, privacy, deletion, audit, cross-org analytics and support-access policies still need to be defined.

### 7.4 Toxicity detection — both directions
The requirement covers language from both players and the agent. Explicitly distinguish:
- **Input moderation:** assess the player's message before the agent processes it.
- **Output moderation:** assess the agent response before it is sent to the user.
- Escalation / blocking / safe fallback behavior.
- Audit and review process.

Built-in Einstein Trust Layer controls can help, but do not interpret the episode's “out of the box” statement as proof that the complete custom game-moderation requirement is automatically covered. Validate current edition, feature status, configuration, input-versus-output coverage and channel support.

## 8. Zero downtime, resilience and LLM failover

The Q&A asks whether a standby conversational agent using another LLM vendor should be available for failover. The expert says Salesforce's LLM Gateway supports multiple LLM vendors and includes load balancing and failover infrastructure.

Architecture lessons:
- Verify platform-managed failover against current product documentation and contract.
- Define agent-level fallback, graceful degradation and user-facing error behavior.
- Test provider outage, rate limiting, latency spikes, token exhaustion and network failure.
- Do not promise “zero downtime” as an absolute. Define measurable availability SLOs, RTO/RPO, graceful degradation and incident processes.

## 9. Scale testing: Scale Test, Scale Center and ApexGuru

### 9.1 Episode guidance
- The experts recommend scale testing at 3x expected scale.
- Use Scale Test to validate production-like load.
- Use Scale Center to observe and analyze performance findings.
- Use ApexGuru for Apex optimization recommendations.
- Use a Full Copy Sandbox in the environment strategy.
- Test the end-to-end stack: CDN, AWS, Salesforce application layer, database and LLM Gateway.

### 9.2 Interpreting 3x / 4x / 5x
The transcript gives 3x, 4x and 5x examples. These are expert recommendations / scenario guidance, not universal Salesforce contractual requirements. Justify the target using workload model, expected peak RPS, concurrency, think time, payload mix, agent-call rate, burst profile and business risk.

### 9.3 Testing layers
1. Functional tests at normal volume.
2. Baseline / performance tests.
3. Load tests at expected peak.
4. Stress tests beyond expected peak.
5. Spike tests for sudden bursts.
6. Soak / endurance tests for sustained load.
7. Failover / resilience tests.
8. End-to-end tests across every external and internal hop.

### 9.4 Scale Test vs Scale Center vs ApexGuru
| Tool | Role in the episode / current documentation |
|---|---|
| Scale Test | Production-peak / high-RPS testing capability. Salesforce Help says it is a paid add-on for Full Sandbox customers and recommends it when expected peak production traffic exceeds 120 RPS. |
| Scale Center | Org performance metrics and analysis. Salesforce Help describes it as free for supported orgs / environments. |
| ApexGuru | Apex code insights and antipattern / optimization recommendations. Check current edition, environment and entitlement availability. |

### 9.5 Cost planning
Scale testing can involve product / sandbox costs and LLM consumption costs. The architect should:
- Estimate testing scope and duration.
- Forecast LLM call volume / token consumption.
- Obtain stakeholder approval.
- Compare test costs with production downtime / lost-revenue risk.
- Allocate a focused final scale-test window.
- Budget for retesting before future major releases / peak events.

An expert gives 3–4 days of focused scale testing as a practical example; this is not a mandatory fixed duration.

## 10. Leaderboard, analytics and cross-org aggregation

A multi-org design needs an aggregation approach for a global leaderboard.

The episode discusses:
- MuleSoft as an integration layer between orgs.
- Aggregating data into a central repository / central org.
- An example of hourly extraction / aggregation.
- Tableau / CRM dashboards for the leaderboard.
- Exposing the dashboard to internal teams or players.

### Design decisions
- Real-time or near-real-time leaderboard?
- Aggregation interval and acceptable staleness?
- Duplicate events / late-arriving data?
- Global rank calculation and tie-breaking?
- PII minimization and player privacy?
- Fallback if central aggregation fails?
- Prevention of public leaderboard abuse / gaming?

The episode raises a question about Data Cloud / “Data Cloud One,” but the concrete example provided by the expert is MuleSoft + central repository + Tableau/CRM dashboard. The transcript does not establish a detailed Data Cloud One implementation.

## 11. Other org-sharding use cases mentioned
- COVID vaccination appointment portals: staged eligibility opening, waiting rooms, CDN and org sharding to manage sudden public demand.
- High-demand FIFA World Cup registration: waiting room to smooth launch traffic.
- Other high-burst launches: ticketing, registration, appointment booking and major sales events.

## 12. Interview-ready architecture walkthrough

**Question: How would you design Salesforce Experience Cloud for 8M registrations and 1M concurrent users?**

“First, I would clarify total registered users versus peak concurrent users, request rate, user journey, data-access model, geography, registration burst, agent interaction rate and availability SLO. I would not jump directly to multi-org.

At the edge, I would use CDN/WAF capabilities for static content caching, bot protection and controlled admission through a waiting room. A pre-login routing service would assign new users to a shard and persist user-to-org affinity; returning users would be routed to their existing org. Registration-heavy work could be accepted asynchronously using Platform Events or another suitable queue/event mechanism, with idempotency, retries and monitoring.

Within each org, Experience Cloud licensing and sharing must match the user access requirements. Agentforce would answer rules/hints using controlled grounding, with separate input and output moderation, Trust Layer controls and consistent deployment across orgs. For global leaderboard reporting, data would be aggregated through an integration layer into an analytics destination.

Finally, I would validate the entire path — CDN, routing, Salesforce, event processing, database, LLM gateway and analytics — using representative performance, load, spike, stress and failover tests. Scale Test / Scale Center / ApexGuru can support the Salesforce-specific part. The exact shard count and test multiplier must come from measured workload and Salesforce capacity review, not assumptions.”

## 13. Interview Q&A — based on the episode

### Q1. Why multi-org?
Because the scenario combines very high concurrency, a large user population and burst registration. Sharding distributes load and routes users to a specific org. However, multi-org adds deployment, governance, reporting and operational complexity.

### Q2. How do returning users reach the same org?
Persist a user-to-shard mapping during initial registration. On later visits, resolve identity and look up the mapping before routing.

### Q3. Why a waiting room?
It shapes traffic and protects origin systems during a burst. A controlled wait is usually better than widespread errors and retries.

### Q4. Why CDN?
Static-content caching, edge filtering, geographic screening and traffic admission can reduce origin load. A CDN alone does not solve application capacity.

### Q5. Why asynchronous registration?
It removes heavy user-creation work from the immediate request path and helps absorb bursts. It requires idempotency, status tracking and failure recovery.

### Q6. Why Platform Events instead of Platform Cache?
In the episode, the team selected Platform Events because they were comfortable with the concurrency/event-driven registration pattern and had limited implementation time. Platform Cache is a cache, not a durable business-work queue.

### Q7. Why Full Copy Sandbox?
The episode selected it to support realistic scale testing. Current Scale Test entitlements and supported sandbox / instance requirements must be checked.

### Q8. What is Scale Center for?
Performance observability and analysis to identify bottlenecks.

### Q9. What is ApexGuru for?
Apex code insights and optimization / antipattern recommendations.

### Q10. Is 3x scale testing mandatory?
No universal mandate is established by the transcript or the cited product pages. 3x–5x was expert guidance in the episode. Derive the target from workload, risk and capacity objectives.

### Q11. How do you handle an agent vendor outage?
Use available platform-level LLM routing / failover where supported, and design application-level fallback and graceful degradation. Verify current product behavior and test it.

### Q12. How do you keep multiple orgs consistent?
Packaging, automated deployments, configuration management, version checks, release orchestration and monitoring.

### Q13. How do you create a global leaderboard?
Aggregate selected data from each shard into a central analytics layer; define freshness, privacy and failure-handling requirements.

## 14. Common mistakes / architecture traps

- Treating 8M total registrations as 8M concurrent users.
- Assuming “CC license” from the transcript is a current SKU recommendation.
- Assuming every large Experience Cloud deployment must be multi-org.
- Assuming an org's capacity is represented by one universal concurrent-user number.
- Assuming round robin is sufficient without shard health and capacity awareness.
- Treating IP geolocation as definitive proof of residency.
- Assuming OTP or CAPTCHA guarantees a human user.
- Treating Trust Layer toxicity detection as complete moderation for all player-input and agent-output channels.
- Treating Platform Events as exactly-once durable queue processing.
- Assuming event publish acknowledgement means downstream registration completed.
- Applying 3x or 5x without workload-based justification.
- Testing only Salesforce while ignoring CDN, AWS, identity, database, LLM and analytics layers.
- Forgetting cross-org deployment drift and leaderboard aggregation.
- Calling “zero downtime” a guarantee rather than defining SLO, RTO/RPO and graceful degradation.

## 15. Recommended production design checklist

- [ ] Peak RPS, concurrency, burst duration and user journey quantified.
- [ ] Salesforce licensing, user type, sharing model and capacity reviewed with Salesforce.
- [ ] Single-org versus multi-org decision documented with evidence.
- [ ] Shard assignment and affinity store designed for high availability.
- [ ] Idempotent registration and duplicate prevention implemented.
- [ ] CDN/WAF, bot mitigation and waiting-room policy configured.
- [ ] Geographic eligibility policy reviewed for false positives and legal requirements.
- [ ] Static content separated from dynamic personalized content.
- [ ] Asynchronous processing has retry, replay, dead-letter / failure handling and observability.
- [ ] Agent grounding, access controls, input moderation and output moderation validated.
- [ ] Agent configuration and prompt versions deployed consistently.
- [ ] Cross-org aggregation and leaderboard freshness defined.
- [ ] Full end-to-end load, spike, stress, soak and failover tests completed.
- [ ] Scale Test access, supported region and commercial entitlements confirmed.
- [ ] Cost model includes infrastructure, Salesforce testing and LLM consumption.
- [ ] Runbooks, incident ownership, dashboards and rollback plans ready.

## 16. 2026 Documentation Check — source claims vs current validation

> This section is additional research, not a replacement for what the speakers said. Product availability, editions, entitlements and limits can change; confirm in the target org and with Salesforce before procurement or design sign-off.

| Topic | Episode statement / pattern | Current documentation check / correction |
|---|---|---|
| Experience Cloud scale | Presenter says the scenario needs multi-org because a single org has limits; mentions hundreds of thousands. | Current Salesforce Help license guidance lists recommended user scale bands by license: Partner Community / Channel Account / Customer Community Plus 1M+, and with Account Role Optimization 10M+; Customer Community / External Apps 100M+. It also says larger scale may be possible after Salesforce review. Do **not** turn the episode's “hundreds of thousands” into a universal current hard cap. Validate exact license, org, workload and review with Salesforce. |
| High-volume users | Episode uses legacy “CC / Community Cloud” terminology. | Current Help describes high-volume Experience Cloud users as limited-access users intended for thousands to millions; they do not have roles and use specific license types. Select license by sharing and access requirements, not scale alone. |
| Scale Test | Episode recommends testing at 3x expected capacity and uses Full Copy Sandbox. | Salesforce Help says Scale Test is a paid add-on for Full Sandbox customers and recommends it when expected peak production traffic exceeds 120 RPS. Access is coordinated through the customer success representative / AE. |
| Performance testing | Episode frames 3x as a practical target. | Salesforce's 2026 FAQ distinguishes performance testing at or below 120 RPS from Scale Test above 120 RPS. This is an operational distinction, not a guarantee that a specific multiplier is sufficient. |
| Scale Center | Used for observability. | Current Help describes Scale Center as a performance metrics / analysis tool and states it is free for supported environments / editions. Check current org eligibility and data-visibility implications. |
| ApexGuru | Used for Apex optimization recommendations. | Salesforce release documentation continues to describe ApexGuru antipattern and optimization insights. Availability differs by edition / environment / entitlement. |
| Trust Layer toxicity | Episode says Trust Layer is used out of the box. | Salesforce Help describes toxicity detection for prompts and responses; current docs distinguish prompt-detection availability/status and response-detection behavior. Validate exact feature enablement, edition, channel, and whether player-input moderation is covered. |
| Platform Events | Used for asynchronous registration. | Salesforce docs describe Platform Events as event-driven messaging. High-volume asynchronous publishing can be at-least-once, so subscribers should handle duplicates (for example, deduplicate using EventUuid). Verify current allocations and delivery semantics. |
| LLM failover | Expert says LLM Gateway supports multiple vendors, load balancing and failover. | Treat this as a product capability claim to validate for the specific Agentforce configuration, supported providers, routing controls, contractual SLA and failure modes. Add application-level fallback and test it. |
| Data Cloud One | Asked about leaderboard aggregation. | The concrete answer in this transcript is MuleSoft + central repository + Tableau/CRM dashboard. Do not claim the episode implemented Data Cloud One for this. |

### Official Salesforce references checked
1. Experience Cloud license types: https://help.salesforce.com/s/articleView?id=users_license_types_communities.htm&language=en_US&type=5
2. High-volume Experience Cloud users: https://help.salesforce.com/s/articleView?id=platform.networks_about_hvpu.htm&language=en_US&type=5
3. Scale Test overview: https://help.salesforce.com/s/articleView?id=scale_test_overview.htm&language=en_US&type=5
4. Salesforce Performance and Scale Test FAQs (published July 1, 2026): https://help.salesforce.com/s/articleView?id=000387059&language=en_US&type=1
5. Scale Center overview: https://help.salesforce.com/s/articleView?id=sf.scale_center_overview.htm&language=en_US
6. ApexGuru release notes: https://help.salesforce.com/s/articleView?id=release-notes.rn_apexguru.htm&release=258&type=5
7. Einstein Trust Layer toxicity detection: https://help.salesforce.com/s/articleView?id=ai.generative_ai_trust_toxicity_detection.htm&language=en_US&type=5
8. Trust and Agentforce: https://help.salesforce.com/s/articleView?id=sf.copilot_trust.htm&language=en_US&type=5
9. Platform Events considerations: https://developer.salesforce.com/docs/platform/platform-events/guide/platform-events-considerations.html
10. Platform Events introduction: https://developer.salesforce.com/docs/platform/platform-events/guide/platform-events-intro.html

## 17. Quick revision summary

- WHAT: 8M registrations, 1M concurrent users, US/Canada, Experience Cloud, Agentforce, toxicity and availability.
- HOW: CDN + waiting room + pre-login AWS routing + shard registration + multiple Salesforce orgs + asynchronous registration + Agentforce + analytics aggregation.
- WHY: isolate burst load, route returning users correctly, reduce origin pressure, decouple heavy registration, protect agent interactions and validate performance before launch.
- Governance: consistent packaging, deployments, configuration and operations across orgs.
- Testing: end-to-end workload model, Scale Test, Scale Center, ApexGuru and cost approval.
- Critical caveat: episode recommendations are scenario-specific; current licenses, limits, features and entitlements require validation.

## 18. No-Skip Checklist

- [ ] Introduction, presenters and episode agenda
- [ ] WHAT–HOW–WHY method
- [ ] Full business scenario and all numeric requirements
- [ ] HLR grouping
- [ ] Org and license strategy discussion
- [ ] Multi-org reasoning
- [ ] Registration DML concern
- [ ] US/Canada eligibility
- [ ] Static content and CDN
- [ ] Waiting room
- [ ] New-user routing and returning-user routing
- [ ] AWS pre-login layer, round robin and DynamoDB shard mapping
- [ ] Agentforce Service Agent
- [ ] Rules, hints, conversation history and prompt-template deployment
- [ ] Einstein Trust Layer / toxicity
- [ ] Single-org vs multi-org audience question
- [ ] Scale Test at 3x episode guidance
- [ ] Scale Center and ApexGuru
- [ ] Full Copy Sandbox
- [ ] LLM vendor failover / gateway discussion
- [ ] Granular agent actions / reusability discussion
- [ ] Human verification and anti-bot layers
- [ ] Multi-org governance and packaging
- [ ] Leaderboard / BI aggregation
- [ ] COVID and FIFA waiting-room use cases
- [ ] Heroku / AWS / EventBridge discussion and absence of official recommendation
- [ ] Platform Events async registration and Platform Cache comparison
- [ ] Scale-test cost and downtime-risk discussion
- [ ] Repeated testing for future releases / peak events
- [ ] MuleSoft central aggregation and Tableau / CRM dashboard
- [ ] Closing and resource mention
- [ ] Full original transcript appended verbatim below

## 19. Board-Level Answer

A high-burst Experience Cloud solution should protect Salesforce from uncontrolled traffic at the edge, control admission through a waiting room, and route each user consistently to a shard. Registration can be decoupled through asynchronous event processing. Agentforce should use governed grounding and explicit input/output safety controls. Multi-org operations require automated deployment governance and central analytics aggregation. Before launch, validate the entire path with a representative workload and Salesforce scale-testing capabilities, with costs and failure modes agreed by stakeholders.

---

