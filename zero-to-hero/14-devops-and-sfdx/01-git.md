[Home](../index.md) / [14 · DevOps & SFDX](index.md) / **Git**

# Git

9 topics · Series 14: DevOps & SFDX

**Topics on this page**

- [Repository](#repository)
- [Clone](#clone)
- [Branch](#branch)
- [Merge](#merge)
- [Rebase](#rebase)
- [Cherry Pick](#cherry-pick)
- [Squash](#squash)
- [Tags](#tags)
- [Releases](#releases)

## Repository

*The versioned source of truth for your codebase.*

### 🌱 Simple

*Beginner - plain language*

A **repository (repo)** is a versioned container for your project — all files plus the full **history** of every change. In Salesforce DevOps, the Git repo (not the org) becomes the source of truth for metadata.

### 📏 Limits

*Governor & platform limits*

- GitHub blocks single files over 100 MB - a large retrieved metadata dump can hit this.
- Binary static resources bloat history permanently; consider Git LFS.
- Never commit `.sfdx/` or auth URLs.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Clone

*Copying a remote repo locally with full history.*

### 🌱 Simple

*Beginner - plain language*

**Cloning** (`git clone <url>`) creates a local copy of a remote repository — including all files, branches, and the complete history — so you can work on it.

### 📏 Limits

*Governor & platform limits*

- Full history clones of metadata-heavy repos can be slow; use shallow clones in CI.
- Cloning does not configure org auth - that is a separate step.
- Case-insensitive filesystems on Windows can collide with case-differing metadata names.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Branch

*A movable pointer for isolated parallel work.*

### 🌱 Simple

*Beginner - plain language*

A **branch** is a lightweight, movable pointer to a commit — letting you develop a feature or fix in **isolation** without affecting the main line, then merge it back.

### 📏 Limits

*Governor & platform limits*

- Long-lived branches diverge and cause painful metadata merges, especially in profiles and layouts.
- Branch names with slashes can break some CI path assumptions.
- Metadata XML merges are error-prone - prefer short-lived branches.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Merge

*Combining branch histories into one.*

### 🌱 Simple

*Beginner - plain language*

**Merging** (`git merge`) combines the changes from one branch into another — typically bringing a feature branch's work into main, creating a **merge commit** that ties the histories together.

### 📏 Limits

*Governor & platform limits*

- Profile, permission set and layout XML merge badly - resolve by regenerating, not by hand.
- Merge conflicts in `package.xml` silently drop components if resolved carelessly.
- A clean merge does not guarantee a valid deploy - always validate.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Rebase

*Replaying commits onto a new base for linear history.*

### 🌱 Simple

*Beginner - plain language*

**Rebasing** (`git rebase`) moves your branch's commits to start from the tip of another branch — replaying them one by one to produce a **linear history** instead of a merge commit.

### 📏 Limits

*Governor & platform limits*

- Rewrites history - never rebase a branch others have pulled.
- Force-push is required afterwards, which needs branch protection exceptions.
- Conflicts must be resolved once per commit, which is painful on metadata.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Cherry Pick

*Applying a specific commit onto another branch.*

### 🌱 Simple

*Beginner - plain language*

**Cherry-picking** (`git cherry-pick <sha>`) copies a **single commit** from one branch and applies it onto another — useful to pull just one fix without merging the whole branch.

### 📏 Limits

*Governor & platform limits*

- Copies a commit without its dependencies - the result may not compile or deploy.
- Creates duplicate commits that complicate later merges.
- Common source of hotfixes that are never forward-ported.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Squash

*Collapsing multiple commits into one.*

### 🌱 Simple

*Beginner - plain language*

**Squashing** combines several commits into a **single commit** — turning a messy series of "WIP" commits into one clean, meaningful change before merging.

### 📏 Limits

*Governor & platform limits*

- Loses individual commit history, which hurts bisecting a regression.
- Squash-merge plus a long-lived branch causes repeated conflicts.
- Commit message quality becomes critical since it is the only record.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Tags

*Named, immutable pointers to specific commits.*

### 🌱 Simple

*Beginner - plain language*

A **tag** is a fixed, named pointer to a specific commit — typically marking a **release version** (e.g., `v1.4.0`). Unlike branches, tags don't move.

### 📏 Limits

*Governor & platform limits*

- Tags are not pushed by default - `git push --tags` is required.
- Moving a tag after release breaks reproducibility of that build.
- Tags alone do not record which org a build was deployed to.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Releases

*Packaged, versioned deliverables built from tags.*

### 🌱 Simple

*Beginner - plain language*

A **release** is a packaged, versioned deliverable of your software at a point in time — usually built from a tag, with release notes, artifacts, and a deployment to an environment.

### 📏 Limits

*Governor & platform limits*

- A release artifact does not capture org-specific config (Custom Setting values, assignments).
- Metadata deploy caps: 10,000 files / 39 MB.
- Deploys per 24-hour rolling window are limited.

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
