[Home](../index.md) / [15 · Performance Optimization](index.md) / **Monitoring**

# Monitoring

5 topics · Series 15: Performance Optimization

**Topics on this page**

- [Debug Logs](#debug-logs)
- [Event Monitoring](#event-monitoring)
- [Transaction Monitoring](#transaction-monitoring)
- [Performance Metrics](#performance-metrics)
- [Observability](#observability)

## Debug Logs

*Per-transaction execution logs for diagnosing issues.*

### 🌱 Simple

*Beginner - plain language*

**Debug logs** capture what happens during a transaction — SOQL/DML counts, CPU time, limits, and your `System.debug` output — for a specific user, letting you diagnose errors and performance.

### 📏 Limits

*Governor & platform limits*

- Maximum 20 MB per log; 1,000 MB org allocation; 7-day retention.
- User trace flags expire after 24 hours.
- Logging itself consumes CPU and can push a transaction over the limit.
- No retroactive capture - flags only apply going forward.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Event Monitoring

*Systematic org event data for performance and security.*

### 🌱 Simple

*Beginner - plain language*

**Event Monitoring** provides downloadable **event log files** (and real-time events) covering logins, API calls, Apex executions, page loads, report exports, and more — for systematic performance, adoption, and security analysis.

### 📏 Limits

*Governor & platform limits*

- Requires the Shield or Event Monitoring add-on.
- Files generated daily (hourly with Real-Time); retention 1 or 30 days.
- Meaningful analysis requires export to an external platform.
- Not all event types are available in all editions.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Transaction Monitoring

*Watching transaction-level health, limits, and failures.*

### 🌱 Simple

*Beginner - plain language*

**Transaction monitoring** tracks the health of individual transactions — limit consumption, errors, long-running operations, and failures — so you catch problems before they impact users at scale.

### 📏 Limits

*Governor & platform limits*

- Real-Time Event Monitoring and Transaction Security require additional licensing.
- Policies can block actions but add evaluation latency.
- Not all events are available in real time.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Performance Metrics

*KPIs that quantify org and code performance.*

### 🌱 Simple

*Beginner - plain language*

**Performance metrics** are the measurable KPIs you track to judge org health — page load time (EPT), Apex CPU time, query response time, API response time, error rates, and limit utilization.

### 📏 Limits

*Governor & platform limits*

- Lightning Usage App data is aggregated daily, not real-time.
- EPT is a client-side measure and varies with the user's network and device.
- No native APM - correlation across systems needs an external platform.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Observability

*End-to-end visibility into system behavior and health.*

### 🌱 Simple

*Beginner - plain language*

**Observability** is the ability to understand **what's happening inside** your Salesforce system from its outputs — combining **logs, metrics, and traces** so you can answer "why is this slow/failing?" without guessing.

### 📏 Limits

*Governor & platform limits*

- Salesforce provides no native distributed tracing.
- Log objects consume data storage and need retention policies.
- Event Monitoring export is a build-it-yourself pipeline.
- Vendor-side telemetry is outside your control.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

---

## Connect

These pages carry the **definitions and limits** only. The advanced depth, real-world
scenarios, error playbooks, best-option reasoning and interview questions are kept aside.

If you would like them, or you want to talk about the topics on this page, connect with me
on **LinkedIn**, **X (Twitter)** or **GitHub** - all links are on the
[home page](../index.md).

*- Himanshu Kumar*
