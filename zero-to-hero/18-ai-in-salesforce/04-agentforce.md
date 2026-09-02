[Home](../index.md) / [18 · AI in Salesforce](index.md) / **Agentforce**

# Agentforce

7 topics · Series 18: AI in Salesforce

**Topics on this page**

- [Agent Builder](#agent-builder)
- [Topics](#topics)
- [Instructions](#instructions)
- [Actions](#actions)
- [Agent Testing](#agent-testing)
- [Agent Deployment](#agent-deployment)
- [Agent Analytics](#agent-analytics)

## Agent Builder

*Build autonomous AI agents declaratively.*

### 🌱 Simple

*Beginner - plain language*

**Agent Builder** is where you create **Agentforce agents** — autonomous AI assistants that reason over **topics, instructions, and actions** to handle tasks and conversations across Salesforce, without scripting every dialog.

### 📏 Limits

*Governor & platform limits*

- Topics + instructions + actions over Atlas Reasoning Engine (LLM).
- Grounded; Trust Layer; multi-channel; guardrails; test/deploy/monitor.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Topics

*Jobs an agent can handle, grouping actions.*

### 🌱 Simple

*Beginner - plain language*

**Topics** define the **jobs or subject areas** an Agentforce agent can handle — like "Order Management" or "Returns" — each grouping the relevant **instructions and actions** for that job.

### 📏 Limits

*Governor & platform limits*

- Topic = classification + instructions + actions; engine routes to it.
- Distinct, well-scoped descriptions critical; test routing.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Instructions

*Natural-language rules guiding agent behavior.*

### 🌱 Simple

*Beginner - plain language*

**Instructions** are **natural-language rules** that tell an Agentforce agent **how to behave** — tone, what to do or avoid, when to use actions, and when to escalate — guiding the reasoning engine.

### 📏 Limits

*Governor & platform limits*

- Agent + topic level; tone/actions/data/escalation rules.
- Guide not hard-enforce; pair with permission guardrails; refine from transcripts.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Actions

*What an agent can actually do — Flows, Apex, prompts, APIs.*

### 🌱 Simple

*Beginner - plain language*

**Actions** are the **capabilities** an agent can execute to get work done — run a **Flow**, call **Apex**, use a **prompt template**, or hit an **API** — turning conversation into real outcomes.

### 📏 Limits

*Governor & platform limits*

- Flow/Apex/prompt/API actions; description + typed I/O guide invocation.
- Permission-scoped; validate inputs; safeguard destructive ops; reusable.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Agent Testing

*Validate agent behavior before and after deploy.*

### 🌱 Simple

*Beginner - plain language*

**Agent testing** validates that an Agentforce agent **routes correctly, follows instructions, and uses actions safely** — using the Agentforce **Testing Center** and conversation previews before going live.

### 📏 Limits

*Governor & platform limits*

- Testing Center batch cases + evaluations; routing/action/grounding/safety.
- Adversarial + edge cases; regression on every change; auto + human eval.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Agent Deployment

*Roll agents to channels safely and reversibly.*

### 🌱 Simple

*Beginner - plain language*

**Agent deployment** publishes an Agentforce agent to **channels** — web, Slack, mobile, service, internal/external — and moves it through environments (sandbox → production) with proper change management.

### 📏 Limits

*Governor & platform limits*

- Metadata via DevOps pipeline; channels; scoped agent-user permissions.
- Gradual rollout; versioning/rollback; environment parity; monitoring.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Agent Analytics

*Measure agent usage, quality, and outcomes.*

### 🌱 Simple

*Beginner - plain language*

**Agent analytics** track how an agent performs — **conversations, resolution/deflection, escalations, action usage, and satisfaction** — so you can measure value and improve it.

### 📏 Limits

*Governor & platform limits*

- Usage + quality (resolution/escalation/action) + outcome (CSAT/ROI) metrics.
- Transcript analysis → refine topics/instructions/actions; alerting; audit.

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
