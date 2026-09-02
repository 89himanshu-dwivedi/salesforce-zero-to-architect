[Home](../index.md) / [17 · Salesforce Clouds](index.md) / **Service Cloud**

# Service Cloud

10 topics · Series 17: Salesforce Clouds

**Topics on this page**

- [Case Lifecycle](#case-lifecycle)
- [Case Assignment](#case-assignment)
- [Escalation](#escalation)
- [Omni Channel Routing](#omni-channel-routing)
- [Presence Status](#presence-status)
- [Entitlements](#entitlements)
- [Milestones](#milestones)
- [Service Console](#service-console)
- [Knowledge Base](#knowledge-base)
- [CTI Integration](#cti-integration)

## Case Lifecycle

*From case creation through resolution and closure.*

### 🌱 Simple

*Beginner - plain language*

The **Case Lifecycle** is the journey of a support case from **creation** (any channel) through triage, assignment, work, and **resolution/closure**, tracked by status and driven by automation and SLAs.

### 📏 Limits

*Governor & platform limits*

- Multichannel intake; status/support process; routing.
- Entitlements/Milestones SLAs; Knowledge; closure validation + CSAT.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Case Assignment

*Route cases to the right agent or queue.*

### 🌱 Simple

*Beginner - plain language*

**Case Assignment** automatically sets a case's owner using **assignment rules** (by type, priority, product, region) — to a user or **queue** — so cases reach the right team immediately.

### 📏 Limits

*Governor & platform limits*

- Ordered rules, first match, user/queue; auto on Email/Web.
- Omni for skills/capacity/presence; SLA reassignment.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Escalation

*Auto-raise priority/ownership on aging or breach.*

### 🌱 Simple

*Beginner - plain language*

**Escalation** automatically increases a case's priority or reassigns it (e.g., to a manager/tier-2 queue) when it ages or breaches an SLA — ensuring critical or stuck cases get attention.

### 📏 Limits

*Governor & platform limits*

- Escalation rules (business-hours timers) + milestone actions.
- Flow for complex paths; tiered reassignment + alerts.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Omni Channel Routing

*Real-time work distribution by skill and capacity.*

### 🌱 Simple

*Beginner - plain language*

**Omni-Channel Routing** pushes work items (cases, chats, calls, leads) to the **best available agent** in real time based on **skills, capacity, and presence** — replacing manual cherry-picking from queues.

### 📏 Limits

*Governor & platform limits*

- Service Channel + Routing Config + Presence + Queue.
- Capacity units; skills routing; push (no cherry-pick); Supervisor.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Presence Status

*Agent availability states controlling work routing.*

### 🌱 Simple

*Beginner - plain language*

**Presence Status** indicates an agent's availability (e.g., Available – Chat, Busy, Away). Omni-Channel only routes work to agents whose current status **accepts that channel** and who have spare capacity.

### 📏 Limits

*Governor & platform limits*

- Online (channel-mapped) vs Busy/Away; gates routing.
- Presence Config (capacity/auto-accept); time-in-status reporting.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Entitlements

*Define and enforce the support a customer is owed.*

### 🌱 Simple

*Beginner - plain language*

**Entitlements** represent the level of support a customer is entitled to — what service, for how long, under which SLA. They link to accounts/contacts/assets and drive **milestones** on cases.

### 📏 Limits

*Governor & platform limits*

- Links Account/Asset/Service Contract; coverage dates.
- Drives Entitlement Process (milestones); per-tier SLAs.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Milestones

*Time-based SLA steps with targets and actions.*

### 🌱 Simple

*Beginner - plain language*

**Milestones** are the required, time-bound steps within an entitlement process (e.g., First Response in 1 hour, Resolution in 24 hours). They track against **business hours** and trigger **actions** when met or breached.

### 📏 Limits

*Governor & platform limits*

- Business-hours target + completion; success/warning/violation actions.
- Stopped time (pause); sequential; console countdown.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Service Console

*Agent workspace: tabs, related work, productivity.*

### 🌱 Simple

*Beginner - plain language*

The **Service Console** is a Lightning console app giving agents a **multi-tab, split-view workspace** — case, customer context, knowledge, and tools in one screen — to resolve issues efficiently.

### 📏 Limits

*Governor & platform limits*

- Console nav (tabs/subtabs) + utility bar; Omni/CTI.
- Macros/quick text/shortcuts; Knowledge; lean for performance.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Knowledge Base

*Searchable articles for agents and self-service.*

### 🌱 Simple

*Beginner - plain language*

**Salesforce Knowledge** is a **knowledge base** of articles agents and customers use to resolve issues — with categories, search, versioning, and approval, surfaced in the console and self-service portals.

### 📏 Limits

*Governor & platform limits*

- Lightning Knowledge object; versioning/approval; channels.
- Data Categories classify + control visibility; KCS; AI search/deflection.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## CTI Integration

*Connect telephony to the agent workspace.*

### 🌱 Simple

*Beginner - plain language*

**CTI (Computer Telephony Integration)** connects a phone system to Salesforce so agents handle calls in the console — with screen pops, click-to-dial, call logging, and routing — via **Open CTI** or **Service Cloud Voice**.

### 📏 Limits

*Governor & platform limits*

- Open CTI (JS API, partner softphone) vs Service Cloud Voice (native AI).
- Screen pop, auto-log, Omni voice routing, transcription.

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
