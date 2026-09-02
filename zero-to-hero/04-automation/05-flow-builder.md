[Home](../index.md) / [04 · Automation](index.md) / **Flow Builder**

# Flow Builder

18 topics · Series 4: Automation

**Topics on this page**

- [Screen Flow](#screen-flow)
- [Record Triggered Flow - Before Save](#record-triggered-flow-before-save)
- [Record Triggered Flow - After Save](#record-triggered-flow-after-save)
- [Scheduled Flow](#scheduled-flow)
- [Auto Launched Flow](#auto-launched-flow)
- [Platform Event Flow](#platform-event-flow)
- [Assignment Element](#assignment-element)
- [Decision Element](#decision-element)
- [Loop Element](#loop-element)
- [Get Records](#get-records)
- [Create Records](#create-records)
- [Update Records](#update-records)
- [Delete Records](#delete-records)
- [Action Element](#action-element)
- [Subflow](#subflow)
- [Collection Processing](#collection-processing)
- [Bulkification](#bulkification)
- [Fault Handling](#fault-handling)

## Screen Flow

*Interactive, multi-screen guided UI users step through — wizards, intake forms, guided actions.*

### 🌱 Simple

*Beginner - plain language*

A **Screen Flow** shows users **screens** with inputs (text, picklists, lookups) they fill in step-by-step — like a wizard. Great for guided processes: case intake, order entry, guided selling.

### 📏 Limits

*Governor & platform limits*

- Synchronous/user transaction; respects FLS/sharing (configurable); per-transaction governor limits; embed LWC for custom UI.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Record Triggered Flow - Before Save

*Runs before the record is saved — set same-record fields fast, no extra DML.*

### 🌱 Simple

*Beginner - plain language*

A **before-save** record-triggered flow runs *just before* a record is saved and can **change fields on that same record** — without an extra save. It's the fastest way to default or transform values.

### 📏 Limits

*Governor & platform limits*

- Same-record fields only; no DML/callout/email/actions; runs before validation; one-per-object recommended.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Record Triggered Flow - After Save

*Runs after save — update related records, send emails, call actions, do async work.*

### 🌱 Simple

*Beginner - plain language*

An **after-save** record-triggered flow runs *after* the record is committed and can do more: **update related records**, send emails, call actions, and kick off asynchronous work. Use it when before-save isn't enough.

### 📏 Limits

*Governor & platform limits*

- Extra transaction vs before-save; governor limits; order-of-execution interplay; use async path for callouts.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Scheduled Flow

*Runs on a schedule over a set of records — nightly batch-style declarative automation.*

### 🌱 Simple

*Beginner - plain language*

A **Scheduled Flow** runs automatically at a set **time/frequency** (e.g., every night) and can process a batch of records — like flagging overdue invoices or sending daily reminders.

### 📏 Limits

*Governor & platform limits*

- Records-per-run + daily async caps; runs once per matching record; off-peak recommended; Batch Apex for big volumes.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Auto Launched Flow

*A no-UI flow invoked by other automation/Apex/REST — reusable backend logic.*

### 🌱 Simple

*Beginner - plain language*

An **Auto-launched Flow** has **no screens** — it runs in the background when called by another flow, Apex, a process, a button, or the API. It's reusable backend logic you trigger on demand.

### 📏 Limits

*Governor & platform limits*

- No UI; runs in caller's transaction (governor limits); user vs system mode; async for callouts; invocable from Apex/REST.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Platform Event Flow

*A flow triggered by a Platform Event message — event-driven, decoupled automation.*

### 🌱 Simple

*Beginner - plain language*

A **Platform Event-triggered Flow** runs when a **Platform Event** message is published. It lets Salesforce react to events (internal or from external systems) in a **decoupled**, real-time way.

### 📏 Limits

*Governor & platform limits*

- Runs async as Automated Process user; at-least-once delivery/replay; event publishing/delivery limits; decoupled error handling.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Assignment Element

*Set or change variable/collection values in a flow — the workhorse data element.*

### 🌱 Simple

*Beginner - plain language*

The **Assignment** element sets values into **variables** — store a result, build text, increment a counter, or add an item to a collection. It's how a flow holds and changes data as it runs.

### 📏 Limits

*Governor & platform limits*

- Assignments are in-memory only until a Create or Update element runs.
- Collection assignments in a loop are a common cause of CPU limit failures.
- Flow shares the transaction's governor limits with Apex.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Decision Element

*Branch the flow down different paths based on conditions — the if/else of Flow.*

### 🌱 Simple

*Beginner - plain language*

The **Decision** element branches a flow: it checks conditions and sends the flow down the matching **outcome** path — like if/else-if/else. E.g., route high-value deals one way, others another.

### 📏 Limits

*Governor & platform limits*

- Every outcome is evaluated in order until one matches - no short-circuit optimisation.
- Complex conditions consume CPU from the shared 10s budget.
- A missing default outcome silently ends that path.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Loop Element

*Iterate over a collection of records/items one at a time — process lists in Flow.*

### 🌱 Simple

*Beginner - plain language*

The **Loop** element goes through a **collection** (list) of records one at a time, letting you examine or prepare each item. It's how a flow handles many records together.

### 📏 Limits

*Governor & platform limits*

- Iteration/CPU limits; SOQL(100)/DML(150) per transaction; avoid in-loop queries/DML; Batch Apex for huge volumes.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Get Records

*Query records into the flow — the SOQL of Flow, done once and bulk-safe.*

### 🌱 Simple

*Beginner - plain language*

**Get Records** retrieves records from the database into the flow based on conditions — like SOQL without code. You then use those records in decisions, loops, or updates.

### 📏 Limits

*Governor & platform limits*

- Each Get Records is a SOQL query against the 100-query transaction limit.
- Returns a maximum of 50,000 rows and counts toward the transaction row limit.
- Get Records inside a loop is the leading cause of Flow governor failures.
- Non-selective filters abort on large objects just as in Apex.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Create Records

*Insert new records from the flow — single or, ideally, a whole collection at once.*

### 🌱 Simple

*Beginner - plain language*

**Create Records** inserts new records into Salesforce from a flow — one record, or a **collection** of many at once. E.g., create follow-up tasks or child records.

### 📏 Limits

*Governor & platform limits*

- DML 150/transaction; 10k rows; triggers downstream automation; run-mode affects FLS/required; add fault path.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Update Records

*Modify existing records from the flow — by Id collection or by filter criteria.*

### 🌱 Simple

*Beginner - plain language*

**Update Records** changes existing records from a flow. You can update a **collection** you've prepared, or tell Flow to find-and-update records matching **criteria** directly.

### 📏 Limits

*Governor & platform limits*

- DML 150/rows 10k; before-save avoids DML for same record; recursion risk; respects validation; run-mode FLS.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Delete Records

*Remove records from the flow — by Id collection or criteria; powerful, handle with care.*

### 🌱 Simple

*Beginner - plain language*

**Delete Records** removes records from a flow — a prepared **collection** or records matching **criteria**. Useful for cleanup, but irreversible-ish (records go to the Recycle Bin), so use carefully.

### 📏 Limits

*Governor & platform limits*

- DML 150/rows 10k; Recycle Bin retention; cascade + delete triggers; restricted deletes; Batch Apex for volume.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Action Element

*Call platform/Apex/external actions from a flow — email, Apex, callouts, more.*

### 🌱 Simple

*Beginner - plain language*

The **Action** element lets a flow **call something** beyond basic record CRUD — send an email, post to Chatter, call **Apex**, submit for approval, or invoke an **external service**.

### 📏 Limits

*Governor & platform limits*

- Callout limit 100/transaction; async needed for callouts; invocable Apex should take/return lists; Named Credentials for auth.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Subflow

*Call one flow from another — reusable, modular declarative logic.*

### 🌱 Simple

*Beginner - plain language*

A **Subflow** element calls **another flow** from inside your flow, passing inputs and getting outputs back. It lets you reuse logic instead of rebuilding it everywhere.

### 📏 Limits

*Governor & platform limits*

- Shares parent transaction/governor limits; versioned (active runs); explicit input/output contracts; affects all callers.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Collection Processing

*Filter, sort, and transform record collections in-memory — often without loops.*

### 🌱 Simple

*Beginner - plain language*

**Collection processing** means working with a **list of records** in a flow — filtering it, sorting it, counting it, or transforming it. Newer Flow features let you do much of this **without writing a loop**.

### 📏 Limits

*Governor & platform limits*

- In-memory (CPU/heap bound); filter/sort elements reduce loops; correlate via pre-Get + maps; size for count checks.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Bulkification

*Designing flows/triggers to handle many records in one transaction within governor limits.*

### 🌱 Simple

*Beginner - plain language*

**Bulkification** means building automation that works correctly whether it processes **one record or thousands** in a single transaction — without breaking Salesforce's limits. It's the #1 skill for reliable automation.

### 📏 Limits

*Governor & platform limits*

- 100 SOQL / 150 DML / 50k rows / 10s CPU / 6MB heap per transaction; 200-record batches; Batch Apex for huge volumes.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Fault Handling

*Catch and handle errors in flows gracefully — fault paths, messaging, recovery.*

### 🌱 Simple

*Beginner - plain language*

**Fault handling** means planning for things going wrong in a flow — a DML failing, a callout timing out — and handling it **gracefully** instead of showing a cryptic error. You add **fault paths** to risky elements.

### 📏 Limits

*Governor & platform limits*

- Unhandled fault rolls back transaction; $Flow.FaultMessage holds error; background errors need logging/notify; retry for callouts.

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
