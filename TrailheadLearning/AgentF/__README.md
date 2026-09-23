# Salesforce Agentblazer: Champion → Innovator → Legend

Hi, I'm **Himanshu Kumar** — Technical Lead & Salesforce Solution Architect.

This repo is my open learning log for the **Salesforce Agentblazer journey**, covering the complete path from Agentforce fundamentals to advanced agent architecture, implementation, integration, AI, Data 360, RAG, testing, deployment, and architect-level design.

Most of my delivery work lives in client orgs and private repos, so this is where I rebuild, document, and explain the concepts and hands-on work in the open.

---

## Start here — [Agentblazer 2026](https://trailhead.salesforce.com/agentblazer)

> **Goal: Complete all 3 Agentblazer levels — Champion, Innovator, and Legend — with the associated learning content, hands-on exercises, projects, and architecture preparation.**

The curriculum is organized so we can progress from fundamentals to advanced implementation without skipping the hands-on work:

- **Champion** — AI and Agentforce foundations + basic hands-on
- **Innovator** — business implementation + advanced Agentforce use cases + hands-on
- **Legend** — advanced customization + strategy + enterprise architecture

Every major topic will be documented with:

**Simple → Advanced → Super Advanced → Practical → Interview → Errors & Gotchas → Limits → Best Option & Trade-offs → Real World → Architecture**

---

## 🏆 Level 1 — Agentblazer Champion

The foundation layer.

Topics include:

- AI fundamentals
- Human + Agent interaction
- AI ethics
- Autonomous agents
- Agentforce fundamentals
- Agent Builder / Agentforce Builder
- Prompt Builder
- Basic agent creation
- Agentforce concepts and terminology
- Basic hands-on implementation

### Current hands-on foundation

```text
Agentforce Builder
        ↓
Service Agent
        ↓
Custom Subagent
        ↓
Actions
        ↓
Flows
        ↓
Reasoning Instructions
        ↓
Testing
        ↓
Commit
        ↓
Activate
        ↓
Deployment
        ↓
Experience Cloud
        ↓
Customer Chat
```

---

## ⚡ Level 2 — Agentblazer Innovator

The implementation and business-value layer.

Topics will include:

- Agent planning
- Business-driven Agentforce strategy
- Sales Agents
- Service Agents
- RAG
- Testing and troubleshooting
- Threat modeling
- Agent deployment
- Agentforce Voice
- Advanced customization
- Practical business use cases
- Hands-on challenges and Superbadges

The focus changes from:

```text
"What is Agentforce?"
```

to:

```text
"How do I design and implement an Agentforce solution
for a real business problem?"
```

---

## 👑 Level 3 — Agentblazer Legend

The advanced and architect layer.

Topics will include:

- Advanced Agent customization
- Prompts + Flows + Apex
- Data 360 / Data Cloud
- Agent Data Management
- Unstructured data
- RAG best practices
- Agent testing
- Testing Center
- Monitoring
- Slack deployment
- Agentforce DX
- Agent API
- Models API
- Enterprise Agentforce architecture
- AI strategy and governance

The focus becomes:

```text
Business Strategy
       ↓
Enterprise Architecture
       ↓
Agent Architecture
       ↓
Data Architecture
       ↓
Security
       ↓
Integration
       ↓
AI / RAG
       ↓
Testing
       ↓
Deployment
       ↓
Monitoring
       ↓
Business Outcome
```

---

## What you'll get from this repo

- Agentforce Builder explained from fundamentals to advanced usage
- Service Agent and Subagent architecture
- Agent Router and routing patterns
- Actions and Custom Actions
- Flow-powered Agentforce actions
- Agent Script
- Variables
- Reasoning Instructions
- Conditional logic
- Deterministic agent behavior
- Action filtering
- Permissions and field-level security
- Testing and debugging
- Deployment patterns
- Experience Cloud and Embedded Messaging
- Prompt Builder
- RAG and grounding
- Data 360 / Data Cloud
- Sales and Service Agent use cases
- AI and LLM engineering perspective
- Integration architecture
- Apex + Flow + Agentforce design
- Security and governance
- Enterprise architecture
- Interview and architect scenario preparation
- Real-world hands-on projects

---

## Current hands-on content

### Agentforce Builder

```text
Create Service Agent
        ↓
Create Experience Management Subagent
        ↓
Add Actions
        ↓
Get Experience Details
Get Customer Details
Get Sessions
Create Experience Session Booking
        ↓
Add Reasoning Instructions
        ↓
Test
        ↓
Commit
        ↓
Activate
        ↓
Publish Deployment
        ↓
Update Route to ESA Flow
        ↓
Experience Cloud
        ↓
Embedded Messaging
```

### Flows + Actions + Permissions + Determinism

```text
Salesforce Contact
        ↓
Lifetime_Value__c
        ↓
Get Customer Details Flow
        ↓
Agent Variable
        ↓
Conditional Logic
        ↓
Platinum / Gold / Regular
        ↓
Action Filtering
        ↓
Credit Action
        ↓
Salesforce Record
```

This section covers:

- Flow output configuration
- Flow versioning
- Agent User permissions
- Field-level security
- Agentforce variables
- Agent Script
- Conditional expressions
- Loyalty business rules
- Custom Flow actions
- Action filtering
- Variable preview
- Deterministic business rules

---

## How this repo is built

I add one learning unit at a time as I work through the Agentblazer path.

Each section will contain:

```text
Learning Content
      ↓
Hands-On
      ↓
Configuration
      ↓
Testing
      ↓
Errors & Gotchas
      ↓
Architecture
      ↓
Interview Questions
      ↓
Real-World Use Case
```

The objective is not just to collect Trailhead completions.

The objective is to **understand, build, test, debug, and architect Agentforce solutions**.

---

## Architecture-first approach

For every major Agentforce feature, we will ask:

### Business

- What problem are we solving?
- Who is the user?
- What is the expected outcome?

### Agent

- Which Agent?
- Which Subagent?
- How does routing work?
- What should the agent reason about?

### Actions

- Which Action?
- Flow or Apex?
- What are the inputs?
- What are the outputs?
- What validations are required?

### Data

- Which Salesforce objects?
- Which fields?
- Data 360 / Data Cloud?
- Knowledge?
- Unstructured data?
- RAG?

### Security

- Authentication
- Authorization
- CRUD/FLS
- Sharing
- Permission Sets
- Data access
- AI data exposure

### Determinism

```text
Business Rule
      ↓
Variable
      ↓
Condition
      ↓
Action Filter
      ↓
Controlled Agent Behavior
```

### Integration

- REST / SOAP
- MuleSoft
- External systems
- Named Credentials
- Platform Events
- Agent API
- Models API

### Reliability

- Error handling
- Retry
- Idempotency
- Logging
- Monitoring
- Testing
- Guardrails

---

## Hands-on projects

We will build practical projects such as:

- Customer Service Agent
- Experience Booking Agent
- Loyalty / Credit Agent
- Knowledge + RAG Agent
- Sales Agent
- Service Agent
- Data 360 powered Agent
- Integration Agent
- Enterprise Agentforce solution

Each project should eventually document:

```text
Business Problem
      ↓
Requirements
      ↓
Architecture
      ↓
Agent
      ↓
Subagents
      ↓
Actions
      ↓
Flow / Apex
      ↓
Data
      ↓
Security
      ↓
AI / RAG
      ↓
Testing
      ↓
Deployment
      ↓
Monitoring
      ↓
Business Impact
```

---

## Interview & Architect Preparation

Every major topic will also be converted into interview-ready material:

- Basic questions
- Advanced questions
- Scenario-based questions
- Architect questions
- Design trade-offs
- Errors & Gotchas
- Limits / constraints
- Security questions
- Integration questions
- AI questions
- Salesforce implementation questions
- Real-world architecture scenarios

The goal is to move from:

```text
"I know the feature."
```

to:

```text
"I can explain the feature,
implement it,
secure it,
integrate it,
test it,
and defend the architecture decision."
```

---

## Definition of Done

A topic is considered **complete** only when we have:

- [ ] Concept
- [ ] Simple explanation
- [ ] Advanced explanation
- [ ] Super Advanced explanation
- [ ] Hands-On
- [ ] Configuration
- [ ] Example
- [ ] Real-World Use Case
- [ ] Errors & Gotchas
- [ ] Limits / Constraints
- [ ] Security
- [ ] Performance
- [ ] Trade-offs
- [ ] Architecture Perspective
- [ ] Interview Questions
- [ ] Practical Project

---

## Final roadmap

```text
AGENTBLAZER 2026
       │
       ├── 🏆 CHAMPION
       │      Foundations
       │      AI Basics
       │      Agentforce Basics
       │      Basic Hands-On
       │
       ├── ⚡ INNOVATOR
       │      Business Implementation
       │      Sales / Service
       │      RAG
       │      Testing
       │      Deployment
       │      Threat Modeling
       │
       └── 👑 LEGEND
              Advanced Customization
              Data 360
              RAG
              Testing Center
              Monitoring
              Agentforce DX
              Agent API
              Models API
              Enterprise Strategy
                       ↓
              AGENTFORCE ARCHITECT
```

---

## Final target

The goal of this repository is:

```text
Champion
   +
Innovator
   +
Legend
   +
Hands-On
   +
Agentforce
   +
Data 360
   +
RAG
   +
Flow
   +
Apex
   +
Integration
   +
Security
   +
Testing
   +
Deployment
   +
Architecture
   +
Interview Preparation
        ↓
🔥 COMPLETE AGENTFORCE / AGENTBLAZER READINESS
```

---

## ⭐ Repository Principle

> **Don't just complete Trailhead. Understand it. Build it. Break it. Debug it. Integrate it. Secure it. Architect it.**

Every new Agentblazer learning module, Trailhead unit, hands-on challenge, Superbadge, project, architecture note, and implementation exercise will be added here under the appropriate level.

---

## Official Source

Salesforce Trailhead — Agentblazer:

https://trailhead.salesforce.com/agentblazer
