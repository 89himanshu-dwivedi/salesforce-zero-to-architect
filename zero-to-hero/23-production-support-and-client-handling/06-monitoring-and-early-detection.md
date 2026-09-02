[Home](../index.md) / [23 · Production Support & Client Handling](index.md) / **Monitoring & Early Detection**

# Monitoring & Early Detection

8 topics · Series 23: Production Support & Client Handling

**Topics on this page**

- [Catch Issues Before Client](#catch-issues-before-client)
- [Apex Exception Email Alerts](#apex-exception-email-alerts)
- [Custom Error Logging Object](#custom-error-logging-object)
- [Scheduled Health Checks](#scheduled-health-checks)
- [Monitor Integration Failures](#monitor-integration-failures)
- [Dashboard for Job Status](#dashboard-for-job-status)
- [Proactive Limit Monitoring](#proactive-limit-monitoring)
- [Event Monitoring & Logs](#event-monitoring-and-logs)

## Catch Issues Before Client

*Find and fix problems before the client notices.*

### 🌱 Simple

*Beginner - plain language*

The best client experience is one where **you report and fix issues before they do**. That requires **monitoring**: error logging, alerts, health checks, and dashboards that surface failures early. Proactive beats reactive every time — it builds enormous trust.

### 📏 Limits

*Governor & platform limits*

- Salesforce has no built-in APM - you build or buy it.
- Event Monitoring requires the add-on and is not real-time.
- Apex exception emails are easily lost and have no routing rules.
- Scheduled checks consume the 100-scheduled-jobs limit.
- Debug logs are unsuitable for monitoring - 7-day retention and 1,000 MB cap.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Apex Exception Email Alerts

*Get emailed the moment unhandled Apex errors occur.*

### 🌱 Simple

*Beginner - plain language*

Salesforce can **email developers when unhandled Apex exceptions occur** (Apex Exception Email setting). It's the cheapest early-warning system — you learn about errors as they happen, with the stack trace, instead of waiting for a complaint.

### 📏 Limits

*Governor & platform limits*

- Only unhandled exceptions trigger the email.
- No rate limiting, deduplication, or severity routing.
- Emails count toward org email limits in some configurations.
- Batch and future failures may report differently or not at all.
- Stack traces can contain sensitive data - consider who receives them.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Custom Error Logging Object

*A queryable log of caught errors with full context.*

### 🌱 Simple

*Beginner - plain language*

Build a **custom `Error_Log__c` object** and write to it whenever you catch an exception — capturing the message, stack trace, user, record, and inputs. Now errors are **queryable, reportable, and dashboardable**, even the handled ones that never email anyone.

### 📏 Limits

*Governor & platform limits*

- Log records consume data storage - budget and purge for it.
- Long text fields are capped at 131,072 characters and are not searchable/filterable.
- DML-written logs are rolled back with the transaction unless published as Platform Events.
- Platform Event publish allocations apply per edition.
- Log objects can accidentally become a PII store - mask and review access.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Scheduled Health Checks

*A scheduled job that verifies the system is healthy.*

### 🌱 Simple

*Beginner - plain language*

A **scheduled health-check job** runs periodically and verifies key things are working: jobs ran, integrations succeeded, data looks sane, limits aren't near. If something's off, it **alerts you** — catching silent failures early.

### 📏 Limits

*Governor & platform limits*

- Max **100** scheduled Apex jobs per org.
- Standard scheduling granularity is hourly; sub-hourly needs stacked jobs.
- Checks run as the scheduling user, with that user's sharing and permissions.
- Health-check queries consume governor limits like any Apex.
- Alerting by email is subject to daily email limits.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Monitor Integration Failures

*Know immediately when a callout or inbound call fails.*

### 🌱 Simple

*Beginner - plain language*

Integrations fail quietly — a 401, a timeout, a down endpoint. **Log every integration call** (status, endpoint, correlation Id) and **alert on failures**, so you catch token expiry or outages before the business is impacted.

### 📏 Limits

*Governor & platform limits*

- Salesforce provides no native integration dashboard - you build it.
- Platform Event retention **72 hours** limits replay-based recovery.
- Daily API limits are org-wide across all integrations.
- Vendor-side metrics may be unavailable to you.
- Storing full request/response bodies for every call consumes storage quickly.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Dashboard for Job Status

*One screen showing the health of jobs and errors.*

### 🌱 Simple

*Beginner - plain language*

A **monitoring dashboard** over your logs gives an at-a-glance view: today's errors by area, batch/scheduled job outcomes, integration failures, and trends. It turns scattered logs into a **single source of truth** you can check each morning.

### 📏 Limits

*Governor & platform limits*

- Dashboards: **20 components** each.
- Dynamic dashboards limited per edition (3 / 5 / 10).
- Reports display **2,000** rows; report types span max **4** objects.
- `AsyncApexJob` rows age out and are not retained indefinitely.
- Reporting Snapshots have row limits per run.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Proactive Limit Monitoring

*Watch limit usage trends before you hit the wall.*

### 🌱 Simple

*Beginner - plain language*

Many outages are **limits creeping up** — API calls, data/file storage, async usage — until one day they're exhausted. **Monitor usage trends** (via the `Limits` class / `/limits` API) and alert at thresholds (e.g. 80%) so you act before hitting 100%.

### 📏 Limits

*Governor & platform limits*

- Daily API limit is edition and licence based, shared org-wide.
- Data storage overage blocks record creation.
- Async Apex daily limit: 250,000 or 200 x licences, whichever is greater.
- Max **100** scheduled jobs and **5** concurrent batch jobs.
- `OrgLimits.getMap()` itself does not consume a governor limit but the job around it does.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Event Monitoring & Logs

*Platform-level visibility into performance and usage.*

### 🌱 Simple

*Beginner - plain language*

**Event Monitoring** (and Shield) exposes detailed **event log files** — page performance (EPT), API usage, logins, report exports, Apex execution. It's how you spot **performance degradation, unusual access, and heavy users** across the whole org.

### 📏 Limits

*Governor & platform limits*

- Requires the Event Monitoring / Shield add-on licence.
- Files are generated daily (hourly with Real-Time Event Monitoring) - not real-time by default.
- Retention is 1 day or 30 days depending on licence.
- Not all event types are available in all editions.
- Files are CSV downloads; meaningful analysis needs an external platform.

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
