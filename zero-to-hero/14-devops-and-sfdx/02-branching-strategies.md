[Home](../index.md) / [14 · DevOps & SFDX](index.md) / **Branching Strategies**

# Branching Strategies

4 topics · Series 14: DevOps & SFDX

**Topics on this page**

- [Git Flow](#git-flow)
- [Trunk Based Development](#trunk-based-development)
- [Feature Branching](#feature-branching)
- [Release Branching](#release-branching)

## Git Flow

*Structured model with develop, release, hotfix branches.*

### 🌱 Simple

*Beginner - plain language*

**Git Flow** is a branching model with long-lived **main** and **develop** branches plus supporting **feature**, **release**, and **hotfix** branches — a structured, ceremony-heavy workflow.

### 📏 Limits

*Governor & platform limits*

- Multiple long-lived branches multiply metadata merge pain.
- Hotfix branches must be forward-ported or the bug returns next release.
- Heavyweight for orgs that release weekly or more often.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Trunk Based Development

*Everyone integrates to one trunk frequently.*

### 🌱 Simple

*Beginner - plain language*

**Trunk-Based Development (TBD)** has all developers integrate small changes into a single **main (trunk)** branch very frequently (at least daily) — using short-lived branches or direct commits behind feature flags.

### 📏 Limits

*Governor & platform limits*

- Requires feature flags for anything not release-ready - flags need owners and expiry.
- Demands fast, reliable CI; a slow test suite blocks the trunk.
- Apex test execution time becomes the bottleneck at scale.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Feature Branching

*Isolating each feature on its own branch.*

### 🌱 Simple

*Beginner - plain language*

**Feature branching** develops each feature/fix on its own branch off the integration branch, then merges back via a reviewed **pull request** — the most common everyday workflow.

### 📏 Limits

*Governor & platform limits*

- Divergence cost grows with branch age, especially on shared metadata.
- Each branch may need its own scratch org, consuming Dev Hub allocations.
- Scratch orgs live a maximum of 30 days.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Release Branching

*A stabilization branch for an upcoming release.*

### 🌱 Simple

*Beginner - plain language*

A **release branch** is a branch created to **stabilize** an upcoming release — only bug fixes and release prep go in, while new feature work continues on the main/develop line.

### 📏 Limits

*Governor & platform limits*

- Fixes must be applied to both the release branch and the trunk.
- Parallel release branches multiply validation deploy time.
- Deploy frequency limits apply per 24-hour window.

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
