[Home](../index.md) / [23 · Production Support & Client Handling](index.md) / **Client Handling & Communication**

# Client Handling & Communication

10 topics · Series 23: Production Support & Client Handling

**Topics on this page**

- [Client Not Happy Recovery](#client-not-happy-recovery)
- [Acknowledge Then Investigate](#acknowledge-then-investigate)
- [Set Realistic Timelines](#set-realistic-timelines)
- [Explain Tech to Non-Tech](#explain-tech-to-non-tech)
- [Manage Scope Creep](#manage-scope-creep)
- [Status Updates Cadence](#status-updates-cadence)
- [Handle Angry Escalation](#handle-angry-escalation)
- [Under-Promise Over-Deliver](#under-promise-over-deliver)
- [Say No Professionally](#say-no-professionally)
- [Build Long-Term Trust](#build-long-term-trust)

## Client Not Happy Recovery

*Turn an unhappy client back into a confident one.*

### 🌱 Simple

*Beginner - plain language*

When a client is unhappy (a bug, a missed date, a production issue), recovery is more about **communication and ownership** than code. The pattern: **acknowledge, take ownership, fix with a clear plan, over-communicate progress, and follow up**. People forgive problems handled well; they remember problems handled badly.

### 📏 Limits

*Governor & platform limits*

- You cannot fix a relationship while the system is still broken - stabilise first.
- Contractual SLAs and penalties may already be in play; involve your account lead early.
- Some root causes (Salesforce known issues, vendor outages) are outside your control - manage expectations honestly.
- Recovery takes weeks of consistency; a single good meeting does not reset perception.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Acknowledge Then Investigate

*Respond fast even before you have the answer.*

### 🌱 Simple

*Beginner - plain language*

You don't need the solution to respond — you need to **acknowledge**. A quick "I've got this, investigating now, update by X" buys you time and signals control. Silence while you dig is what makes clients anxious and escalate.

### 📏 Limits

*Governor & platform limits*

- SLA response clocks usually start at report time, not at your acknowledgement.
- Written channels create a record - assume anything you write may be quoted back.
- Acknowledgement does not accept liability, but careless wording can imply it.
- Time zones matter for global clients - state them explicitly in commitments.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Set Realistic Timelines

*Commit to dates you can actually hit.*

### 🌱 Simple

*Beginner - plain language*

Trust is built on **kept commitments**. Pad estimates for testing, review, and the unexpected. It's better to say "by Thursday" and deliver Wednesday than promise "tomorrow" and slip. When unsure, commit to a **next-update time** rather than a delivery time.

### 📏 Limits

*Governor & platform limits*

- Deploy windows, change freezes and CAB approval often set the real floor.
- Apex test execution can add 30+ minutes to every production deploy.
- Sandbox refresh times (Full: hours to days) can block a validated fix.
- Client UAT availability is outside your control and must be stated as a dependency.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Explain Tech to Non-Tech

*Translate technical issues into business language.*

### 🌱 Simple

*Beginner - plain language*

Clients care about **business impact**, not stack traces. Explain in terms of **what they experience and what it costs/affects**, then the plan — skip jargon. "A governor limit was exceeded" means nothing; "the nightly job stopped because data volume grew" makes sense.

### 📏 Limits

*Governor & platform limits*

- Executive attention is short - aim for three sentences, then offer detail.
- Written summaries get forwarded; assume a wider audience than the recipient.
- Over-simplifying to a technical audience damages credibility.
- Analogies break down if pushed - use them once, then move to specifics.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Manage Scope Creep

*Handle 'can you just add...' without derailing delivery.*

### 🌱 Simple

*Beginner - plain language*

**Scope creep** — endless small additions — silently blows timelines and budgets. Handle it by **acknowledging the request, logging it, and assessing impact** rather than silently absorbing it. "Yes, and here's what it affects" protects both delivery and the relationship.

### 📏 Limits

*Governor & platform limits*

- Fixed-price contracts make creep a direct margin loss; T&M shifts it to the client but still risks the timeline.
- Change control that is slower than the work itself will be bypassed.
- Verbal agreements are unenforceable and are always remembered differently.
- Cumulative small changes are invisible without deliberate tracking.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Status Updates Cadence

*Regular, predictable communication prevents surprises.*

### 🌱 Simple

*Beginner - plain language*

Proactive, **scheduled updates** (daily during issues, weekly on projects) keep clients informed and reduce anxious check-ins. A predictable rhythm signals control and lets small concerns surface before they become escalations.

### 📏 Limits

*Governor & platform limits*

- Contractual comms obligations often specify cadence and channel - check before improvising.
- Written updates are discoverable records; write accordingly.
- Global clients need explicit timezones on every commitment.
- Too-frequent updates on low-severity items train people to ignore you.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Handle Angry Escalation

*Stay calm and professional when a client is furious.*

### 🌱 Simple

*Beginner - plain language*

When a client escalates angrily, **don't match their energy**. Listen fully, acknowledge the impact, stay factual, and move to action. The goal is to **de-escalate** and refocus on solving the problem together.

### 📏 Limits

*Governor & platform limits*

- Escalations often carry contractual triggers - involve your account lead.
- Anything you concede verbally may be treated as a commitment.
- You cannot resolve a commercial dispute in a technical call.
- Repeated hostile escalation is an account-management issue, not a support one.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Under-Promise Over-Deliver

*Beat expectations instead of barely missing them.*

### 🌱 Simple

*Beginner - plain language*

Set expectations you can **comfortably exceed**. Promising "by Friday" and delivering Wednesday delights; promising "tomorrow" and slipping disappoints — even for the same actual work. Manage the **gap between expectation and reality**.

### 📏 Limits

*Governor & platform limits*

- Obvious padding is detected quickly and is treated as dishonesty.
- Fixed-price contracts penalise over-delivery directly.
- Unrequested features still need testing, documentation and support.
- Consistently early delivery resets expectations to the earlier date anyway.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Say No Professionally

*Decline or push back without damaging the relationship.*

### 🌱 Simple

*Beginner - plain language*

Sometimes the right answer is "no" — an unsafe shortcut, an unrealistic date, a bad-practice request. Say it **professionally**: explain the risk, offer an alternative, and let the client decide with full information. "No, because... but here's what we can do" preserves trust.

### 📏 Limits

*Governor & platform limits*

- Some requests are genuinely impossible on the platform - know the limits well enough to prove it.
- Refusing commercial asks without authority creates conflict for your account lead.
- Written refusals become records - be precise and neutral.
- Repeated pushback without alternatives damages the relationship regardless of correctness.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Build Long-Term Trust

*Become the partner clients rely on, not just a coder.*

### 🌱 Simple

*Beginner - plain language*

Long-term trust comes from **consistency, honesty, and proactivity** over time: deliver what you promise, admit mistakes early, flag risks before they bite, and look out for the client's interests. Trust is the compounding asset that survives the inevitable bad day.

### 📏 Limits

*Governor & platform limits*

- Trust takes months to build and one hidden problem to lose.
- Commercial pressure will sometimes conflict with the client's best interest - know your own line.
- Individual trust does not automatically transfer when people change roles.
- Documentation and handover quality outlive you on the account.

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
