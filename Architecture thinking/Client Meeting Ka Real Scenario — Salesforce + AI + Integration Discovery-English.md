# 🎯 Real Client Meeting Scenario — A Storm of Requirements

Client meetings often bring requirements all at once:

**Fast + Secure + Offline + Pretty + AI + Integration + Aggressive Rollout + Low Cost**

At the Senior Salesforce Developer / Lead level, an architect's job is **not to panic and say “yes” to everything**.

The architect's job is:

> **Clarify requirements, identify priorities, understand constraints, and build a justified architecture.**

---

# 13.1 Typical Client Wishlist — Everything, Right Now, Cheap

A client may say:

📱 **“We need a mobile solution.”**

⚡ **“It should be very fast.”**

🏃 **“We need the rollout in 3 months.”**

🔒 **“It should be highly secure.”**

📴 **“It should also work offline.”**

✨ **“The UI should be beautiful and easy.”**

🤖 **“Add AI too.”**

🔗 **“We need to integrate both Salesforce and SAP.”**

💰 **“The budget should also remain under control.”**

And sometimes:

> **“Sir, it’s just mobile. How difficult can it be?”**

### ⚠️ This Is Where the Architect Should Stop and Start Discovery

These requirements may look individually simple.

But combined requirements:

**Offline + Real-time + Secure + Cheap + Fast + AI + Aggressive Timeline**

can make the architecture significantly more complex.

---

# 13.2 The Architect's Job — Drive Discovery

The architect's role is not to immediately convert every client demand into a solution.

First understand:

**What is the actual business meaning of the requirement?**

### Requirement Discovery Flow

```text
Client Requirement
       ↓
Clarifying Questions
       ↓
Business Priority
       ↓
Technical Constraint
       ↓
Options
       ↓
Trade-off
       ↓
Architecture
```

### 🔹 Salesforce Example

Client:

> **“We need a mobile Salesforce solution that also works offline.”**

Senior-level architect questions:

**What exactly needs to be done offline?**

- Read data?
- Create records?
- Update records?
- Complete case workflow?

**How many records will be available offline?**

**How will data security be handled if the device is lost?**

**When will data sync?**

**How will conflict resolution work?**

**Android, iOS, or both?**

**Which Salesforce objects are required?**

Now the requirement starts becoming meaningful.

---

# 13.3 Toolkit Approach — Keep Questions Ready Alongside Requirements

Instead of panic:

> **Checklist + Questions + Trade-offs**

Use them.

Every requirement will have some key questions behind it.

---

## 📱 Requirement: Mobile

Questions:

- Native app or cross-platform?
- Android / iOS / both?
- Is Salesforce mobile capability sufficient?
- Is a custom UX required?
- Is offline required?

### Salesforce Perspective

Possible options:

**Salesforce Mobile / Mobile SDK / LWC-based experience / Custom Mobile App**

The decision will be driven by the use case.

---

## ⚡ Requirement: Fast

Ask what “fast” exactly means.

- Page load < 2 sec?
- API response < 500 ms?
- Search < 1 sec?
- Can bulk operations run in the background?

### Integration Example

```text
Salesforce
   ↓
Middleware
   ↓
External API
```

If the API is slow, making the Salesforce UI directly dependent on a synchronous call can be a problem.

Possible solution:

**Async processing + caching + queue**

---

## 📴 Requirement: Offline

Offline does not simply mean:

> **“There is no internet and the app is still running.”**

It also raises the actual questions:

- Which data will be stored locally?
- For how long?
- Encryption?
- Sync strategy?
- Conflict handling?
- Device security?
- What happens when data becomes stale?

### Salesforce Example

A Field Service user is at a remote location.

```text
Salesforce Data
      ↓
Device Local Data
      ↓
Offline Work
      ↓
Internet Available
      ↓
Sync
      ↓
Conflict Resolution
```

👉 The offline requirement can become a **major architecture driver**.

---

# 13.4 Security — “It Should Be Secure” Is Not a Sufficient Requirement

If the client says:

> **“The application should be highly secure.”**

The architect should immediately ask:

**Secure against what?**

- Unauthorized access?
- Data theft?
- Device loss?
- API abuse?
- PII exposure?
- Regulatory compliance?

### Salesforce Security Questions

- Authentication?
- OAuth?
- Permission Sets?
- Sharing?
- Field-Level Security?
- Encryption?
- Integration user?
- Secrets management?

### 🤖 AI Security

If AI is involved:

```text
Salesforce Data
      ↓
Data Validation / Masking
      ↓
AI Layer
      ↓
Response Validation
      ↓
Salesforce
```

Questions:

- Will sensitive customer data be sent to the LLM?
- Which approved model/provider?
- Prompt injection risk?
- Output validation?
- Audit trail?

👉 **Convert “Secure” into a measurable requirement.**

---

# 13.5 “Pretty + Fast + Easy” — Measure UX

If the client says:

> **“The UI should be pretty, fast, and easy.”**

The architect should clarify:

**Pretty → What is the design standard?**

**Fast → What is the performance target?**

**Easy → How many steps should there be in the user journey?**

### Salesforce Example

For case creation, the user does not need:

**12 screens**

A possible workflow:

```text
Open Case
    ↓
AI Summary
    ↓
Suggested Action
    ↓
Approve
    ↓
Close / Escalate
```

Here both architecture and UX are aligned with the business outcome.

---

# 13.6 🤖 AI Requirement — “Add AI Too”

This is a common client statement.

The architect should ask:

**What actual business problem will AI solve?**

Possible use cases:

- Case summarization
- Email generation
- Knowledge Q&A
- Classification
- Recommendation
- Agent assistance
- Data extraction
- Workflow automation

### Salesforce Example

Client:

> “We want to add AI to customer service.”

Better discovery:

```text
Problem
  ↓
High Case Volume
  ↓
Manual Case Analysis
  ↓
AI Summarization
  ↓
Suggested Next Action
  ↓
Agent Approval
```

Here AI has been defined as a **business capability** rather than a **technology feature**.

---

# 13.7 🔗 Integration Requirement — “We’ll Connect It to Salesforce”

Client:

> **“We need to integrate Salesforce with SAP and the billing system.”**

The architect should ask:

### Data

- What data will be exchanged?
- What is the direction?
- Frequency?
- Volume?

### Pattern

- Synchronous?
- Asynchronous?
- Event-driven?
- Batch?

### Reliability

- Retry?
- Timeout?
- Duplicate handling?
- Dead-letter/error queue?

### Security

- OAuth?
- Certificates?
- API gateway?
- Encryption?

### Salesforce Limits

- Callout limits?
- Async processing?
- Bulkification?
- Transaction boundaries?

### Example

```text
Salesforce
     ↓
MuleSoft / Integration Layer
     ↓
SAP
     ↓
Billing
```

The architect's goal is not merely to build the integration.

The goal is:

**Reliable + Secure + Maintainable + Scalable Integration**

---

# 13.8 Aggressive Rollout — Don't Challenge the Timeline, Decompose It

Client:

> **“We need the complete rollout in 3 months.”**

Directly saying:

> “Yes.”

is risky.

The architect should break it down:

```text
MVP
 ↓
Pilot
 ↓
Phase 1
 ↓
Phase 2
 ↓
Enterprise Rollout
```

### Salesforce Example

**Phase 1**

Case Management + basic integration

**Phase 2**

AI Case Summary

**Phase 3**

Advanced automation + analytics

**Phase 4**

Global rollout

This can convert an aggressive timeline into manageable increments.

---

# 13.9 Conflicting Requirements — This Is Where the Architect's Value Shows

The client wants:

**100% Offline + Real-time data + Zero latency + Very low cost**

Do not assume that all requirements can be perfectly satisfied at the same time.

The architect must:

**Identify Trade-off → Explain → Prioritize → Document**

### Example

| Requirement | Impact |
|---|---|
| Offline | Local storage + sync complexity |
| Real-time | Connectivity dependency |
| High Security | Additional controls |
| Low Cost | Less flexibility |
| Fast Rollout | Scope reduction needed |

### Architect Conversation

> “We can support offline capability, but it introduces local data storage and synchronization complexity. We should therefore define which data truly needs to be available offline.”

This is senior-level architectural communication.

---

# 13.10 Discovery Checklist — Salesforce + AI + Integration

As soon as you hear a client requirement, mentally run this checklist:

```text
1. Business Outcome?
        ↓
2. Functional Requirement?
        ↓
3. Non-Functional Requirement?
        ↓
4. Salesforce Capability?
        ↓
5. Integration Requirement?
        ↓
6. AI Opportunity?
        ↓
7. Security / Compliance?
        ↓
8. Scale / Volume?
        ↓
9. Offline / Sync?
        ↓
10. Timeline / Budget?
        ↓
11. Risks?
        ↓
12. Rollout Strategy?
```

---

# 🧠 General Architect Rule

Do not directly interpret the client's words as architecture.

Convert:

**“We need it fast”**

→ measurable performance requirement

**“We need it secure”**

→ security controls

**“We need AI”**

→ business use case

**“We need integration”**

→ data flow + integration pattern

**“We need offline”**

→ local data + synchronization strategy

**“We need it quickly”**

→ MVP + phased rollout

---

# 🤖 Salesforce + AI + Integration Discovery Flow

```text
Client Says:
"Fast, Secure, Offline, AI + Salesforce"

          ↓

Discovery

          ↓

Business Outcome
          +
Functional Requirements
          +
NFRs
          +
Constraints

          ↓

Salesforce Capability
          +
Integration Pattern
          +
AI Capability

          ↓

Security
          +
Scalability
          +
Risk

          ↓

MVP / Phased Rollout

          ↓

Final Architecture
```

---

# 🎯 Senior Salesforce Developer / Lead Interview Angle

If an interview asks a requirement-discovery question, do not just name technologies.

Strong thought process:

> **“First I clarify the business outcome and convert vague requirements into measurable functional and non-functional requirements. Then I evaluate what Salesforce can handle natively, where integration or middleware is required, whether AI genuinely adds business value, and finally assess security, scalability, risks, and rollout strategy before selecting the architecture.”**

---

# 📝 Exam Point

**Architect ≠ Requirement Collector**

An architect is a:

**Requirement Clarifier + Trade-off Evaluator + Solution Designer + Risk Owner + Technology Advisor**

### 🔥 One-Line Memory Trick

**Ask → Clarify → Prioritize → Evaluate → Design → Validate → Rollout**

And most importantly:

> **“The architect does not blindly implement what the client says; the architect discovers the actual business meaning of that requirement.”**

# ✅ CTA / Architecture Exam Perspective

In an exam scenario, the client is not physically in front of you.

Therefore, ask yourself:

**What? → Why? → How much? → How fast? → How secure? → What if it fails? → What happens at scale?**

This **discovery mindset** is useful in Salesforce + Integration + AI architecture questions.
