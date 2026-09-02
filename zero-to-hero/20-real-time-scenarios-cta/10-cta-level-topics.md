[Home](../index.md) / [20 · Real-Time Scenarios (CTA)](index.md) / **CTA-Level Topics**

# CTA-Level Topics

25 topics · Series 20: Real-Time Scenarios (CTA)

**Topics on this page**

- [CAP Theorem](#cap-theorem)
- [Eventual Consistency](#eventual-consistency)
- [Strong Consistency](#strong-consistency)
- [Distributed Transactions](#distributed-transactions)
- [Saga Pattern](#saga-pattern)
- [CQRS](#cqrs)
- [Event Sourcing](#event-sourcing)
- [Bulkhead Pattern](#bulkhead-pattern)
- [Circuit Breaker Pattern](#circuit-breaker-pattern)
- [Retry Pattern](#retry-pattern)
- [API Gateway](#api-gateway)
- [Service Mesh](#service-mesh)
- [Zero Trust Security](#zero-trust-security)
- [Data Mesh](#data-mesh)
- [Data Fabric](#data-fabric)
- [MDM](#mdm)
- [Lakehouse Architecture](#lakehouse-architecture)
- [Domain Driven Design](#domain-driven-design)
- [Bounded Context](#bounded-context)
- [Architecture Trade-off Analysis](#architecture-trade-off-analysis)
- [TOGAF Basics](#togaf-basics)
- [NFR Analysis](#nfr-analysis)
- [Cost Optimization](#cost-optimization)
- [FinOps](#finops)
- [Enterprise Governance](#enterprise-governance)

## CAP Theorem

*Consistency, Availability, Partition tolerance — pick two.*

### 🌱 Simple

*Beginner - plain language*

The **CAP theorem** states a distributed system can guarantee only **two of three**: **Consistency, Availability, Partition tolerance**. Since network partitions happen, you really trade **consistency vs availability**.

### 📏 Limits

*Governor & platform limits*

- Pick 2 of C/A/P; partitions force C-vs-A; CP (consistent) vs AP (available).
- Most SFDC integrations → AP/eventual; PACELC adds latency-vs-consistency.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Eventual Consistency

*Systems converge to a consistent state over time.*

### 🌱 Simple

*Beginner - plain language*

**Eventual consistency** means distributed systems may be **temporarily out of sync** but will **converge to the same state** over time — trading immediate consistency for availability and resilience.

### 📏 Limits

*Governor & platform limits*

- Temporary divergence, guaranteed convergence; async messaging + idempotency.
- Conflict resolution; tolerate lag in UX; not for real-time-accuracy domains.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Strong Consistency

*Every read reflects the latest committed write — immediately.*

### 🌱 Simple

*Beginner - plain language*

**Strong consistency** guarantees that once a write commits, **every subsequent read sees it immediately** — no stale data. It prioritizes correctness over availability/latency.

### 📏 Limits

*Governor & platform limits*

- Reads always see latest write; synchronous coordination/locking/consensus.
- SFDC strong within a transaction; cross-system costly — use selectively for critical ops.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Distributed Transactions

*Coordinate atomic operations across multiple systems.*

### 🌱 Simple

*Beginner - plain language*

**Distributed transactions** coordinate a single logical operation across **multiple systems** so it all succeeds or all fails — hard to do across Salesforce and external systems, which usually favor other patterns.

### 📏 Limits

*Governor & platform limits*

- Cross-system atomicity is hard; 2PC fragile/unsupported in SFDC.
- Use Saga + compensation, idempotency, reconciliation; SFDC single-txn is atomic.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Saga Pattern

*Manage cross-service consistency via compensating transactions.*

### 🌱 Simple

*Beginner - plain language*

The **Saga pattern** manages a long-running, multi-system transaction as a **sequence of local transactions**, each with a **compensating action** to undo it if a later step fails — achieving consistency without distributed locks.

### 📏 Limits

*Governor & platform limits*

- Local transactions + compensations; choreography (events) vs orchestration (coordinator).
- Idempotent reversible compensations; persist state; retries/timeouts/dead-letter.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## CQRS

*Separate read and write models for scale and clarity.*

### 🌱 Simple

*Beginner - plain language*

**CQRS (Command Query Responsibility Segregation)** separates the **write model (commands)** from the **read model (queries)** — optimizing each independently for scale, performance, and clarity.

### 📏 Limits

*Governor & platform limits*

- Separate command (write) and query (read) models; event-synced; eventual consistency.
- SFDC = write model, event-fed read stores for heavy reads; avoid for simple CRUD.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Event Sourcing

*Store state as an immutable log of events.*

### 🌱 Simple

*Beginner - plain language*

**Event sourcing** stores application state as an **immutable sequence of events** (what happened) rather than just the current state — you rebuild state by replaying events, gaining full history and auditability.

### 📏 Limits

*Governor & platform limits*

- Immutable append-only event log; state = replay; audit/temporal/replay benefits.
- SFDC = current-state; approximate via Big Object/external store + Platform Events/CDC.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Bulkhead Pattern

*Isolate resources so one failure can't sink everything.*

### 🌱 Simple

*Beginner - plain language*

The **bulkhead pattern****isolates resources into separate pools** (like a ship's watertight compartments) so a failure or overload in one part can't exhaust resources and take down the whole system.

### 📏 Limits

*Governor & platform limits*

- Partition resources (threads/connections/queues) per dependency/tenant/feature.
- Contain cascade; middleware connection pools; pair with circuit breaker; size + monitor.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Circuit Breaker Pattern

*Stop calling a failing dependency to prevent cascade.*

### 🌱 Simple

*Beginner - plain language*

The **circuit breaker pattern** stops calling a **failing dependency** after repeated failures — "tripping" to fail fast and fall back — then periodically tests recovery, preventing cascading failures and resource exhaustion.

### 📏 Limits

*Governor & platform limits*

- Closed/Open/Half-Open; fail fast + fallback after threshold; trial recovery.
- Wrap callouts/middleware; pair with bulkheads + retries; tune; alert on trips.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Retry Pattern

*Transparently retry transient failures — safely.*

### 🌱 Simple

*Beginner - plain language*

The **retry pattern** automatically **re-attempts a failed operation** that's likely **transient** (a blip, timeout, throttle) — improving resilience — using backoff and idempotency so retries are safe.

### 📏 Limits

*Governor & platform limits*

- Transient-only; exponential backoff + jitter; max attempts then dead-letter.
- Idempotency required; middleware/Queueable; pair with circuit breaker; observe.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## API Gateway

*A single managed entry point for APIs.*

### 🌱 Simple

*Beginner - plain language*

An **API gateway** is a single managed **entry point** for APIs — handling routing, authentication, rate limiting, and transformation — so clients hit one endpoint instead of many backend services directly.

### 📏 Limits

*Governor & platform limits*

- Single entry; centralizes auth/throttling/routing/transform/caching/versioning.
- MuleSoft gateway fronting SFDC+backends; protects API limits; north-south traffic.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Service Mesh

*Infrastructure layer managing service-to-service traffic.*

### 🌱 Simple

*Beginner - plain language*

A **service mesh** is an infrastructure layer that manages **service-to-service communication** in a microservices system — handling routing, security (mTLS), retries, and observability transparently via sidecar proxies.

### 📏 Limits

*Governor & platform limits*

- East-west service traffic via sidecars; mTLS, resilience, traffic control, telemetry.
- Complements gateway; in the platform around SaaS Salesforce; needs scale to justify.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Zero Trust Security

*Never trust, always verify — every request.*

### 🌱 Simple

*Beginner - plain language*

**Zero trust security** assumes **no implicit trust** — every user, device, and request is **verified** regardless of network location. "Never trust, always verify" replaces the old trusted-perimeter model.

### 📏 Limits

*Governor & platform limits*

- Never trust/always verify; identity, least privilege, device, micro-seg, encryption.
- Per-request continuous auth; SFDC: MFA/SSO/policies/Shield/secured code/mTLS.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Data Mesh

*Decentralized, domain-owned data as a product.*

### 🌱 Simple

*Beginner - plain language*

**Data mesh** is a **decentralized** approach where **business domains own their data as products** — rather than a central team owning one monolithic data platform — with self-serve infrastructure and federated governance.

### 📏 Limits

*Governor & platform limits*

- 4 principles: domain ownership, data-as-product, self-serve platform, federated governance.
- Decentralized analytical data; SFDC publishes domain data product (Data Cloud/zero-copy).

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Data Fabric

*Unified, intelligent data integration across sources.*

### 🌱 Simple

*Beginner - plain language*

**Data fabric** is an architecture that provides a **unified, intelligent layer** connecting data across all sources — using metadata, automation, and virtualization to integrate and access data wherever it lives.

### 📏 Limits

*Governor & platform limits*

- Unified intelligent layer; active metadata/knowledge graph; virtualization; governance.
- Centralized tech-driven (vs mesh org-driven); SFDC via Data Cloud/zero-copy/MuleSoft.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## MDM

*One trusted golden record across systems.*

### 🌱 Simple

*Beginner - plain language*

**MDM (Master Data Management)** creates a single, trusted **"golden record"** for core business entities (customer, product, account) across all systems — resolving duplicates and conflicts to give one consistent version of the truth.

### 📏 Limits

*Governor & platform limits*

- Golden record via matching/merging/survivorship/stewardship; registry/consolidation/coexistence/centralized.
- SFDC usually spoke to MDM hub + native duplicate rules; governance-heavy.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Lakehouse Architecture

*Data warehouse reliability on data-lake economics.*

### 🌱 Simple

*Beginner - plain language*

**Lakehouse architecture** combines the **low-cost, open storage of a data lake** with the **reliability, performance, and management of a data warehouse** — one platform for BI and AI/ML on the same data.

### 📏 Limits

*Governor & platform limits*

- Open table formats (Delta/Iceberg/Hudi) over object storage; ACID/time-travel; BI+ML on one copy.
- Warehouse reliability on lake economics; SFDC via Data Cloud zero-copy.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Domain Driven Design

*Model software around the business domain.*

### 🌱 Simple

*Beginner - plain language*

**Domain-Driven Design (DDD)** models software around the **business domain**, using a **shared "ubiquitous language"** between developers and domain experts, and organizing the system by business concepts rather than technical layers.

### 📏 Limits

*Governor & platform limits*

- Strategic (bounded contexts, ubiquitous language, context maps) + tactical (entities/VOs/aggregates/events).
- SFDC: guides logic placement, aggregate/transaction boundaries; domain events ↔ Platform Events.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Bounded Context

*An explicit boundary where one model is consistent.*

### 🌱 Simple

*Beginner - plain language*

A **bounded context** is an explicit boundary within which a particular **domain model and its terms are consistent and well-defined** — the same word (e.g., "Customer") can mean different things in different contexts.

### 📏 Limits

*Governor & platform limits*

- Explicit boundary; each context owns model/language/data; context map relationships.
- ACL translates between contexts; aligns org/service boundaries (CRM vs ERP).

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Architecture Trade-off Analysis

*Decide by weighing competing quality attributes.*

### 🌱 Simple

*Beginner - plain language*

**Architecture trade-off analysis** is the discipline of evaluating design options by explicitly **weighing competing quality attributes** (performance vs cost, consistency vs availability, security vs usability) to pick the best fit — there's no perfect answer, only trade-offs.

### 📏 Limits

*Governor & platform limits*

- Weigh competing quality attributes; ATAM sensitivity/trade-off points/risks; anchor in NFRs.
- SFDC trade-offs: declarative vs Apex, real-time vs batch, single vs multi-org; document via ADRs.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## TOGAF Basics

*A framework for enterprise architecture.*

### 🌱 Simple

*Beginner - plain language*

**TOGAF (The Open Group Architecture Framework)** is a widely used **enterprise architecture framework** — providing a method (the ADM), domains, and governance to design, plan, implement, and govern enterprise IT architecture.

### 📏 Limits

*Governor & platform limits*

- EA framework; ADM iterative method; 4 domains (business/data/app/tech); governance.
- Positions Salesforce within EA; tailor pragmatically; combine with Agile.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## NFR Analysis

*Define and design for the 'how well', not just 'what'.*

### 🌱 Simple

*Beginner - plain language*

**NFR (Non-Functional Requirement) analysis** defines and designs for **how well** a system must behave — performance, scalability, availability, security, usability — as opposed to functional requirements (**what** it does). NFRs shape the architecture.

### 📏 Limits

*Governor & platform limits*

- Measurable quality attributes (perf/scale/availability/security…); drive architecture+testing.
- SFDC: governor-safe/LDV/API budget/Shield/availability; capture early; validate with testing.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Cost Optimization

*Maximize value per dollar across the architecture.*

### 🌱 Simple

*Beginner - plain language*

**Cost optimization** is designing and operating systems to **maximize business value per dollar** — right-sizing licenses, storage, compute, and integrations while avoiding waste — without sacrificing required quality.

### 📏 Limits

*Governor & platform limits*

- Value per dollar; license/storage/API/integration right-sizing; avoid over-engineering.
- Measure continuously (→FinOps); balance against required NFRs; trade-offs explicit.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## FinOps

*Bringing financial accountability to cloud spend.*

### 🌱 Simple

*Beginner - plain language*

**FinOps** is a practice that brings **financial accountability to cloud/SaaS spend** — a cultural and operational discipline where engineering, finance, and business **collaborate** to make data-driven, value-based spending decisions.

### 📏 Limits

*Governor & platform limits*

- Cultural+operational; Inform→Optimize→Operate; teams own spend; eng+finance+business collaborate.
- Operating model around cost optimization; SFDC license/consumption showback + governance.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Enterprise Governance

*Steering and controlling the enterprise platform.*

### 🌱 Simple

*Beginner - plain language*

**Enterprise governance** is the framework of **policies, decision rights, standards, and oversight** that ensures the platform (e.g., a multi-team Salesforce program) aligns with business goals, stays secure and compliant, and evolves in a controlled, consistent way.

### 📏 Limits

*Governor & platform limits*

- Decision rights/RACI, CoE, architecture/change/security boards, standards, org strategy, metrics.
- Enable not block: lightweight automated guardrails; pair with Agile/DevOps.

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
