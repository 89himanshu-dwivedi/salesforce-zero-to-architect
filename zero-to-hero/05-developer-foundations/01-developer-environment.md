[Home](../index.md) / [05 · Developer Foundations](index.md) / **Developer Environment**

# Developer Environment

6 topics · Series 5: Developer Foundations

**Topics on this page**

- [VS Code](#vs-code)
- [Salesforce Extension Pack](#salesforce-extension-pack)
- [CLI Authentication](#cli-authentication)
- [Org Management](#org-management)
- [Scratch Org Create](#scratch-org-create)
- [Scratch Org Push Pull Delete](#scratch-org-push-pull-delete)

## VS Code

*The official Salesforce IDE — the modern replacement for the deprecated Developer Console & Mavensmate.*

### 🌱 Simple

*Beginner - plain language*

**VS Code** is a free, lightweight code editor from Microsoft. With the **Salesforce Extension Pack** installed, it becomes your main tool to write Apex, LWC, triggers, and to deploy/retrieve metadata to/from your org.

### 📏 Limits

*Governor & platform limits*

- Requires a **JDK** (Java 11+/17) for the Apex Language Server.
- Large orgs: indexing thousands of classes is slow — use **Org Browser** to retrieve selectively rather than all metadata.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Salesforce Extension Pack

*The bundle of VS Code extensions that turns the editor into a full Salesforce IDE.*

### 🌱 Simple

*Beginner - plain language*

The **Salesforce Extension Pack** is a one-click bundle of extensions: Apex, Apex Replay Debugger, Lightning Web Components, SOQL, and the core "Salesforce CLI Integration". Install it once and you get syntax highlighting, autocomplete, deploy/retrieve commands, and debugging.

### 📏 Limits

*Governor & platform limits*

- Requires JDK 11/17 for Apex features.
- Indexing memory grows with org size.
- Replay debugger needs a generated debug log — it is not a live debugger.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## CLI Authentication

*How VS Code/CLI securely connects to a Salesforce org (web login or JWT).*

### 🌱 Simple

*Beginner - plain language*

**CLI Authentication** is how you log the CLI into an org. The simplest way: `sf org login web` opens a browser, you log in, and the CLI stores a token so future commands target that org.

### 📏 Limits

*Governor & platform limits*

- Access tokens expire (session timeout); refresh handled automatically for web, re-signed JWT for CI.
- JWT requires a connected app + cert + pre-authorization.
- MFA does not apply to JWT (by design for automation).

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Org Management

*Tracking and switching between the many orgs (prod, sandboxes, scratch) a developer works with.*

### 🌱 Simple

*Beginner - plain language*

**Org Management** is keeping track of all the orgs you're connected to and choosing which one a command targets. `sf org list` shows them; aliases and a "default" org make switching easy.

### 📏 Limits

*Governor & platform limits*

- Two distinct defaults (target-org vs dev-hub).
- Aliases are local to your machine unless committed in project config.
- Scratch org count is capped by Dev Hub limits.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Scratch Org Create

*Spinning up a fresh, disposable, source-driven org from a config file.*

### 🌱 Simple

*Beginner - plain language*

A **scratch org** is a brand-new, empty, temporary Salesforce org you create on demand for development or testing. `sf org create scratch -f config/project-scratch-def.json -a myScratch` builds one in seconds; you throw it away when done.

### 📏 Limits

*Governor & platform limits*

- Max lifespan **30 days** (default 7).
- **Active/daily scratch org counts** are capped by Dev Hub edition.
- Not a copy of prod data — starts empty (seed it yourself).

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Scratch Org Push Pull Delete

*The source-tracking workflow: push local changes up, pull org changes down, delete when done.*

### 🌱 Simple

*Beginner - plain language*

With scratch orgs you **push** local source to the org (`sf project deploy start` / legacy `force:source:push`), **pull** changes you made in the org UI back to files (`sf project retrieve start` / `force:source:pull`), and **delete** the org when finished (`sf org delete scratch`).

### 📏 Limits

*Governor & platform limits*

- Source tracking is for **scratch orgs and tracking-enabled sandboxes**, not production.
- Deleting a scratch org is **irreversible** and unsaved UI changes are lost.
- Large pushes still subject to metadata deploy limits.

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
