[Home](../index.md) / [04 · Automation](index.md) / **Approval Process**

# Approval Process

4 topics · Series 4: Automation

**Topics on this page**

- [Submit](#submit)
- [Approve](#approve)
- [Reject](#reject)
- [Recall](#recall)

## Submit

*Sending a record into an approval process — locks it and routes to the first approver.*

### 🌱 Simple

*Beginner - plain language*

**Submit** starts an approval: a user sends a record (e.g., a big discount) **for approval**. The record is usually **locked** and routed to the first **approver** defined in the process.

### 📏 Limits

*Governor & platform limits*

- Entry criteria gate submission; locking configurable; dynamic approver via manager/related field; history tracked.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Approve

*An approver accepts the record — runs approval actions and advances or completes the process.*

### 🌱 Simple

*Beginner - plain language*

**Approve** is when an approver says "yes". The approval process then runs its **approval actions** (e.g., set status, send confirmation) and either moves to the **next step** or finishes if it was the last.

### 📏 Limits

*Governor & platform limits*

- Step + final actions; multi-step/parallel (unanimous/first-response); history tracked; delegated approvers; order-of-execution interplay.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Reject

*An approver declines — runs rejection actions and sends the record back or ends the process.*

### 🌱 Simple

*Beginner - plain language*

**Reject** is when an approver says "no". The process runs its **rejection actions** (e.g., set status to Rejected, notify the submitter, unlock) and either ends or sends the record **back a step**, depending on configuration.

### 📏 Limits

*Governor & platform limits*

- Final vs back-one-step rejection; rejection actions/unlock; reason via comments; history tracked; recursion on field updates.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Recall

*The submitter withdraws a pending approval before it's decided — recall actions run.*

### 🌱 Simple

*Beginner - plain language*

**Recall** lets the submitter **withdraw** a record from approval while it's still pending — maybe they spotted a mistake. **Recall actions** run (e.g., unlock, reset status) and the approval stops.

### 📏 Limits

*Governor & platform limits*

- Only the submitter or an admin can recall, and only while the request is pending.
- Recall actions are configured per approval step and are not automatic.
- Recalled requests must be resubmitted from the start - there is no resume.

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
