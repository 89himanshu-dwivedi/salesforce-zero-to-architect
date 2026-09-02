[Home](../index.md) / [14 · DevOps & SFDX](index.md) / **Quality Gates**

# Quality Gates

5 topics · Series 14: DevOps & SFDX

**Topics on this page**

- [PMD](#pmd)
- [SonarQube](#sonarqube)
- [Code Coverage](#code-coverage)
- [Static Analysis](#static-analysis)
- [Security Scan](#security-scan)

## PMD

*Static analyzer with Apex/Visualforce rulesets.*

### 🌱 Simple

*Beginner - plain language*

**PMD** is an open-source static code analyzer with built-in **Apex and Visualforce rulesets** — flagging code smells, performance issues, security risks, and style violations before merge.

### 📏 Limits

*Governor & platform limits*

- Rules must be tuned or false positives overwhelm the team and get ignored.
- Apex support lags new language features by a release or two.
- Does not analyse Flows, formulas or validation rules - a major blind spot.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## SonarQube

*Continuous code-quality platform with dashboards.*

### 🌱 Simple

*Beginner - plain language*

**SonarQube** is a code-quality platform that continuously analyzes code for bugs, vulnerabilities, code smells, coverage, and duplication — with dashboards and a configurable **Quality Gate**.

### 📏 Limits

*Governor & platform limits*

- Apex support requires the commercial edition for full rule coverage.
- Quality gates must be agreed or they block releases arbitrarily.
- Metadata and configuration risk are out of scope.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Code Coverage

*Percent of code exercised by tests (75% prod minimum).*

### 🌱 Simple

*Beginner - plain language*

**Code coverage** measures the percentage of Apex lines exercised by tests. Salesforce requires at least **75% org-wide coverage** to deploy to production — but coverage is a floor, not a quality guarantee.

### 📏 Limits

*Governor & platform limits*

- 75% org-wide required for production deployment; every trigger needs coverage.
- Coverage figures can be stale - run all tests before a release-critical deploy.
- Coverage measures executed lines only; assertions are not counted.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Static Analysis

*Analyzing code without executing it.*

### 🌱 Simple

*Beginner - plain language*

**Static analysis** inspects source code **without running it** to find bugs, security flaws, and style issues — the umbrella that includes PMD, ESLint, and Sonar analyzers.

### 📏 Limits

*Governor & platform limits*

- Cannot detect runtime governor limit failures or data-dependent bugs.
- Configuration (flows, validation rules, sharing) is largely uncovered.
- Hardcoded Ids in formulas and flows are missed by Apex-only scanners.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Security Scan

*Automated scanning for security vulnerabilities in CI.*

### 🌱 Simple

*Beginner - plain language*

A **security scan** in the pipeline automatically checks code and dependencies for **security vulnerabilities** (injection, XSS, missing FLS/CRUD, vulnerable libraries) before release.

### 📏 Limits

*Governor & platform limits*

- Checkmarx scan results expire and must be regenerated per submission.
- False positives require documented justification for AppExchange review.
- Scans cover code, not org configuration or sharing model risk.

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
