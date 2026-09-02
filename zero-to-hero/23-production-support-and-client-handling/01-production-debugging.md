[Home](../index.md) / [23 · Production Support & Client Handling](index.md) / **Production Debugging**

# Production Debugging

10 topics · Series 23: Production Support & Client Handling

**Topics on this page**

- [Read a Production Debug Log](#read-a-production-debug-log)
- [Works in Sandbox Not Prod](#works-in-sandbox-not-prod)
- [Reproduce a Prod-Only Bug](#reproduce-a-prod-only-bug)
- [Trace a Specific User's Error](#trace-a-specific-user-s-error)
- [Debug Without Breaking Prod](#debug-without-breaking-prod)
- [Find the Root Cause Fast](#find-the-root-cause-fast)
- [Intermittent Bug Hunting](#intermittent-bug-hunting)
- [Debug a Failed Batch Job](#debug-a-failed-batch-job)
- [Debug a Failed Integration](#debug-a-failed-integration)
- [Debug a Slow Transaction](#debug-a-slow-transaction)

## Read a Production Debug Log

*Capture and read a live debug log without guessing.*

### 🌱 Simple

*Beginner - plain language*

When something breaks in production, the **debug log** is your eyes. You can't change code to add `System.debug` freely in prod, so you set a **Debug Log trace on the affected user**, reproduce the action, and read the timeline + exception. This turns "it's broken" into "line 42 threw a null pointer."

### 📏 Limits

*Governor & platform limits*

- Single debug log max **20 MB** - larger transactions are truncated.
- Org keeps up to **1,000 MB** of logs; oldest deleted first.
- User trace flags expire after **24 hours**.
- Logs retained **7 days**.
- Logging itself consumes CPU - FINEST can push a borderline transaction over the limit.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Works in Sandbox Not Prod

*The classic: passes in sandbox, fails in production.*

### 🌱 Simple

*Beginner - plain language*

"It worked in sandbox!" almost always means the **environments differ** — data volume, config, permissions, or missing metadata. Prod has real data, real users, more automation, and stricter sharing. The fix is to **compare the two environments systematically**, not re-test the same way.

### 📏 Limits

*Governor & platform limits*

- Refresh intervals: Full **29 days**, Partial Copy **5 days**, Developer **1 day**.
- Partial Copy: **5 GB** and 10,000 records per object per template.
- Developer sandbox data storage **200 MB**.
- Query selectivity enforcement only engages above **200,000** rows.
- Sandbox refresh regenerates Ids, breaking hardcoded references.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Reproduce a Prod-Only Bug

*You can't fix what you can't reproduce — make it happen safely.*

### 🌱 Simple

*Beginner - plain language*

If a bug only appears in prod, your first job is to **reproduce it** — ideally in a sandbox so you can fix safely. That means copying the **exact conditions**: same data shape, same user/profile, same volume, same sequence of actions.

### 📏 Limits

*Governor & platform limits*

- Login As must be enabled, is blocked for some profiles, and is fully audited.
- Trace flags only capture from creation onward.
- Event Monitoring requires the add-on and files arrive hours later.
- The Apex Interactive Debugger does not attach to production.
- Only Full sandboxes contain production data.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Trace a Specific User's Error

*One user hits it, others don't — isolate their context.*

### 🌱 Simple

*Beginner - plain language*

When **only one user** sees an error, the cause is usually their **context**: their profile/permissions, their role/sharing, their data, or their device/browser. Trace *that user* specifically rather than testing as yourself.

### 📏 Limits

*Governor & platform limits*

- User trace flags expire after **24 hours**.
- Org log storage **1,000 MB**, retained **7 days**.
- Max **20 MB** per log.
- Event Monitoring needs the add-on; data is not real-time.
- No logs are generated for transactions that started before the flag existed.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Debug Without Breaking Prod

*Investigate live issues without making things worse.*

### 🌱 Simple

*Beginner - plain language*

Debugging in production is risky — you must **observe without changing behavior**. Use **debug logs, Developer Console, read-only queries, and Login As**. Never push experimental code, never edit data blindly, and never disable controls without a plan to restore them.

### 📏 Limits

*Governor & platform limits*

- Anonymous Apex commits - there is no automatic rollback.
- `Savepoint`: max **5** per transaction.
- Debug logging adds CPU overhead and can itself cause limit failures.
- The Apex Interactive Debugger requires a licence and does not attach to production.
- Setup Audit Trail keeps **6 months** - assume everything you do is visible.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Find the Root Cause Fast

*A repeatable method to get from symptom to cause quickly.*

### 🌱 Simple

*Beginner - plain language*

Fast debugging isn't luck — it's a **method**: reproduce, read the actual error, isolate the failing component, form one hypothesis, test it, repeat. Don't change five things at once; change one, measure, learn.

### 📏 Limits

*Governor & platform limits*

- Setup Audit Trail retains **6 months** in the UI.
- Debug logs retained **7 days** - evidence disappears fast.
- Field History Tracking: **20 fields** per object.
- Event Monitoring files arrive with hours of delay.
- You cannot see vendor-side logs - integration RCA needs their timeline.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Intermittent Bug Hunting

*"Sometimes it fails" — find the hidden variable.*

### 🌱 Simple

*Beginner - plain language*

Intermittent bugs feel random but aren't — there's a **hidden variable**: concurrency, data state, timing, caching, or a specific record/user. The trick is to **capture enough context when it does happen** to spot the pattern.

### 📏 Limits

*Governor & platform limits*

- Row lock timeout is about **10 seconds** before `UNABLE_TO_LOCK_ROW`.
- Debug logs are useless for rare events - 7-day retention, no retroactive capture.
- Field History and audit data are limited, not a full transaction log.
- Platform Event retention **72 hours** limits replay.
- Some known issues have no fix - plan mitigations, not resolutions.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Debug a Failed Batch Job

*A nightly batch failed — find which records and why.*

### 🌱 Simple

*Beginner - plain language*

When a Batch Apex job fails, check **Setup → Apex Jobs** for its status, and use the job's **error info + debug logs**. Common causes: a limit hit in `execute`, a bad record, or an uncaught exception. Good batches log per-scope failures instead of dying silently.

### 📏 Limits

*Governor & platform limits*

- **5 concurrent** batch jobs; Flex Queue holds **100**.
- Scope size max **2,000**, default 200.
- `start()` QueryLocator up to **50 million** rows.
- Async daily limit: 250,000 or 200 x licences, whichever is greater.
- `AsyncApexJob` rows age out - export before they do.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Debug a Failed Integration

*An inbound/outbound integration stopped working — isolate the side.*

### 🌱 Simple

*Beginner - plain language*

Integration failures have two sides — **Salesforce** and the **external system** — plus the network between. Debugging is about **isolating which side failed**: check the request/response, status codes, auth, and logs on both ends.

### 📏 Limits

*Governor & platform limits*

- Callout timeout max **120s**; total callout time **120s** per transaction.
- **100 callouts** per transaction; body max **6 MB** sync / 12 MB async.
- Daily API request limit is org-wide across all integrations.
- Platform Event retention **72 hours** caps replay.
- You cannot see vendor logs - RCA depends on their cooperation.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Debug a Slow Transaction

*A save/page is slow in prod — find the time sink.*

### 🌱 Simple

*Beginner - plain language*

For a slow transaction, **measure where the time goes** with a debug log timeline: which trigger/Flow/query/callout took longest. Slowness is usually **stacked automation, non-selective queries, or a slow callout** — fix the biggest segment first.

### 📏 Limits

*Governor & platform limits*

- CPU **10s** sync / **60s** async, excluding DB and callout wait.
- Concurrent long-running (>5s) requests: **10** per org.
- SOQL query timeout about 120s.
- Debug log 20 MB cap - FINEST on a slow transaction often truncates.
- Logging overhead itself adds measurable time.

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
