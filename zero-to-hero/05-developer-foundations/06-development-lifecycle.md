[Home](../index.md) / [05 · Developer Foundations](index.md) / **Development Lifecycle**

# Development Lifecycle

4 topics · Series 5: Developer Foundations

**Topics on this page**

- [Development](#development)
- [Testing](#testing)
- [UAT](#uat)
- [Production](#production)

## Development

*The build stage — coding and configuring features in isolated dev environments.*

### 🌱 Simple

*Beginner - plain language*

**Development** is where features are built — admins and developers create flows, fields, Apex, and LWC in their own **dev sandboxes or scratch orgs**, committing work to version control.

### 📏 Limits

*Governor & platform limits*

- Sandbox/scratch org allocations cap parallel environments.
- Declarative changes must be captured to source.
- Refresh/setup time per environment.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Testing

*The stage where changes are validated functionally and technically before release.*

### 🌱 Simple

*Beginner - plain language*

**Testing** is verifying that features work as intended — running **Apex unit tests**, QA functional testing, and integration checks in a dedicated **test/SIT environment** before moving toward production.

### 📏 Limits

*Governor & platform limits*

- Prod requires passing tests + 75% coverage.
- SIT needs integration-capable environment.
- Test runs consume time/limits.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## UAT

*User Acceptance Testing — business users confirm the solution meets requirements.*

### 🌱 Simple

*Beginner - plain language*

**UAT** (User Acceptance Testing) is where **business users**, not developers, try the feature against real-world scenarios to confirm it does what they actually need before go-live.

### 📏 Limits

*Governor & platform limits*

- Needs prod-like (Partial/Full Copy) sandbox.
- Sandbox refresh intervals/storage limits.
- Requires business stakeholder availability.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Production

*The live org serving real users — the destination of the release pipeline.*

### 🌱 Simple

*Beginner - plain language*

**Production** is the real, live Salesforce org that your business users use every day. Changes only reach it after passing development, testing, and UAT — deployed carefully, usually in a **maintenance window**.

### 📏 Limits

*Governor & platform limits*

- Requires tests + 75% coverage; all-or-nothing.
- Quick deploy validation valid 4 days.
- Change governance/maintenance windows.

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
