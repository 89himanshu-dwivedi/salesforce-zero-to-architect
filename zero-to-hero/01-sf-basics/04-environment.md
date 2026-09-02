[Home](../index.md) / [01 · SF Basics](index.md) / **Environment**

# Environment

7 topics · Series 1: SF Basics

**Topics on this page**

- [Production Org](#production-org)
- [Sandbox](#sandbox)
- [Developer Sandbox](#developer-sandbox)
- [Partial Sandbox](#partial-sandbox)
- [Full Sandbox](#full-sandbox)
- [Scratch Org](#scratch-org)
- [Trailhead Playground](#trailhead-playground)

## Production Org

*The live org where real users and real data operate — protected by change control.*

### 🌱 Simple

*Beginner - plain language*

The **production org** is your real, live Salesforce — where actual employees work and real customer data lives. Everything you build is eventually deployed here. Mistakes here affect real business, so changes are tested elsewhere first.

### 📏 Limits

*Governor & platform limits*

- Data and file storage are allocated by edition plus licences; hitting 100% blocks record creation.
- Deploys per 24-hour rolling window are capped.
- Triggers cannot be deactivated from the UI - only by deploy.
- API request limits are org-wide and shared by every integration.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Sandbox

*A copy of production for development/testing — types differ by data and refresh interval.*

### 🌱 Simple

*Beginner - plain language*

A **sandbox** is a separate copy of your production org used to build and test safely, without touching real data/users. You develop here, test, then deploy to production.

### 📏 Limits

*Governor & platform limits*

- Refresh intervals: Full 29 days, Partial Copy 5 days, Developer and Developer Pro 1 day.
- Email deliverability defaults to "System email only".
- Refresh regenerates record Ids, breaking any hardcoded reference.
- Sandbox counts are allocated by edition and are not unlimited.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Developer Sandbox

*Lightweight metadata-only sandbox, refreshable daily — for individual development.*

### 🌱 Simple

*Beginner - plain language*

A **Developer sandbox** copies your org's configuration (metadata) but *no* real data, and can be refreshed every day. It's the everyday workspace for a developer to build and unit-test.

### 📏 Limits

*Governor & platform limits*

- 200 MB data storage and 200 MB file storage.
- Metadata only - no production data is copied.
- Refresh once per day.
- Far too small to reveal volume-related bugs.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Partial Sandbox

*Partial Copy — metadata + a sample of real data; ideal for QA/UAT. ~5-day refresh.*

### 🌱 Simple

*Beginner - plain language*

A **Partial Copy sandbox** includes your configuration plus a *sample* of real production data (defined by a sandbox template). It's great for testing with realistic-but-limited data, refreshable about every 5 days.

### 📏 Limits

*Governor & platform limits*

- 5 GB data storage; 10,000 records per object per sandbox template.
- Refresh every 5 days.
- Sampled data means edge-case records are frequently missing.
- Templates must be defined before refresh, not after.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Full Sandbox

*Exact production replica (all data) — for staging, performance, training. ~29-day refresh.*

### 🌱 Simple

*Beginner - plain language*

A **Full sandbox** is a complete copy of production — all data and configuration. It's used for final staging, performance testing, and training, but refreshes only about every 29 days and uses the most storage.

### 📏 Limits

*Governor & platform limits*

- Matches production data storage; refresh only every 29 days.
- Refresh can take hours to days for large orgs.
- Contains real production data - masking and access control are mandatory.
- Usually only one is licensed, so it becomes a scheduling bottleneck.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Scratch Org

*Disposable, source-defined org for package development & CI — the SFDX way.*

### 🌱 Simple

*Beginner - plain language*

A **scratch org** is a temporary, throwaway Salesforce org you spin up from a configuration file, use for a few days, then delete. It's defined by code, making it perfect for automated development and testing.

### 📏 Limits

*Governor & platform limits*

- Maximum lifetime 30 days, default 7.
- Active and daily counts are capped by Dev Hub edition.
- No production data; features must be declared in the definition file.
- Not every edition feature can be enabled via a scratch definition.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Trailhead Playground

*Free personal practice org tied to Trailhead — for learning, not production.*

### 🌱 Simple

*Beginner - plain language*

A **Trailhead Playground** is a free Salesforce org Salesforce gives you to practice and complete Trailhead challenges. It's for learning and experimentation — never for real business data.

### 📏 Limits

*Governor & platform limits*

- Very limited storage and licences; not suitable for realistic testing.
- Expires and can be reset without warning.
- Not connected to your production org and cannot be used for real deployments.

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
