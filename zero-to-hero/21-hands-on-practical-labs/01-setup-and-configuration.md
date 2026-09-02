[Home](../index.md) / [21 · Hands-On Practical Labs](index.md) / **Setup & Configuration**

# Setup & Configuration

7 topics · Series 21: Hands-On Practical Labs

**Topics on this page**

- [Dev Environment Setup](#dev-environment-setup)
- [Connected App Setup](#connected-app-setup)
- [Named Credential Setup](#named-credential-setup)
- [Remote Site Settings](#remote-site-settings)
- [CSP Trusted Sites](#csp-trusted-sites)
- [Auth Provider Setup](#auth-provider-setup)
- [Permission Set Setup](#permission-set-setup)

## Dev Environment Setup

*Get a working Salesforce dev setup: org + VS Code + CLI + first deploy.*

### 🌱 Simple

*Beginner - plain language*

Before writing any code you need **3 things**: (1) a free Salesforce **Developer org** (your sandbox to play in), (2) **VS Code + Salesforce Extension Pack** (your editor), and (3) the **Salesforce CLI (`sf`)** (the tool that pushes/pulls code between your laptop and the org).

Think of it like web dev: the org is your server, VS Code is your editor, and the CLI is git-push for Salesforce metadata.

### 📏 Limits

*Governor & platform limits*

- Scratch orgs: limited by Dev Hub edition (e.g. 40 active / 80 daily on Enterprise; higher with add-ons).
- Scratch org max lifetime: **30 days** (default 7).
- Metadata API deploy: **10,000 files** or **39 MB** per deploy.
- Deploys per 24h rolling window are capped; large orgs hit this during release week.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Connected App Setup

*The 'door' external systems use to log into Salesforce via OAuth.*

### 🌱 Simple

*Beginner - plain language*

A **Connected App** is how any *outside* system (a website, mobile app, MuleSoft, a Python script) is allowed to talk to Salesforce securely. It gives them a **Client ID** and **Client Secret** (like a username/password for apps) and defines what they're allowed to do (OAuth scopes).

No Connected App = no programmatic login from outside. This is the #1 thing you set up for almost every integration.

### 📏 Limits

*Governor & platform limits*

- Access token lifetime = the session timeout on the profile (default 2 hours).
- Max **5 active access tokens** per user per Connected App - the 6th evicts the oldest.
- Connected App metadata changes take up to **10 minutes** to take effect.
- Daily API request limits still apply and are shared org-wide across all Connected Apps.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Named Credential Setup

*Store the endpoint + auth once, so Apex callouts carry no secrets.*

### 🌱 Simple

*Beginner - plain language*

A **Named Credential** lets Apex call an external system *without* you hard-coding the URL, username, password, or token in code. Salesforce stores the endpoint and the credentials securely, and injects the auth automatically at callout time.

Rule of thumb: **never put passwords/tokens in Apex**. Put them in a Named Credential and reference it by name.

### 📏 Limits

*Governor & platform limits*

- Callout timeout: default 10s, max **120s** via `req.setTimeout()`.
- Max **100 callouts** per transaction; total callout time budget **120s** per transaction.
- Request/response body max **6 MB** (12 MB async).
- No callouts after uncommitted DML in the same transaction.
- Legacy Named Credentials still work but new auth features ship only on External Credentials.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Remote Site Settings

*The allow-list of external URLs Apex is permitted to call.*

### 🌱 Simple

*Beginner - plain language*

By default Salesforce **blocks** Apex from calling any external URL — for security. A **Remote Site Setting** is a simple **allow-list**: you register the external base URL and only then can Apex make a callout to it.

It's the *older/simpler* mechanism. For anything with authentication, prefer a **Named Credential** (which also allow-lists the host *and* handles auth).

### 📏 Limits

*Governor & platform limits*

- No wildcard subdomain support - one entry per host.
- Deploys as metadata (`RemoteSiteSetting`) - must be in your package.xml or it is missing in prod.
- Grants org-wide access; there is no per-profile RSS.
- Stores no credentials, so any auth header must be built (and therefore stored) by you - usually in a Custom Setting, which is exactly what you are trying to avoid.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## CSP Trusted Sites

*Browser-side allow-list so LWC can call external URLs/load resources.*

### 🌱 Simple

*Beginner - plain language*

When your **LWC runs in the browser** and tries to `fetch()` an external site or load an external script/image/font, Salesforce's **Content Security Policy (CSP)** blocks it by default. A **CSP Trusted Site** tells the browser "this external origin is allowed."

Key difference: **Remote Site Setting** = server-side (Apex callouts). **CSP Trusted Site** = client-side (LWC fetch / load in the browser).

### 📏 Limits

*Governor & platform limits*

- CSP is browser-enforced - it cannot be bypassed from Apex.
- Experience Cloud sites have a separate CSP level; the "Relaxed" level is blocked for sites that need security review.
- Inline scripts are always blocked in Lightning - no `eval`, no inline `onclick`.
- Static resources are same-origin and need no CSP entry - the preferred path.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Auth Provider Setup

*Let users log in to Salesforce with Google/Azure/etc. (inbound SSO).*

### 🌱 Simple

*Beginner - plain language*

An **Auth Provider** lets people sign in to your Salesforce/Experience Cloud site using an *external* identity — Google, Microsoft/Azure AD, Facebook, or any OpenID Connect provider. Salesforce becomes the **service provider**; the external system is the **identity provider**.

Use it for: "Log in with Google" on a community portal, or social sign-up for customers.

### 📏 Limits

*Governor & platform limits*

- Registration Handler runs in system context - you own all the security checks.
- Each JIT-created user consumes a licence; there is no "free" provisioning.
- Callback URL is generated by Salesforce and cannot be chosen freely.
- Token refresh depends on the provider issuing `offline_access` / a refresh token - many do not by default.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Permission Set Setup

*Grant access additively without touching profiles.*

### 🌱 Simple

*Beginner - plain language*

A **Permission Set** grants extra access (object/field permissions, Apex classes, tabs, system perms) to specific users **on top of** their Profile. Profiles set the baseline; Permission Sets add capabilities to *some* users without cloning whole profiles.

Modern best practice: keep Profiles minimal and grant almost everything via **Permission Sets** and **Permission Set Groups**.

### 📏 Limits

*Governor & platform limits*

- Max **1,000 permission sets** per org; **200** permission set groups.
- A user can be assigned many permission sets but recalculation of a group can take minutes after edit.
- Permission sets cannot *remove* a permission granted by the profile - only muting sets inside a group can.
- Field-level security still wins over everything: no FLS, no data, regardless of Apex.

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
