[Home](../index.md) / [14 · DevOps & SFDX](index.md) / **Salesforce CLI**

# Salesforce CLI

4 topics · Series 14: DevOps & SFDX

**Topics on this page**

- [Auth](#auth)
- [Org Management](#org-management)
- [Source Tracking](#source-tracking)
- [Package Management](#package-management)

## Auth

*Authorizing the CLI to orgs (web, JWT, device).*

### 🌱 Simple

*Beginner - plain language*

**CLI auth** connects the Salesforce CLI (`sf`) to an org so you can deploy, query, and manage it. `sf org login web` opens a browser; other flows suit automation.

### 📏 Limits

*Governor & platform limits*

- Auth URLs and tokens must never be committed - use CI secret storage.
- JWT auth requires a certificate and a pre-authorised user.
- Refresh token expiry policy on the Connected App can silently break CI.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Org Management

*Listing, aliasing, and targeting orgs from the CLI.*

### 🌱 Simple

*Beginner - plain language*

**Org management** commands let you list, alias, set defaults, open, and inspect connected orgs — e.g., `sf org list`, `sf org open`, `sf org display`.

### 📏 Limits

*Governor & platform limits*

- Scratch org active and daily allocations depend on Dev Hub edition.
- Sandbox refresh intervals: Full 29 days, Partial Copy 5 days, Developer 1 day.
- Org aliases are local to the machine and are not shared with the team.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Source Tracking

*Tracking metadata changes between org and local.*

### 🌱 Simple

*Beginner - plain language*

**Source tracking** lets the CLI know what metadata changed in your org vs your local project — so `sf project retrieve start` and `deploy start` can sync only the differences.

### 📏 Limits

*Governor & platform limits*

- Only available for scratch orgs and sandboxes with tracking enabled - never production.
- Tracking state is local and drifts; `sf project reset tracking` is a common fix.
- Manual changes in the org by another user are picked up as conflicts.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Package Management

*Creating, versioning, and installing packages via CLI.*

### 🌱 Simple

*Beginner - plain language*

**Package management** commands build and deploy modular **packages** — `sf package create`, `sf package version create`, `sf package install` — for source-driven, versioned delivery.

### 📏 Limits

*Governor & platform limits*

- Package version creation is asynchronous and can take many minutes.
- Unlocked packages cannot remove components once released without a new major version.
- Package dependencies must be installed in the correct order.

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
