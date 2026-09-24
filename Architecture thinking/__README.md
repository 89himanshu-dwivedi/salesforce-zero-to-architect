# Enterprise Architecture Thinking: Zero to Architect

Hi, I'm **Himanshu Kumar** — Technical Lead & Solution Architect with **8 years** of experience building enterprise systems.

I work at **CRISIL Ltd, an S&P Global company**, on S&P Global projects.

This is my catch-all learning repo for everything that sits *around the application* — messaging, caching, databases, integrations, infrastructure, cloud, AI/GenAI, and the architecture thinking needed to connect them into production-ready enterprise systems.

---

## What You'll Get From Here

This repository focuses on the **architecture thinking** required to move from:

```text
Developer
    ↓
Senior Developer
    ↓
Technical Lead
    ↓
Solution Architect
    ↓
Enterprise Architect
```

The goal is not just to learn technologies.

The goal is to understand:

```text
Business Requirement
        ↓
Constraints
        ↓
Architecture Options
        ↓
Trade-offs
        ↓
Technology Selection
        ↓
Implementation
        ↓
Security
        ↓
Scalability
        ↓
Integration
        ↓
Operations
        ↓
Business Outcome
```

---

# 🏛️ Architecture Thought — Zero to Architect

This section covers the **architecture thinking** required to move from Developer → Architect.

## Covered Topics — 44

1. Career Self-Check — Is Becoming an Architect Right for You?
2. Architect Responsibilities — Design and Risk
3. Architect Responsibilities — Technology and Standards
4. Architect Responsibilities — Leadership and People
5. Solution Design — The LEGO Mindset
6. Architect Mindset — 3 Habits That Matter Most
7. Salesforce Certification Pyramid — Admin to CTA
8. Path to Success — Theory + Real Project + Repeat
9. Technical vs Solution vs Enterprise Architect
10. Vision to Developer Specification — Architect Handover Chain
11. Admin to Architect — Is Coding Required?
12. Mobile Designer Exam — The “Easy” Trap and Reality Check
13. Client Meeting — Handling Requirements
14. 4 Magic Questions — Mobile Strategy Framework
15. Salesforce Mobile App or Custom App?
16. HTML5 vs Native vs Hybrid
17. Mobile Security Scope
18. Mobile Testing Strategy
19. Exam Guide Connection, Identity Overlap and Final Recap
20. Privacy Regulations — GDPR, CCPA and LGPD
21. Why Users Are Unhappy — The Business Case for Privacy
22. Three Privacy Pillars — Find, Manage, Control
23. Find and Understand Customer Data
24. Make Data Management Easier — DSAR
25. Give Customers Control
26. Salesforce Tool 1 — Data Classification
27. Salesforce Tool 2 — Four Levels of Consent
28. Consent API — Reconciliation of Distributed Consent
29. Privacy Roadmap — What Is Coming and When
30. Privacy Q&A — Marketing Cloud, Community Limits and Case Studies
31. Platform App Builder Exam — Session Plan and Weighting
32. Declarative vs Programmatic — Meaning and Available Tools
33. Declarative vs Programmatic — Pros, Cons and Decision Framework
34. Real Scenarios — Declarative, Programmatic or Both?
35. Use Case Cheat Sheet — Choosing the Right Tool
36. AppExchange — Salesforce Marketplace and Solution Types
37. AppExchange — When to Use It, Read Listings and Install
38. Managed vs Unmanaged Packages — Difference and Impact
39. Data Modeling — Five Salesforce Relationship Types
40. Relationship Scenarios — Master-Detail or Lookup?
41. Junction Objects — Solving Many-to-Many Relationships
42. Field Types — How to Choose the Right Field
43. Changing Field Types — Data Loss Rules
44. Session Q&A and Study Resources

> These 44 topics will be covered **one by one and in depth**, including concepts, architecture reasoning, real scenarios, trade-offs and Q&A.

Everything is Markdown with diagrams and practical examples, so it can be read directly on GitHub.

---

# 🧠 Architecture Decision Framework

Every technology or design decision should answer:

### 1. What problem are we solving?

```text
Business Problem
      ↓
Technical Problem
      ↓
Architecture Requirement
```

### 2. What options do we have?

```text
Option A
Option B
Option C
```

### 3. What are the trade-offs?

```text
Cost
Performance
Scalability
Security
Complexity
Maintainability
Availability
Team Skills
Time to Market
```

### 4. What happens in production?

```text
Failure
      ↓
Retry
      ↓
Recovery
      ↓
Monitoring
      ↓
Alerting
      ↓
Business Continuity
```

### 5. Why did we choose this architecture?

The architect should be able to explain the **reasoning behind the decision**, not just the technology selected.

---

# 🔧 Architecture Areas

This repository will grow across the technology areas surrounding enterprise applications:

```text
Enterprise Architecture
│
├── Application Architecture
├── Integration Architecture
├── Data Architecture
├── Security Architecture
├── Infrastructure
├── Cloud
├── Messaging
├── Caching
├── Databases
├── APIs
├── Observability
├── DevOps
├── AI / GenAI
├── RAG
└── Distributed Systems
```

The Salesforce architecture topics remain an important part of the journey, while the surrounding enterprise technologies help build broader architect-level thinking.

---

## How This Repo Is Built

I add one folder per technology or architecture area as I work through it.

Each folder focuses on:

1. **What problem does it solve?**
2. **How does it work?**
3. **When should you use it?**
4. **When should you NOT use it?**
5. **What are the architecture trade-offs?**
6. **What can go wrong in production?**
7. **How does it integrate with an enterprise system?**
8. **Small runnable examples where applicable**

The goal is not just to learn tools — it is to understand **why an architect chooses one approach over another.**

---

# 📚 Learning Structure

Every major topic should eventually be understood through:

```text
Simple
   ↓
Advanced
   ↓
Super Advanced
   ↓
Practical
   ↓
Architecture
   ↓
Trade-offs
   ↓
Errors & Gotchas
   ↓
Production Considerations
   ↓
Interview / Q&A
   ↓
Real-World Use Case
```

---

# 🏗️ From Developer Thinking to Architect Thinking

### Developer

```text
How do I implement this?
```

### Senior Developer

```text
How do I implement this correctly and efficiently?
```

### Technical Lead

```text
How should the team implement this?
```

### Solution Architect

```text
What is the right solution for the business and technical constraints?
```

### Enterprise Architect

```text
How does this decision fit the entire enterprise
across systems, data, security, integration,
operations, cost and future evolution?
```

---

# 🌐 Enterprise System Thinking

A production system is rarely a single application.

```text
Users
  ↓
Experience Layer
  ↓
API / Integration Layer
  ↓
Application Services
  ↓
Messaging
  ↓
Databases
  ↓
Cache
  ↓
External Systems
  ↓
Infrastructure / Cloud
  ↓
Monitoring & Operations
```

The architect's responsibility is to understand how these pieces work **together**.

---

# 🤖 AI / GenAI Architecture

AI and GenAI are also part of modern enterprise architecture.

The repository will connect AI concepts with traditional architecture:

```text
Business Use Case
       ↓
AI / Agent Decision
       ↓
Prompt / Model
       ↓
RAG / Data
       ↓
Integration / Tools
       ↓
Security / Guardrails
       ↓
Evaluation
       ↓
Monitoring
       ↓
Production
```

The focus is not simply:

> "How do I call an LLM?"

It is:

> **"How do I safely and reliably integrate AI into an enterprise architecture?"**

---

# 🔐 Production Architecture Checklist

For every important architecture decision, consider:

- [ ] Business requirements
- [ ] Functional requirements
- [ ] Non-functional requirements
- [ ] Security
- [ ] Authentication
- [ ] Authorization
- [ ] Data protection
- [ ] Scalability
- [ ] Performance
- [ ] Availability
- [ ] Reliability
- [ ] Disaster recovery
- [ ] Integration
- [ ] Observability
- [ ] Monitoring
- [ ] Cost
- [ ] Maintainability
- [ ] Deployment
- [ ] Operational ownership
- [ ] Future evolution

---

# 🎤 Architect Interview Preparation

Each topic will include questions such as:

### Concept

- What is it?
- Why does it exist?
- How does it work?

### Architecture

- When would you use it?
- When would you avoid it?
- What alternatives exist?
- What are the trade-offs?

### Production

- What can fail?
- How do you handle failures?
- How do you scale it?
- How do you monitor it?
- How do you secure it?

### Scenario

```text
Business Requirement
        ↓
Constraints
        ↓
Options
        ↓
Architecture Decision
        ↓
Trade-offs
        ↓
Implementation
        ↓
Production Considerations
```

---

# 🚀 Real-World Architecture Goal

The ultimate objective is to become comfortable taking a vague requirement such as:

> “We need a scalable, secure platform that integrates Salesforce, external systems, data, and AI.”

and turning it into:

```text
Business Requirements
        ↓
Architecture Drivers
        ↓
System Context
        ↓
Component Architecture
        ↓
Data Architecture
        ↓
Integration Architecture
        ↓
Security Architecture
        ↓
Deployment Architecture
        ↓
Operational Model
        ↓
Trade-offs
        ↓
Implementation Roadmap
```

---

# 📊 Definition of Done

A topic is considered **COMPLETE** when we have:

- [ ] Concept
- [ ] Simple explanation
- [ ] Advanced explanation
- [ ] Super Advanced explanation
- [ ] Architecture reasoning
- [ ] Practical example
- [ ] Real-world scenario
- [ ] Trade-offs
- [ ] Errors & Gotchas
- [ ] Production considerations
- [ ] Security considerations
- [ ] Scalability considerations
- [ ] Integration considerations
- [ ] Interview / Q&A
- [ ] Decision framework

---

# 🏁 Final Goal

The journey is:

```text
Developer
    ↓
Strong Technical Foundation
    ↓
System Thinking
    ↓
Architecture Thinking
    ↓
Solution Design
    ↓
Enterprise Integration
    ↓
Cloud + Infrastructure
    ↓
Data + Security
    ↓
AI / GenAI
    ↓
Production Architecture
    ↓
Solution Architect
    ↓
Enterprise Architect
```

The goal is not to memorize architecture diagrams.

The goal is to develop the ability to answer:

> **What should we build, why should we build it this way, what are the alternatives, what can go wrong, and how will it evolve?**

---

## About Me

- Technical Lead & Solution Architect at **CRISIL Ltd, an S&P Global company**
- 8 years across Salesforce architecture, integrations and enterprise GenAI
- Domains: Public Sector, Financial Services, Healthcare, Supply Chain
- B.Tech in Computer Science, AKTU Lucknow
- [GitHub](https://github.com/89himanshu-dwivedi)
- [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/)
- [X](https://x.com/kum60094)
- [Trailblazer](https://www.salesforce.com/trailblazer/hdwivedi2)
- Email: [himanshu.jee.1996@gmail.com](mailto:himanshu.jee.1996@gmail.com)

---

# 🔒 Attribution & Ownership

Copyright (c) 2026 Himanshu Kumar. All rights reserved.

This material is my original learning and documentation work.

Reading it here and linking to it is welcome.

Downloading, copying, mirroring, forking, redistributing, republishing, creating derivative works, or using this material to train an AI/ML model requires **prior written permission**.

Request permission through [GitHub](https://github.com/89himanshu-dwivedi).

---

## License

See [LICENSE](https://github.com/89himanshu-dwivedi/enterprise-stack-zero-to-architect/blob/main/LICENSE) — proprietary, all rights reserved, permission required.

---

## ⭐ Repository Principle

> **Don't just learn technologies. Learn the problem they solve, understand the trade-offs, design for production, and know why an architect would choose one approach over another.**
