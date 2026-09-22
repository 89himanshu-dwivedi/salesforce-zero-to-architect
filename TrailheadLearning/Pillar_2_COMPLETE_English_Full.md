# 🔐 Pillar 2 — Manage Karna Aasaan Banao
## DSAR Process + Least Privilege + Salesforce + Integration + AI

In Pillar 1 we learned:

> **Find the Data**

Now the question in Pillar 2 is:

> **"If a customer makes a request about their data, how do we respond efficiently?"**

The core is:

**DSAR Process + Data Access + Security + Least Privilege**

---

# 24.1 📤 DSAR — "What Data Do You Have About Me?"

DSAR = **Data Subject Access Request**

In simple language:

> **"What data do you have about me, how are you using/processing it, and what information can I receive under applicable rights?"**

Exact rights and response requirements depend on the applicable regulation.

---

# 🔥 Why Is DSAR Difficult?

The problem is that customer data is not stored in only one system.

Example:

```text id="2f8m6r"
Customer
   ↓
Salesforce
   ↓
MuleSoft
   ├── ERP
   ├── Billing
   ├── Marketing
   └── Support
           ↓
       Data Lake
           ↓
           AI
```

The customer says:

> **"Show me all my data."**

The architect needs to ask:

- What is in Salesforce?
- What is in the ERP?
- What is in Billing?
- What is in Marketing?
- What is in the Data Lake?
- Is there relevant data in AI/vector systems?
- Which systems consume the data?

---

# 🔄 DSAR Process

A proper process can look like this:

```text id="v4m7x2"
DSAR Request
      ↓
Identity Verification
      ↓
Request Classification
      ↓
Data Discovery
      ↓
System Owners / APIs
      ↓
Data Collection
      ↓
Validation
      ↓
Response Generation
      ↓
Audit
      ↓
Customer
```

### 🧠 Important

DSAR does not simply mean:

**"Just retrieve the Contact record with SOQL."**

It is not that simple.

It can be an **enterprise data orchestration problem**.

---

# ☁️ Salesforce Example

Customer request:

> "Please provide the personal information you hold about me."

Possible Salesforce data:

```text id="s3q8n1"
Account
Contact
Cases
Orders
Preferences
Activities
```

But outside Salesforce:

```text id="d6k2p9"
Salesforce
   ↓
MuleSoft
   ↓
SAP
   ↓
Marketing Cloud
   ↓
Data Warehouse
```

Therefore, the DSAR architecture may need to support cross-system data retrieval.

---

# 👨‍💻 Senior Developer / Lead Perspective

You should not immediately start coding.

First ask these questions:

### 1. Identity

**Is the requester actually the customer?**

### 2. Scope

**What data is covered by the request?**

### 3. Systems

**Which systems contain the data?**

### 4. Retrieval

**Manual, API-based, or automated?**

### 5. Security

**Which employee/system is permitted to view the data?**

### 6. Audit

**Who processed the request and what data was accessed?**

---

# 24.2 🧰 Manual to Fully Automated — Spectrum

You do not need to wait for perfect automation.

Think of privacy maturity as a spectrum.

---

# 🟢 Level 1 — Minimum Viable DSAR

Simple document:

```text id="p8w3y1"
DSAR Received
      ↓
Verify Customer
      ↓
CRM → Owner
      ↓
Billing → Owner
      ↓
Marketing → Owner
      ↓
Collect Data
      ↓
Validate
      ↓
Respond
```

Maintain the following in one document:

- Steps
- System owners
- Contacts
- Data locations
- Response process

maintain.

### Important

**Manual ≠ Bad Architecture**

If the process is documented and repeatable, it can be a useful starting point.

---

# 🟡 Level 2 — Structured Workflow

You can create a privacy-request record in Salesforce:

```text id="u7c4s9"
Privacy Request
      ↓
Status
      ↓
Assigned Owner
      ↓
Systems
      ↓
Tasks
      ↓
Due Date
      ↓
Validation
      ↓
Complete
```

Example custom object:

**Privacy_Request__c**

Potential fields:

- Request Type
- Customer
- Status
- Received Date
- Due Date
- Request Owner
- Verification Status
- Completion Status

---

# 🟠 Level 3 — API-Based

Next level:

```text id="m2q8v6"
Customer Portal
      ↓
Privacy Request
      ↓
Salesforce
      ↓
MuleSoft
      ↓
ERP / Billing / Marketing
      ↓
Data Aggregation
      ↓
Salesforce
```

System APIs can automatically retrieve data.

---

# 🔴 Level 4 — Fully Automated

Ideal enterprise pattern:

```text id="a5f9k3"
Customer
   ↓
Privacy Portal
   ↓
Identity Verification
   ↓
Privacy Orchestrator
   ↓
 ┌─────────┬─────────┬─────────┐
 ↓         ↓         ↓
Salesforce ERP    Marketing
 ↓         ↓         ↓
 └─────────┴─────────┘
           ↓
     Data Aggregation
           ↓
       Validation
           ↓
       Response
           ↓
        Audit
```

### 🎯 Architect Principle

> **Automation is the destination, but repeatability is the first requirement.**

---

# 🤖 AI Use Case — DSAR

AI can be given an assistive role in the DSAR process.

Example:

```text id="w1x6r8"
Customer Message
      ↓
AI Classification
      ↓
"Access Request"
      ↓
Salesforce Privacy Workflow
      ↓
Data Collection
      ↓
Human / Policy Validation
      ↓
Response
```

AI can be useful for:

- Request classification
- Intent detection
- Routing
- Summarization
- Draft response

### ⚠️ Important

AI should not automatically:

**"Ye request legally valid hai, delete everything."**

It should not be treated as the decision-maker for such sensitive privacy decisions.

For sensitive privacy decisions:

**Policy + deterministic workflow + appropriate human review**

can be important.

---

# 24.3 🔐 Security Review — Least Privilege

The second major part of the DSAR process:

> **"Who can access customer data?"**

Core principle:

> **Least Privilege**

Meaning:

**Only the access required for the task, by the appropriate user/system, for the appropriate time/context.**

---

# ❌ Bad Design — Blanket Access

```text id="k9r2v5"
Support User
      ↓
Full Customer Database
```

Problem:

If the account is compromised:

**Large data exposure**

---

# ✅ Better Design — Contextual Access

Suppose a support agent is handling a customer call.

The agent needs:

```text id="f7s3w2"
Customer
  ↓
Current Case
  ↓
Relevant Order
  ↓
Required Contact Details
```

They need this.

Not:

```text id="e5m8q1"
All Customers
All Orders
All Billing
All Historical Data
```

---

# ☁️ Salesforce Security Model

For a Senior Salesforce Developer / Lead, think about security in multiple layers:

```text id="b3n7x4"
Authentication
      ↓
Authorization
      ↓
Object Permissions
      ↓
Field-Level Security
      ↓
Record-Level Sharing
      ↓
Apex / API Enforcement
      ↓
Integration Security
```

### Important

Even when writing Apex code:

**"Does the user have access?"**

It is important to check this.

Sirf UI hide kar dena security It is not that simple.

---

# 🔗 Integration User — Special Risk

A common problem in enterprise integration:

```text id="r4k8m2"
Integration User
      ↓
Salesforce
      ↓
Everything
```

If the integration user receives unnecessary permissions:

**One compromised integration credential → large data exposure**

Better:

```text id="t6p1y9"
Integration
    ↓
Specific API
    ↓
Specific Objects
    ↓
Specific Fields
    ↓
Required Operations
```

---

# 🤖 AI Agent — Least Privilege

This is even more important for AI agents.

### ❌ Bad

```text id="n8q4s6"
AI Agent
    ↓
System Admin
    ↓
Full Salesforce Access
```

### ✅ Better

```text id="c2v7m5"
AI Agent
    ↓
Identity / Context
    ↓
Authorization
    ↓
Allowed Data
    ↓
Allowed Actions
    ↓
Salesforce
```

Example:

AI The agent needs:

✅ Case read karna  
✅ Knowledge search karna  
✅ Draft response banana  

is allowed.

But:

❌ User permissions modify karna  
❌ Arbitrary Account deletion  
❌ Full customer database export  

should not be allowed unless explicitly justified and controlled.

---

# 🎯 Contextual Access

A very important point:

> **"Think in terms of scenarios."**

Question:

> **"Who should access this data in which situation?"**

Example:

### Normal

Customer service agent:

**Current customer → Current case → Relevant information**

### Special

Fraud investigation:

**Fraud team → Extended transaction history**

### Admin

System administrator:

**Administrative troubleshooting access**

However:

**Admin ≠ Everyone gets unrestricted business data by default.**

Access should still be governed and audited.

---

# 🧠 Dynamic / Temporary Access

In an advanced architecture, access can be restricted by:

**time + context + purpose**

 

Example:

```text id="v9m3q2"
Support Agent
     ↓
Customer Call
     ↓
Temporary Context
     ↓
Required Data
     ↓
Case Closed
     ↓
Access Ends
```

This can reduce the exposure surface.

---

# 🔥 DSAR + Least Privilege Together

Interesting architecture challenge:

The DSAR processor may need a **broad view of the customer's data**.

But that does not mean:

> **"Every employee gets access to every customer's data."**

Better:

```text id="h6w2p8"
DSAR Request
      ↓
Authorized Privacy Team
      ↓
Controlled Retrieval
      ↓
System APIs
      ↓
Minimum Necessary Human Access
      ↓
Audit
```

Data collection can be automated while human access remains tightly controlled.

---

# 🚨 Errors & Gotchas

### ❌ "DSAR ke liye ek SOQL query enough hai."

Not necessarily.

Data may exist across multiple systems.

### ❌ "Manual process means non-compliant."

Not necessarily.

A **documented + repeatable + controlled** process can be a valuable starting point.

### ❌ "Automation = AI."

No.

DSAR automation can also be done through deterministic APIs/workflows.

### ❌ "Give support users the complete customer history."

Evaluate need-to-know access.

### ❌ "Give the integration user System Admin."

Avoid unnecessary privilege.

### ❌ "Give the AI agent admin access."

High-risk architecture.

### ❌ "We hid the button in the UI, so the data is secure."

Backend/API/Apex/security enforcement is also required.

---

# 💼 Interview Question

## Q: How would you design a DSAR process in Salesforce?

### Strong Answer

> **"I would first verify the requester's identity and classify the request. Then I would use the data inventory from the discovery process to identify Salesforce and downstream systems containing relevant customer data. Initially this can be a documented manual workflow, but at scale I would introduce a Salesforce privacy-request workflow with API-based orchestration through the integration layer. Data retrieval should follow least-privilege access, maintain an audit trail, and include validation before the final response. AI can assist with request classification and summarisation, but sensitive decisions and data access should remain governed by deterministic controls and appropriate human oversight."**

---

# 🧠 Super-Advanced Architect Thinking

Do not think of DSAR architecture only as:

**Request → Response**

Instead, think of it as:

Actually:

```text id="p7k2v4"
IDENTITY
   ↓
AUTHORIZATION
   ↓
DISCOVERY
   ↓
RETRIEVAL
   ↓
FILTERING
   ↓
VALIDATION
   ↓
RESPONSE
   ↓
AUDIT
   ↓
RETENTION
```

Every step is a security boundary.

---

# 🔥 Real-World Salesforce + Integration + AI Scenario

## Requirement

A global customer submits a request through Salesforce:

> **"Show me the personal data your company holds about me."**

### Architecture

```text id="s8m4x1"
Customer Portal
      ↓
Identity Verification
      ↓
Salesforce Privacy Request
      ↓
Privacy Orchestrator
      ↓
 ┌─────────┬──────────┬───────────┐
 ↓         ↓          ↓
Salesforce ERP      Marketing
 ↓         ↓          ↓
 └─────────┴──────────┘
             ↓
        Data Aggregation
             ↓
       Sensitive Data Filter
             ↓
       Human Validation
             ↓
          Response
             ↓
           Audit
```

### AI Assistance

```text id="w5q9n2"
Customer Message
      ↓
AI
      ↓
Request Classification
      ↓
DSAR Workflow
```

AI:

**classify + route + summarise**

Workflow:

**authorize + retrieve + validate + respond**

This separation is important.

---

# 📌 Pillar 2 — Quick Revision

## 📤 DSAR

**"What data do you have about me?"**

## 🧰 Manage

**Document → Workflow → API → Automation**

## 🔐 Security

**Who can access what?**

## 🎯 Least Privilege

**Only required data + required user + required context**

## ☁️ Salesforce

**Sharing + FLS + Permission Sets + Apex/API Security**

## 🔗 Integration

**Controlled system-to-system data retrieval**

## 🤖 AI

**Classify + Route + Summarize**

Not:

**Unrestricted data access + autonomous privacy decisions**

---

# ⭐ Final Architect Formula

```text id="d4r8m6"
             DSAR
               ↓
       Verify Identity
               ↓
        Find Relevant Data
               ↓
      Salesforce + Integration
               ↓
        Least Privilege
               ↓
       Collect / Validate
               ↓
        Generate Response
               ↓
            Audit
```

## 🧠 One-Line Architect Mindset

> **"A good DSAR architecture makes the process repeatable and auditable, while ensuring that even the people processing the request receive only the access they actually need."**

### Remember

**Pillar 1 → FIND the data**

**Pillar 2 → MANAGE the request**

**Security → Least Privilege**

**Salesforce → Controlled data access**

**Integration → Cross-system orchestration**

**AI → Assist, don't bypass governance**
