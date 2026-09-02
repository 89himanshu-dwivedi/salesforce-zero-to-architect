[Home](../index.md) / [13 · Security Deep Dive](index.md) / **Security Attacks**

# Security Attacks

6 topics · Series 13: Security Deep Dive

**Topics on this page**

- [SOQL Injection](#soql-injection)
- [SOSL Injection](#sosl-injection)
- [XSS](#xss)
- [CSRF](#csrf)
- [Clickjacking](#clickjacking)
- [Session Hijacking](#session-hijacking)

## SOQL Injection

*Malicious input altering dynamic SOQL queries.*

### 🌱 Simple

*Beginner - plain language*

**SOQL injection** happens when untrusted input is concatenated into a **dynamic SOQL** query, letting an attacker alter the query to access data they shouldn't.

### 📏 Limits

*Governor & platform limits*

- Only dynamic SOQL built by string concatenation is vulnerable.
- `String.escapeSingleQuotes()` protects string literals but not field or object names.
- Bind variables and `Database.queryWithBinds` are the safe options.
- Static analysis (PMD) catches most cases - enforce it in CI.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## SOSL Injection

*Malicious input altering dynamic SOSL searches.*

### 🌱 Simple

*Beginner - plain language*

**SOSL injection** is the SOSL equivalent of SOQL injection — untrusted input concatenated into a dynamic **SOSL** search lets an attacker manipulate the search.

### 📏 Limits

*Governor & platform limits*

- Same risk profile as SOQL injection for `Search.query()`.
- Escaping rules differ - SOSL has additional reserved characters.
- Prefer bind variables over concatenation.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## XSS

*Cross-site scripting: injecting executable script.*

### 🌱 Simple

*Beginner - plain language*

**XSS (Cross-Site Scripting)** is when an attacker injects **malicious script** into a page so it runs in other users' browsers — stealing sessions, data, or performing actions as them.

### 📏 Limits

*Governor & platform limits*

- LWC escapes template expressions automatically; `lwc:dom="manual"` and `innerHTML` do not.
- Visualforce needs explicit `HTMLENCODE`/`JSENCODE` when escaping is disabled.
- Rich text fields are a common stored-XSS vector.
- CSP blocks inline scripts but not all injection paths.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## CSRF

*Cross-Site Request Forgery: forcing unwanted actions.*

### 🌱 Simple

*Beginner - plain language*

**CSRF (Cross-Site Request Forgery)** tricks a logged-in user's browser into sending an **unwanted request** (e.g., changing data) to an app where they're authenticated, using their session.

### 📏 Limits

*Governor & platform limits*

- Salesforce adds CSRF tokens automatically to standard pages and forms.
- Custom Visualforce with `showHeader="false"` or GET-based state changes can bypass protection.
- Apex REST endpoints authenticated by OAuth are not CSRF-protected by session tokens.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Clickjacking

*Tricking users into clicking hidden UI via framing.*

### 🌱 Simple

*Beginner - plain language*

**Clickjacking** embeds a target page in a hidden/transparent **iframe** over decoy content, tricking users into clicking actions they didn't intend (UI redress attack).

### 📏 Limits

*Governor & platform limits*

- Controlled by Session Settings framing options per page type.
- Allowing framing for Experience Cloud sites weakens protection org-wide for that surface.
- Third-party iframes cannot be protected by your settings.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Session Hijacking

*Stealing/reusing a valid session to impersonate a user.*

### 🌱 Simple

*Beginner - plain language*

**Session hijacking** is stealing a user's valid **session ID/token** (via XSS, sniffing, malware) to impersonate them without needing their password.

### 📏 Limits

*Governor & platform limits*

- Session timeout and "Lock sessions to the IP address" are set per profile.
- Session Ids in URLs or logs are a direct compromise vector.
- Debug logs retained 7 days may contain session data.
- High Assurance session requirements can be enforced per profile.

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
