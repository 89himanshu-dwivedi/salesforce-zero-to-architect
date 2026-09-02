[Home](../index.md) / [05 · Developer Foundations](index.md) / **Deployment**

# Deployment

4 topics · Series 5: Developer Foundations

**Topics on this page**

- [Outbound Change Set](#outbound-change-set)
- [Inbound Change Set](#inbound-change-set)
- [Metadata API](#metadata-api)
- [Package XML](#package-xml)

## Outbound Change Set

*A point-and-click bundle of metadata you send from one org to a connected org.*

### 🌱 Simple

*Beginner - plain language*

An **Outbound Change Set** is a list of metadata components (classes, fields, layouts…) you assemble in the source org's UI and **upload** to a connected target org (e.g., sandbox → production).

### 📏 Limits

*Governor & platform limits*

- Only between orgs of the same lineage.
- No deletes, no version control, no automation.
- Manual dependency management.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Inbound Change Set

*Receiving and deploying a change set that was uploaded to your org.*

### 🌱 Simple

*Beginner - plain language*

An **Inbound Change Set** is one that has arrived in the **target** org. There you **validate** it (test deploy) and then **deploy** it to apply the changes.

### 📏 Limits

*Governor & platform limits*

- Validation valid for Quick Deploy for 4 days.
- Prod needs tests + 75% coverage.
- All-or-nothing deploy (full rollback on failure).

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Metadata API

*The programmatic API for deploying/retrieving metadata — the engine behind real CI/CD.*

### 🌱 Simple

*Beginner - plain language*

The **Metadata API** lets tools (the CLI, ANT, CI systems) **retrieve** and **deploy** metadata programmatically — no clicking. It's what powers automated, repeatable deployments.

### 📏 Limits

*Governor & platform limits*

- Asynchronous (poll job status).
- Some metadata types unsupported (use Tooling API).
- File/component size and concurrency limits.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Package XML

*The manifest that declares exactly which metadata components to deploy or retrieve.*

### 🌱 Simple

*Beginner - plain language*

**package.xml** is a manifest file listing the metadata you want to retrieve or deploy — by type and name. It tells the Metadata API *what* to move.

### 📏 Limits

*Governor & platform limits*

- Wildcards don't cover every type (some require explicit names).
- Manifest version tied to API version.
- Large wildcard retrieves can be slow/oversized.

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
