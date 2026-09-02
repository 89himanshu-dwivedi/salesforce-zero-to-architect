[Home](../index.md) / [13 · Security Deep Dive](index.md) / **Security Review**

# Security Review

3 topics · Series 13: Security Deep Dive

**Topics on this page**

- [AppExchange Review](#appexchange-review)
- [Vulnerability Scanning](#vulnerability-scanning)
- [Penetration Testing](#penetration-testing)

## AppExchange Review

*Salesforce's mandatory security review for ISV apps.*

### 🌱 Simple

*Beginner - plain language*

The **AppExchange Security Review** is Salesforce's **mandatory assessment** every managed package must pass before being listed/sold on AppExchange — verifying it meets security standards.

### 📏 Limits

*Governor & platform limits*

- Review takes weeks and must be repeated for major versions.
- CRUD/FLS enforcement in Apex is mandatory - `without sharing` needs justification.
- Hardcoded Ids, SOQL injection and missing escaping are automatic failures.
- `global` members become permanent API commitments.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Vulnerability Scanning

*Automated scanning for security weaknesses.*

### 🌱 Simple

*Beginner - plain language*

**Vulnerability scanning** uses automated tools to inspect code and running apps for known **security weaknesses** (injection, XSS, misconfigurations) before attackers find them.

### 📏 Limits

*Governor & platform limits*

- Checkmarx and the Salesforce Code Analyzer report false positives that must be triaged.
- Scanners cover Apex, Visualforce and LWC but not configuration risk.
- Scan results expire and must be regenerated for each submission.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Penetration Testing

*Authorized simulated attacks to find real exploits.*

### 🌱 Simple

*Beginner - plain language*

**Penetration testing** is an **authorized, simulated attack** by skilled testers who actively try to exploit your Salesforce app to find real, exploitable vulnerabilities automated scans miss.

### 📏 Limits

*Governor & platform limits*

- Requires prior notification to Salesforce and must follow their testing policy.
- Only your own org and custom code are in scope - not the platform.
- Load-generating tests may be treated as abuse without approval.

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
