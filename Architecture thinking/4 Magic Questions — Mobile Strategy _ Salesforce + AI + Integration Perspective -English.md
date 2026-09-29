# 📱 4 Magic Questions — The Complete Mobile Strategy Framework

When building a mobile strategy, a Salesforce Architect / Senior Developer should answer 4 core questions:

**1. Salesforce Mobile App or Custom App?**  
**2. If Custom, HTML5, Native, or Hybrid?**  
**3. How will security work?**  
**4. How will testing work?**

The answers to these 4 questions together create a **documentable mobile strategy**.

---

# 14.1 The Four Questions — At a Glance

In a client meeting, simply write down these 4 questions.

## 1️⃣ Salesforce App or Custom App?

The most important architectural decision.

Possible options:

**Salesforce Mobile App**

or

**Custom Mobile App**

The decision depends on:

- Business requirements
- Salesforce dependency
- User experience
- Offline requirement
- Device capabilities
- Integration requirements
- Cost
- Timeline

### 🔹 Salesforce Example

Field Sales users need:

**Customer → Account → Opportunity → Update Visit**

If the requirement is mostly centered around Salesforce data/processes, the Salesforce mobile experience may be suitable.

### 🤖 AI Use Case

On mobile, a sales representative could see:

**Customer → AI-generated account summary → recommended next action**

The AI capability can be integrated with the existing Salesforce experience.

---

## 2️⃣ If Custom — HTML5, Native, or Hybrid?

If a custom app is required, the next question is:

**Which technology/platform should be used to build it?**

Options:

**HTML5 / Web App**

**Native iOS / Android**

**Hybrid / Cross-platform**

The decision depends on:

- Development skills
- Budget
- Timeline
- Device capabilities
- Performance
- Offline support
- Integrations
- Security

### 🔹 Example

If a simple customer portal is needed:

**HTML5 / responsive web app**

If deep device capabilities are needed:

**Native**

If multiple platforms need to be targeted from the same codebase:

**Cross-platform / Hybrid**

### 🤖 AI Use Case

In an AI-powered mobile app:

```text
Mobile App
    ↓
API Layer
    ↓
AI Service
    ↓
Response
```

The architect must consider **API latency, authentication, and AI response time** along with the UI technology.

👉 **Exam Point:** Choosing a custom app is not just a coding preference; the decision is based on **business need + device capability + cost + skills**.

---

# 14.2 Why Is the Order Important?

The answer to Q1 determines whether Q2 is necessary.

## Question 1

**Salesforce App or Custom App?**

### ✅ Salesforce App

If the Salesforce mobile app is sufficient:

**Skip Q2**

Go directly to:

**Q3 Security → Q4 Testing**

### ❌ Custom App

If the Salesforce app does not fulfill the requirement:

**Q2 is mandatory**

Then:

**HTML5 / Native / Hybrid**

After that:

**Security → Testing**

---

## 🔄 Decision Flow

```text
Salesforce App?
      │
      ├── Yes
      │    ↓
      │ Security
      │    ↓
      │ Testing
      │
      └── No
           ↓
        Custom App
           ↓
      HTML5 / Native / Hybrid
           ↓
         Security
           ↓
         Testing
```

### 👨‍💻 Senior Developer / Lead Perspective

Do not answer Q1 too quickly.

First understand:

**Who are the users?**

**What do they need to do?**

**Do they need offline support?**

**Do they need camera/GPS/biometrics?**

**Is Salesforce the primary system?**

**Are multiple external systems involved?**

Then decide the app choice.

---

# 14.3 Question 3 — How Will Security Work?

In mobile architecture, security is not limited to login.

Consider:

- Authentication
- Authorization
- Data encryption
- Device security
- BYOD
- MDM/MAM
- Session management
- Secure storage
- App distribution
- API security
- Salesforce Connected App / OAuth
- Compliance

### 🔹 Salesforce Example

```text
Mobile App
    ↓
OAuth / Identity
    ↓
Salesforce
    ↓
Data Access
```

Question:

**Which Salesforce data should the user be able to access?**

Here:

**Profiles / Permission Sets / Sharing / OAuth scopes**

may be important.

### 🤖 AI Use Case

A mobile user asks an AI assistant for customer information.

Risk:

> The user could receive customer data that they should not have access to.

Ensure the architecture follows:

```text
User
 ↓
Authentication
 ↓
Authorization
 ↓
Allowed Salesforce Data
 ↓
AI / Agent
 ↓
Response
```

AI should **not bypass authorization**.

### ⚠️ Important

For AI use cases, additionally:

**PII protection + prompt/data controls + auditability + guardrails**

are important.

---

# 14.4 Question 4 — How Will Testing Be Done?

Ignoring testing in mobile projects is dangerous.

Consider testing:

- Different devices
- Different OS versions
- Different screen sizes
- Network conditions
- Offline/online scenarios
- Authentication
- API failures
- Salesforce integration
- Performance
- Security

### 🔹 Example

The app works properly on Android, but:

**iPhone + slow network + old OS**

causes an API timeout.

To avoid a production issue, device-matrix and network testing are required.

### 🤖 AI Use Case

For AI response testing, UI testing alone is not enough.

Check:

**Correctness + latency + timeout + fallback + unsafe output + API failure**

Example:

```text
Mobile
  ↓
AI Request
  ↓
Timeout?
  ├── No → Response
  └── Yes
       ↓
     Retry / Fallback
       ↓
     Human Support
```

---

# 14.5 Final Output of the Four Questions

```text
Q1 Architecture
Salesforce App vs Custom App
        ↓
Q2 Platform
HTML5 / Native / Hybrid
        ↓
Q3 Security
Identity + Device + Data + API
        ↓
Q4 Testing
Device + Network + Integration + AI
        ↓
Mobile Strategy Document
```

---

# 📋 Strategy Decision Table

| Question | What Is Decided | Salesforce / AI Angle |
|---|---|---|
| Q1 · Salesforce vs Custom | Overall app architecture | Salesforce-first or custom experience |
| Q2 · Platform | Development approach, cost, timeline | Mobile UI + API integration |
| Q3 · Security | Access, storage, device & API security | OAuth, permissions, data protection, AI guardrails |
| Q4 · Testing | Quality & rollout confidence | Salesforce + API + AI + device testing |

---

# 🔗 Salesforce + Integration View

A real enterprise mobile architecture could look like this:

```text
                  Mobile User
                      ↓
               Mobile Application
                      ↓
               Identity / OAuth
                      ↓
                 API Gateway
                  ↙       ↘
           Salesforce    Middleware
               ↓             ↓
           CRM Data      ERP / Legacy
                  \       /
                   \     /
                    AI / Agent
                      ↓
               Business Response
```

The architect must decide:

**Direct Salesforce integration or middleware?**

**Sync or async?**

**Real-time or eventual consistency?**

**How will offline data be handled?**

**What is the fallback for API failure?**

**What data will the AI receive?**

---

# 🤖 AI + Salesforce Mobile Example

### Business Requirement

A Field Service agent is at a customer site and wants to quickly understand the customer's issue.

### Possible Flow

```text
Field Agent Mobile
       ↓
Salesforce Customer
       ↓
AI Summary
       ↓
Recent Cases + Orders + Knowledge
       ↓
Recommended Action
       ↓
Agent Decision
```

AI provides **decision support** here, while Salesforce remains the system of record.

---

# 🧠 Magic 4 — Easy Memory Trick

```text
1️⃣ WHICH APP?
Salesforce or Custom

        ↓

2️⃣ WHICH PLATFORM?
HTML5 / Native / Hybrid

        ↓

3️⃣ HOW SECURE?
Identity / Data / Device / API

        ↓

4️⃣ HOW TESTED?
Device / Network / Integration / AI
```

## 🎯 Architect Mindset

> **“Mobile strategy does not simply mean selecting an app. It means designing the correct platform, secure Salesforce/integration architecture, AI usage, and reliable testing together.”**

## 💼 Senior Salesforce Developer / Lead Interview Line

> **“I first determine whether the Salesforce mobile experience can satisfy the business requirement. If not, I evaluate a custom mobile approach based on device capabilities, offline needs, integration complexity, security, cost and delivery timeline. Then I define the Salesforce/API/AI integration, security model and testing strategy.”**

## ✅ One-Minute Revision

**Q1 → What will the app be?**  
**Q2 → What technology will it use?**  
**Q3 → How will it be secured?**  
**Q4 → How will it be tested?**

**Salesforce + Mobile + Integration + AI + Security = Complete Mobile Strategy**
