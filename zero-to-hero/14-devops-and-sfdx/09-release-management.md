[Home](../index.md) / [14 · DevOps & SFDX](index.md) / **Release Management**

# Release Management

4 topics · Series 14: DevOps & SFDX

**Topics on this page**

- [Release Planning](#release-planning)
- [Change Management](#change-management)
- [Hotfix Process](#hotfix-process)
- [Production Support](#production-support)

## Release Planning

*Coordinating scope, schedule, and dependencies of a release.*

### 🌱 Simple

*Beginner - plain language*

**Release planning** coordinates **what ships when** — defining scope, schedule, dependencies, environments, and readiness criteria so a release reaches production smoothly.

### 📏 Limits

*Governor & platform limits*

- Salesforce seasonal releases are non-negotiable and must be planned around.
- Deploy frequency limits per 24 hours constrain hotfix cadence.
- Sandbox refresh windows (Full: 29 days) constrain UAT scheduling.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Change Management

*Governance process for approving production changes.*

### 🌱 Simple

*Beginner - plain language*

**Change management** is the governance process for **reviewing, approving, and tracking** production changes — ensuring every change is assessed for risk, authorized, and recorded.

### 📏 Limits

*Governor & platform limits*

- Setup Audit Trail retains only 6 months in the UI - export for longer.
- Production change lockdown is not available in all editions.
- Managed package upgrades from vendors may not respect your change freeze.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Hotfix Process

*Fast, controlled path for urgent production fixes.*

### 🌱 Simple

*Beginner - plain language*

A **hotfix process** is the expedited, controlled path to fix a **critical production issue** fast — branching from production's state, fixing, testing, deploying, and merging the fix back.

### 📏 Limits

*Governor & platform limits*

- Production Apex deploys always require 75% coverage - no emergency bypass.
- Quick Deploy needs a validation within the last 10 days.
- Hotfixes must be forward-ported the same day or the bug returns.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Production Support

*Operating, monitoring, and supporting the live org.*

### 🌱 Simple

*Beginner - plain language*

**Production support** is the ongoing operation of the live org — monitoring health, responding to incidents, supporting users, and maintaining stability after release.

### 📏 Limits

*Governor & platform limits*

- Debug logs retained 7 days, 1,000 MB org allocation, 20 MB per log.
- Event Monitoring needs an add-on and is not real-time.
- Setup Audit Trail covers 6 months.
- Salesforce Support cannot roll back your metadata.

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
