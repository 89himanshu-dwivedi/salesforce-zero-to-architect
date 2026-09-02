[Home](../index.md) / [04 · Automation](index.md) / **Workflow (Legacy)**

# Workflow (Legacy)

3 topics · Series 4: Automation

**Topics on this page**

- [Email Alert](#email-alert)
- [Field Update](#field-update)
- [Outbound Message](#outbound-message)

## Email Alert

*Legacy workflow action that sends a templated email to defined recipients on a rule.*

### 🌱 Simple

*Beginner - plain language*

An **Email Alert** is a workflow action that automatically **sends an email** (using a template) to chosen recipients when a rule's conditions are met — e.g., notify a manager when a big deal is created.

### 📏 Limits

*Governor & platform limits*

- Workflow is legacy/retired for new builds; daily email limits; deliverability settings; reuse alerts from Flow.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Field Update

*Legacy workflow action that automatically sets a field's value when a rule fires.*

### 🌱 Simple

*Beginner - plain language*

A **Field Update** is a workflow action that **changes a field's value** automatically — e.g., set Status to "Escalated" or stamp an approval date — when conditions are met.

### 📏 Limits

*Governor & platform limits*

- Legacy; can re-trigger rules; before-save Flow is the efficient successor; still used in approval processes.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Outbound Message

*Legacy workflow action that sends a SOAP message to an external endpoint on a rule.*

### 🌱 Simple

*Beginner - plain language*

An **Outbound Message** is a workflow action that sends record data as a **SOAP message** to an external system's URL when a rule fires — a no-code way to push data out.

### 📏 Limits

*Governor & platform limits*

- Retries with backoff for 24 hours, then discards the message silently.
- Pending message queue has a per-org cap.
- SOAP only, and it sends a session Id - usually rejected in security review.
- No response handling beyond an acknowledgement.

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
