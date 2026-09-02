[Home](../index.md) / [10 · LWC Advanced](index.md) / **Security**

# Security

4 topics · Series 10: LWC Advanced

**Topics on this page**

- [Locker Service](#locker-service)
- [Lightning Web Security](#lightning-web-security)
- [CSP](#csp)
- [Trusted Sites](#trusted-sites)

## Locker Service

*The legacy LWC security architecture sandboxing components.*

### 🌱 Simple

*Beginner - plain language*

**Locker Service** is Salesforce's security architecture that sandboxes Lightning components — isolating namespaces, restricting global access, and enforcing DOM/API rules to protect against malicious or buggy code.

### 📏 Limits

*Governor & platform limits*

- Being replaced by Lightning Web Security; behaviour differs between the two.
- Restricts access to `window`, `document` and prototype patching.
- Many third-party libraries are incompatible - test rather than assume.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Lightning Web Security

*The modern, standards-compliant successor to Locker.*

### 🌱 Simple

*Beginner - plain language*

**Lightning Web Security (LWS)** is the modern replacement for Locker Service — providing component isolation with better web-standards compliance and third-party library compatibility.

### 📏 Limits

*Governor & platform limits*

- Distorts global objects in a sandboxed realm per namespace.
- Libraries touching `window.top`, `document.cookie` or prototypes may silently misbehave.
- Cannot be relaxed per component the way CSP can.
- Enabling it org-wide requires regression testing every third-party library.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## CSP

*Content Security Policy restricting external resource loading.*

### 🌱 Simple

*Beginner - plain language*

**CSP (Content Security Policy)** restricts where a page can load scripts, styles, images, and make connections from — mitigating XSS and data exfiltration. Salesforce enforces a strict CSP for LWC.

### 📏 Limits

*Governor & platform limits*

- Enforced by the browser - Apex-side settings cannot bypass it.
- Inline scripts and `eval` are always blocked in Lightning.
- Experience Cloud has its own CSP level that can be stricter than the org setting.
- Each directive must be enabled explicitly per trusted site.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Trusted Sites

*Allow-listing external origins for CSP and connections.*

### 🌱 Simple

*Beginner - plain language*

**CSP Trusted Sites** are entries that allow LWC pages to load resources from or connect to specific external origins — the mechanism to permit otherwise CSP-blocked external content.

### 📏 Limits

*Governor & platform limits*

- CSP Trusted Sites cover browser requests; Remote Site Settings cover Apex callouts.
- Each directive (`connect-src`, `frame-src`, ...) must be ticked individually.
- Every entry widens the org's XSS blast radius.
- Relaxed CSP is blocked for Experience Cloud sites requiring security review.

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
