[Home](../index.md) / [23 · Production Support & Client Handling](index.md) / **Incident & Hotfix Management**

# Incident & Hotfix Management

8 topics · Series 23: Production Support & Client Handling

**Topics on this page**

- [Severity Triage (Sev1-Sev4)](#severity-triage-sev1-sev4)
- [Hotfix vs Full Fix](#hotfix-vs-full-fix)
- [Emergency Production Fix](#emergency-production-fix)
- [Rollback a Bad Deploy](#rollback-a-bad-deploy)
- [Disable Bad Automation Fast](#disable-bad-automation-fast)
- [Root Cause Analysis (RCA)](#root-cause-analysis-rca)
- [Post-Incident Review](#post-incident-review)
- [Change Freeze & Comms](#change-freeze-and-comms)

## Severity Triage (Sev1-Sev4)

*Classify the issue so the response matches the impact.*

### 🌱 Simple

*Beginner - plain language*

Not every bug is an emergency. **Severity triage** classifies impact so you respond appropriately: a Sev1 (production down, money/data at risk) gets all hands now; a Sev4 (cosmetic) goes in the backlog. Agreeing severity with the client sets expectations.

### 📏 Limits

*Governor & platform limits*

- SLA response times are contractual - agree them before signing, not during an incident.
- Salesforce Support has its own severity scale and response targets by licence tier.
- Sev1 to Salesforce requires phone contact for the fastest response.
- Premier Support is required for guaranteed response times on critical issues.
- Your SLA cannot be faster than the vendor SLAs you depend on.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Hotfix vs Full Fix

*Stop the bleeding now, fix it properly after.*

### 🌱 Simple

*Beginner - plain language*

In an incident you often need **two fixes**: a fast **hotfix/workaround** to stop impact now (deactivate automation, toggle a flag, correct data), then a **full fix** done properly with tests in the normal release. Don't let the quick patch become permanent untested debt.

### 📏 Limits

*Governor & platform limits*

- Production Apex deploys require **75%** coverage even under emergency.
- Validation-only deploy plus Quick Deploy (valid for 10 days) is the fastest safe path.
- Change Sets cannot be partially deployed or rolled back.
- Deploys per 24h rolling window are capped.
- Some config changes (sharing, roles) trigger long recalculations that cannot be cancelled.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Emergency Production Fix

*How to change prod safely under pressure.*

### 🌱 Simple

*Beginner - plain language*

An emergency fix still needs **discipline**: understand the cause, make the smallest safe change, deploy through a proper path (or quick sandbox validation), and communicate. Panicked direct edits to prod cause second incidents.

### 📏 Limits

*Governor & platform limits*

- Apex deploys always require **75%** coverage - there is no emergency bypass.
- Quick Deploy requires a successful validation within the last **10 days**.
- Change Sets have no rollback.
- Large deploys can take 30+ minutes to run tests - factor that into the mitigation plan.
- Salesforce Support cannot roll back your metadata for you.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Rollback a Bad Deploy

*A release broke prod — get back to a known-good state.*

### 🌱 Simple

*Beginner - plain language*

If a deploy caused the problem, the fastest recovery is often to **roll back to the previous working state**. Salesforce has no one-click rollback, so you rely on a **back-out plan**: the prior metadata version, a quick revert deploy, or deactivating the new components.

### 📏 Limits

*Governor & platform limits*

- No native rollback in the Metadata API or Change Sets.
- Deleting a custom field deletes its data (recoverable for 15 days from the field Recycle Bin).
- Destructive changes must be a separate deployment step.
- Some changes (master-detail conversion, OWD) cannot be reversed cleanly.
- Deploy frequency is capped per 24h rolling window.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Disable Bad Automation Fast

*Kill a misbehaving trigger/Flow without a full deploy.*

### 🌱 Simple

*Beginner - plain language*

When a trigger or Flow is actively damaging data, you need to **stop it immediately**. Flows can be **deactivated in clicks**; for Apex, a **kill-switch custom setting/permission** built into your code lets admins disable it without deploying.

### 📏 Limits

*Governor & platform limits*

- Triggers cannot be deactivated from the production UI - deploy only.
- Aborting a batch stops future chunks; the current chunk completes.
- Flow deactivation is immediate but in-flight interviews continue.
- Hierarchy Custom Settings allow per-user/profile bypass without a deploy.
- Custom Metadata edits are immediate but are metadata changes and are audited.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Root Cause Analysis (RCA)

*Find the true cause, not just the symptom.*

### 🌱 Simple

*Beginner - plain language*

After containment, **RCA** answers *why* it happened — not just what broke. The classic technique is **"5 Whys"**: keep asking why until you reach the systemic cause (e.g. "no test for bulk" → "no bulk-test standard").

### 📏 Limits

*Governor & platform limits*

- Debug logs are gone after **7 days** - collect evidence during the incident.
- Setup Audit Trail covers **6 months**.
- Event Monitoring data arrives with delay and requires the add-on.
- Vendor-side evidence depends on their retention and cooperation.
- Contractual RCA deadlines are common in enterprise SLAs.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Post-Incident Review

*A blameless retro that prevents the next incident.*

### 🌱 Simple

*Beginner - plain language*

A **post-incident review (postmortem)** documents what happened, the timeline, impact, root cause, and **action items** — blamelessly. The goal is **learning and prevention**, not punishment, so people share honestly.

### 📏 Limits

*Governor & platform limits*

- Evidence windows are short - logs 7 days, Platform Events 72 hours.
- Contractual review deadlines are common in enterprise agreements.
- Action tracking needs a system; meeting notes are not a tracker.
- Client attendance may require account-team approval.
- Metrics are only meaningful if incidents are consistently logged.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Change Freeze & Comms

*Control changes and communicate during/after incidents.*

### 🌱 Simple

*Beginner - plain language*

During a major incident (or critical business periods like month-end), a **change freeze** stops new deploys that could add risk. Clear **communication** — status, ETA, resolution — keeps stakeholders calm and aligned.

### 📏 Limits

*Governor & platform limits*

- Salesforce seasonal releases cannot be frozen - only the sandbox preview window gives you lead time.
- Some changes (sharing recalculation, roles) run long and should never start near a freeze boundary.
- Deploy limits per 24h can bottleneck the post-freeze catch-up.
- Production change lockdown is not available in all editions.
- Managed package upgrades from vendors may not respect your freeze.

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
