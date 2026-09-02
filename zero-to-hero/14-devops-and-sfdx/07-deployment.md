[Home](../index.md) / [14 · DevOps & SFDX](index.md) / **Deployment**

# Deployment

5 topics · Series 14: DevOps & SFDX

**Topics on this page**

- [Validation Deployment](#validation-deployment)
- [Quick Deploy](#quick-deploy)
- [Rollback Strategy](#rollback-strategy)
- [Backout Plan](#backout-plan)
- [Deployment Windows](#deployment-windows)

## Validation Deployment

*A check-only deploy that validates without committing.*

### 🌱 Simple

*Beginner - plain language*

A **validation (check-only) deployment** verifies that metadata *would* deploy successfully — running tests and checks — **without actually committing** the changes to the org.

### 📏 Limits

*Governor & platform limits*

- Runs tests without committing changes; result is valid for Quick Deploy for 10 days.
- Still consumes a deployment slot in the 24-hour window.
- Large validations can take 30+ minutes on test execution alone.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Quick Deploy

*Committing a recently validated deployment fast.*

### 🌱 Simple

*Beginner - plain language*

**Quick Deploy** commits a deployment that was already **validated** recently — without re-running the Apex tests — so the actual production deploy is near-instant.

### 📏 Limits

*Governor & platform limits*

- Requires a successful validation within the last 10 days.
- The org must not have changed in ways that invalidate the validation.
- Not available for all deployment paths, including some Change Sets.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Rollback Strategy

*Planned path to revert a bad deployment.*

### 🌱 Simple

*Beginner - plain language*

A **rollback strategy** is the planned way to **revert** a deployment that caused problems — restoring the org to its prior working state quickly and safely.

### 📏 Limits

*Governor & platform limits*

- There is no native rollback - you deploy the previous version.
- Deleted fields lose their data (recoverable for 15 days from the field Recycle Bin).
- Data changes cannot be rolled back by a metadata deploy.
- Destructive changes must be a separate deployment step.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Backout Plan

*Documented step-by-step recovery procedure.*

### 🌱 Simple

*Beginner - plain language*

A **backout plan** is the documented, step-by-step procedure to **undo a release** and recover service if the deployment fails or causes critical issues — the operational runbook for rollback.

### 📏 Limits

*Governor & platform limits*

- Must be written before the deploy, not after.
- Some changes (master-detail conversion, OWD, picklist value removal) are effectively irreversible.
- An untested backout plan is a wish - rehearse it in a sandbox.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Deployment Windows

*Scheduled time slots for production changes.*

### 🌱 Simple

*Beginner - plain language*

**Deployment windows** are scheduled, approved time slots (often low-traffic, like weekends/nights) during which production deployments are allowed — minimizing user and business impact.

### 📏 Limits

*Governor & platform limits*

- Deploys per 24-hour rolling window are capped.
- Salesforce seasonal releases cannot be deferred, only previewed in sandboxes.
- Long-running sharing recalculations should never start near a window boundary.

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
