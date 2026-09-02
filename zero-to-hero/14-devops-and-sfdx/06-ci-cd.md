[Home](../index.md) / [14 · DevOps & SFDX](index.md) / **CI/CD**

# CI/CD

4 topics · Series 14: DevOps & SFDX

**Topics on this page**

- [Jenkins](#jenkins)
- [GitHub Actions](#github-actions)
- [Azure DevOps](#azure-devops)
- [GitLab CI](#gitlab-ci)

## Jenkins

*Self-hosted, plugin-rich automation server.*

### 🌱 Simple

*Beginner - plain language*

**Jenkins** is a popular open-source, self-hosted automation server that runs CI/CD pipelines — building, testing, and deploying Salesforce metadata via the CLI on your own infrastructure.

### 📏 Limits

*Governor & platform limits*

- Requires a self-hosted agent with the sf CLI installed and kept current.
- Auth secrets must come from the credential store, never the repo.
- Apex test execution time dominates pipeline duration.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## GitHub Actions

*Repo-native, YAML-defined SaaS CI/CD.*

### 🌱 Simple

*Beginner - plain language*

**GitHub Actions** is GitHub's built-in CI/CD — workflows defined in YAML in `.github/workflows/` that run on triggers (push, PR, tag) to build, test, and deploy Salesforce metadata.

### 📏 Limits

*Governor & platform limits*

- Free-tier minutes are limited for private repos.
- Secrets are not available to workflows triggered from forks.
- Runner timeout (6 hours default) can be exceeded by very large validations.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Azure DevOps

*Enterprise CI/CD with pipelines, boards, repos.*

### 🌱 Simple

*Beginner - plain language*

**Azure DevOps** provides enterprise CI/CD (**Azure Pipelines**) plus Boards, Repos, and Artifacts — running YAML or classic pipelines to build, test, and deploy Salesforce metadata.

### 📏 Limits

*Governor & platform limits*

- Parallel job limits apply per organisation and tier.
- Service connections must hold the Salesforce auth securely.
- Pipeline timeouts must accommodate Apex test runs.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## GitLab CI

*Integrated CI/CD via .gitlab-ci.yml.*

### 🌱 Simple

*Beginner - plain language*

**GitLab CI/CD** is GitLab's built-in pipeline engine — defined in `.gitlab-ci.yml` — running stages on runners to build, test, and deploy Salesforce metadata, tightly integrated with GitLab repos and MRs.

### 📏 Limits

*Governor & platform limits*

- Shared runner minutes are capped on free tiers.
- Job artifacts expire and cannot be relied on for audit evidence.
- Requires the sf CLI in the runner image.

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
