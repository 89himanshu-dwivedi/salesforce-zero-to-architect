# Enterprise Architecture Thinking: Zero to Architect

Hi, I'm **Himanshu Kumar** — Technical Lead & Solution Architect with **8 years** of experience building enterprise systems.

I work at **CRISIL Ltd, an S&P Global company**, on S&P Global projects.

This is my catch-all learning repo for everything that sits *around the application* — messaging, caching, databases, integrations, infrastructure, cloud, AI/GenAI, and the architecture thinking needed to connect them into production-ready enterprise systems.

---

## What You'll Get From Here

# Salesforce Modular Development & Unlocked Packages

A **quick-reference README** for Salesforce's 4-part series on modular development and unlocked packages.

> This README is intentionally short. **Read the official Parts 1–4 for the full deep-dive.** It captures the concepts you should remember and where each part fits.

## 📚 Official 4-Part Series

| Part | Focus | Link |
|---|---|---|
| **Part 1** | What is a package? Why modularize? Discover metadata and dependencies. | [Read Part 1](https://developer.salesforce.com/blogs/2018/06/working-with-modular-development-and-unlocked-packages-part-1) |
| **Part 2** | Organize metadata into modules, `sfdx-project.json`, `.forceignore`, dependencies and FlexiPage/actionOverride gotchas. | [Read Part 2](https://developer.salesforce.com/blogs/2018/06/working-with-modular-development-and-unlocked-packages-part-2) |
| **Part 3** | Create unlocked packages, package versions and package dependencies; validate installation. | [Read Part 3](https://developer.salesforce.com/blogs/2018/06/working-with-modular-development-and-unlocked-packages-part-3) |
| **Part 4** | Git branching, package-aware CI/CD and controlled promotion to production. | [Read Part 4](https://developer.salesforce.com/blogs/2018/06/working-with-modular-development-and-unlocked-packages-part-4) |

---

## 🧠 The Whole Series in One Picture

```text
Existing Salesforce Org
        │
        ▼
Understand Metadata + Dependencies
        │
        ▼
Create Logical Modules
        │
        ▼
Validate Modules in Scratch Orgs
        │
        ▼
Convert Modules → Unlocked Packages
        │
        ▼
Create Package Versions
        │
        ▼
Declare Package Dependencies
        │
        ▼
Validate Clean Installation
        │
        ▼
Git + CI/CD
        │
        ▼
UAT / Integration
        │
        ▼
Promote + Install
        │
        ▼
Production
```

---

## Part 1 — Understand the Problem

**Core idea:** Don't start by packaging the entire org.

First understand:

- What metadata belongs together?
- What depends on what?
- What is a foundation?
- What is a business capability?
- What can be deployed independently?

Think:

```text
Org
 ├── Data Model
 ├── Business Logic
 ├── UI
 ├── Integration
 └── Other Metadata
```

Then identify logical boundaries.

### Key takeaway

**Source control ≠ modularization ≠ packaging**

They are related but different:

```text
Source Control → manages source
Modularization → defines boundaries
Packaging      → versions/distributes modules
```

---

## Part 2 — Organize Metadata

Once dependencies are understood, organize metadata into meaningful modules.

Example:

```text
Base Objects
     ↓
Business Logic
     ↓
UI / Application
```

Typical project concepts:

```text
sfdx-project.json
.forceignore
package directories
source tracking
scratch orgs
```

### Important Gotcha: FlexiPage / actionOverride

A Lightning Record Page can create an `actionOverride` reference on an object.

So:

```text
Object
  └── actionOverride
          └── FlexiPage
```

can create a package dependency you didn't expect.

**Architect lesson:** package boundaries must follow real metadata dependencies, not just folder structure.

### Another Gotcha

`.forceignore` is project-level. Broad wildcard rules can affect more metadata than expected.

---

## Part 3 — Unlocked Packages

After modules are stable:

```text
Module
  ↓
Package
  ↓
Package Version
```

If Package B uses Package A:

```text
Package A
    ↓
Package B
```

A must be available before B.

Think of packages as a dependency graph:

```text
        Data
       /     Business   Integration
    |
    UI
```

### Important validation

Don't only ask:

> "Does it work in my developer org?"

Ask:

> "Can this package be installed into a clean environment with its declared dependencies?"

That exposes hidden dependencies on metadata that already existed in your org.

---

## Part 4 — Git + CI/CD

Packaging becomes powerful when combined with source control and automation.

A simplified flow:

```text
Feature Branch
      ↓
Code Review
      ↓
Tests
      ↓
Package Version
      ↓
UAT / Integration
      ↓
Approved Package Version
      ↓
Production
```

Git can be organized around:

```text
feature/*
   ↓
packaging / develop
   ↓
release
   ↓
production
```

Exact branch names are organization-specific.

### Important principle

Don't necessarily create a package version for every developer commit.

Use fast source-based validation during development and package-version validation at the appropriate release boundary.

---

# 🎯 Architect Mental Model

When designing unlocked-package architecture, ask these questions:

1. **What is the business capability?**
2. **What metadata belongs to it?**
3. **What does it depend on?**
4. **What depends on it?**
5. **Can it be installed independently?**
6. **What is the package dependency order?**
7. **How will CI validate it?**
8. **How will UAT consume the exact version?**
9. **How will production know exactly which version is installed?**
10. **How will emergency production changes return to source control?**

---

# ⚠️ Common Mistakes

```text
❌ Package entire org immediately
❌ Ignore metadata dependencies
❌ Assume folder separation = package independence
❌ Forget FlexiPage/actionOverride dependencies
❌ Test only in an existing org
❌ Create package versions for every tiny change
❌ Allow production changes to remain outside source control
❌ Blindly copy old CLI commands into a modern project
```

---

# 💡 One-Line Summary

> **First design the modules, then understand dependencies, then package them, then version and validate them, and finally connect them to Git/CI/CD for controlled production delivery.**

---

## ⚠️ Version Note

This series was published in **2018**. The architectural concepts remain useful, but Salesforce CLI commands, packaging behavior, limits, and recommended practices have evolved.

For implementation today, use the current Salesforce documentation and modern `sf` CLI syntax.

## 🔗 Official Source

[Salesforce Developers — Working with Modular Development and Unlocked Packages](https://developer.salesforce.com/blogs/2018/06/working-with-modular-development-and-unlocked-packages-part-2)


---

# 📌 Additional GitHub / Git Deep-Dive Content

The detailed learning script also covers the complete GitHub team workflow and Git internals. The README is only a **glimpse/index**; the actual script contains the in-depth explanations, examples, quizzes, and Salesforce-specific applications.

## Team GitHub Workflow

```text
Issue / Task
     ↓
Create Branch
     ↓
Make Changes
     ↓
Atomic Commits
     ↓
Push / Publish Branch
     ↓
Pull Request
     ↓
Code Review
     ↓
Automated Tests
     ↓
Merge
     ↓
Deploy / Validate
     ↓
Cleanup Branch
```

### Covered Topics

- Learning Objectives
- Complete GitHub Workflow
- Branching Strategies
- Short-lived vs Long-running branches
- Branch naming conventions
- Team branching decisions
- Labels, Assignees and Milestones
- Project Boards
- Issue / PR templates
- PR sign-off
- Who should merge
- Protected branches
- Automated tests
- Deploy and rollback concepts
- Merge policies
- Branch deletion
- Local synchronization
- `git clone`
- `git fetch`
- `git pull`
- `git push`
- `git pull = fetch + merge`
- Remote vs Local repository
- Git configuration
- `--system`
- `--global`
- `--local`
- `git config --list`
- `user.name`
- `user.email`
- `core.autocrlf`
- Windows / Mac / Linux configuration
- GitHub authentication
- GitHub Desktop
- Existing repository connection
- `git branch`
- `git checkout`
- `HEAD`
- Working Tree
- Staging Area
- Repository History
- `git status`
- `git add`
- `git commit`
- Push / Publish branch
- Pull Request creation
- Base vs Compare branch
- Conversation comments
- Line comments
- Start Review
- Approval / Request Changes

---

# 🔀 Merge Conflict Deep Dive

The script also covers practical conflict resolution:

```text
Branch A
   │
   ├── README change
   │
   ▼
Branch B
   │
   └── Different README change
          ↓
       PR / Merge
          ↓
       CONFLICT
```

### Conflict Markers

```text
<<<<<<<
Your / Current Branch
=======
Incoming Branch
>>>>>>>
```

Covered:

- `new-branch-1`
- `new-branch-2`
- Conflicting README changes
- `git status`
- `Unmerged Paths`
- Conflict markers
- Manual conflict resolution
- `git add README.md`
- Conflict-resolution commit
- Publish branch
- PR verification
- Merge
- Branch cleanup
- `git checkout main`
- `git pull`

### Architect Principle

> **Git conflict resolution ≠ complete Salesforce validation.**

After resolving a Git conflict:

```text
Git Conflict Resolved
        ↓
Business Logic Validation
        ↓
Salesforce Metadata Validation
        ↓
Security Validation
        ↓
Apex / LWC Tests
        ↓
Integration Tests
        ↓
UAT
        ↓
Production
```

---

# ⚛️ Atomic Commits

The script covers why commits should represent a small, logical unit of work.

```text
Atomic ≠ One Line

Atomic =
One Small
+
Logical
+
Coherent
Unit of Work
```

Covered examples include:

- `bigFile.md`
- Large unrelated changes
- Line 1 / Line 100 changes
- Hunks
- `git add --patch`
- `git add -p`
- Patch selection
- `y`
- `?` for patch options
- Selective staging
- Keeping unrelated work out of a commit

Example:

```text
Working Tree
   │
   ├── Change A
   ├── Change B
   └── Change C
        ↓
git add -p
        ↓
Commit only A
        ↓
Later commit B
        ↓
Later commit C
```

This makes:

- review easier
- rollback safer
- cherry-pick easier
- debugging easier
- release history cleaner

---

# 🧬 How Git Stores Data

The deep-dive also covers Git's internal object model:

```text
File Content
    ↓
   Blob
    ↓
   Tree
    ↓
  Commit
    ↓
Parent Commit
```

Covered concepts:

- Blob
- Tree
- Commit
- SHA-1
- SHA-256 note
- Parent / child commit relationship
- Snapshot model
- Changed vs unchanged file hashes
- Git DAG
- `git log`

### Useful Commands

```bash
git log
git log -10
git log --oneline
git log --oneline --graph
git log --oneline --graph --decorate
```

Specific commit:

```bash
git show <SHA-1>
```

Compare changes:

```bash
git diff
```

The detailed script also covers comparing:

- commits
- branches
- tags

---

# ↩️ Undoing Changes

Covered separately because these commands have very different purposes:

```text
git revert
git commit --amend
git reset
git rebase
```

## `git revert`

Creates a new commit that reverses an earlier commit.

Best suited to:

```text
Already shared history
        ↓
Need a safe undo
        ↓
git revert
```

Also covers:

- revert limitations
- conflicts during revert
- why revert is different from deleting history

## `git commit --amend`

Used to modify the latest commit.

```bash
git commit --amend
```

Important warning:

> Avoid amending commits that other developers already depend on unless you deliberately understand the history rewrite.

## `git reset`

Covered modes:

```bash
git reset --soft
git reset --mixed
git reset --hard
```

Also:

```bash
git reset HEAD~2
```

The detailed script explains what happens to:

```text
HEAD
Index / Staging Area
Working Tree
```

for each reset mode.

---

# 🔀 Merge vs Rebase

Covered concepts:

## Recursive Merge

Combines divergent histories and can create a merge commit.

## Fast-Forward Merge

When the target branch has no divergent commits:

```text
A---B---C
        \
         D
```

can move the branch pointer forward without a merge commit.

## `git rebase`

Replays commits onto another base.

```text
Before:

main:    A---B---C
              \
feature:       D---E


After rebase:

main:    A---B---C
                  \
feature:           D'---E'
```

Important:

> Rebase creates new commit identities because commits are recreated on a new parent/base.

Covered:

- merge history behavior
- replaying commits
- interactive rebase
- `git rebase -i`
- branch divergence
- merge vs rebase decision-making
- rebase risks
- SHA changes
- architect-level history design

---

# 🧯 Advanced Safety: Reflog

Additional advanced topic:

```bash
git reflog
```

Useful for recovering references after operations such as:

- reset
- rebase
- accidental branch movement

Mental model:

```text
Git branch/history looks lost
          ↓
       reflog
          ↓
Find previous HEAD
          ↓
Recover reference
```

---

# 🧩 Salesforce-Specific Git Architecture

The detailed script connects Git concepts to Salesforce DX:

```text
Git Branch
    ↓
Salesforce Source
    ↓
Scratch Org / Dev Environment
    ↓
Validation
    ↓
Pull Request
    ↓
CI
    ↓
Package / Deployment
    ↓
UAT
    ↓
Production
```

Covered Salesforce examples include:

- Salesforce metadata conflicts
- Apex changes
- LWC changes
- Object / Field metadata
- Salesforce DX source format
- PR validation
- Salesforce release/debugging workflow
- Unlocked package integration
- Salesforce CI/CD
- Production rollback / fix-forward thinking

---

# 📦 GitHub + Salesforce + Unlocked Packages

The detailed script makes this distinction explicit:

```text
GitHub Flow
=
Source Collaboration Workflow

Salesforce CI/CD
=
Validation + Deployment Workflow

Unlocked Package
=
Versioned Salesforce Artifact
```

Combined architecture:

```text
Feature Branch
      ↓
Atomic Commit
      ↓
Push
      ↓
Pull Request
      ↓
Code Review
      ↓
CI
      ↓
Salesforce Validation
      ↓
Package / Deployment
      ↓
UAT
      ↓
Production
      ↓
Git Tag
```

---

# 🏗️ Advanced / Super-Advanced Architect Topics

The detailed material also extends into:

- Merge vs Rebase
- Branch divergence
- DAG mental model
- Branch vs Commit mental model
- Rebase SHA behavior
- Conflict prevention
- Atomic commits + rollback
- Atomic commits + cherry-pick
- Fix-forward vs revert
- Feature flags
- Trunk-based thinking
- Release strategy
- Protected branches
- PR templates
- CI/CD governance
- Salesforce metadata dependency management
- Salesforce security validation
- Package dependency validation

---

# 🚨 Errors & Gotchas

Examples covered in the detailed script:

```text
Git conflict resolved
≠
Salesforce deployment is valid
```

Other gotchas:

- Large long-running branches
- Branch divergence
- Mixing unrelated changes in one commit
- Rebasing shared/public branches
- Using `reset --hard` carelessly
- Amending shared commits
- Incorrect PR base branch
- Incorrect compare branch
- Forgetting to pull before merging
- Ignoring Salesforce metadata dependencies
- Assuming Git success means Salesforce success
- Deploying without Apex/LWC validation
- Missing package dependencies
- Production-only metadata differences

---

# 🎯 Practical Decision Tree

```text
Need to undo a shared commit?
        ↓
     revert

Need to edit latest local commit?
        ↓
      amend

Need to move/reset local history?
        ↓
      reset

Need cleaner linear branch history?
        ↓
      rebase

Need to combine divergent histories?
        ↓
      merge

Lost a commit/reference?
        ↓
     reflog
```

---

# 🧪 Complete Validation Model

```text
Code Change
    ↓
Local Tests
    ↓
Atomic Commit
    ↓
Push
    ↓
Pull Request
    ↓
Code Review
    ↓
CI
    ↓
Git Conflict Check
    ↓
Salesforce Metadata Validation
    ↓
Apex / LWC Tests
    ↓
Security Validation
    ↓
Package / Deployment Validation
    ↓
UAT
    ↓
Production
```

---

# 🎤 Interview Coverage

The detailed script includes:

- Git fundamentals
- GitHub workflow
- Branching strategy
- Merge conflicts
- Atomic commits
- Git internals
- Blob / Tree / Commit
- SHA
- Merge vs Rebase
- Revert vs Reset
- Amend
- Reflog
- PR review
- Protected branches
- CI/CD
- Salesforce DX
- Unlocked packages
- Salesforce metadata conflicts
- Salesforce release architecture
- Salesforce Architect interview questions
- Real-world scenarios
- Developer checklist
- Reviewer checklist
- Conflict checklist

---

# 📝 Quiz Preservation

The original learning material's quiz questions and correct answers are preserved in the detailed script.

The enhanced material additionally adds:

- practical scenario questions
- Git command questions
- Salesforce-specific questions
- architect-level questions
- troubleshooting questions
- release-management questions

---

# 📋 Quick Cheat Sheet

```bash
# Repository
git clone <url>
git status
git branch
git checkout <branch>

# Sync
git fetch
git pull
git push

# Stage / Commit
git add <file>
git add -p
git commit -m "message"

# History
git log
git log --oneline --graph --decorate
git show <SHA>

# Compare
git diff
git diff --staged

# Undo
git revert <SHA>
git commit --amend
git reset --soft HEAD~1
git reset --mixed HEAD~1
git reset --hard HEAD~1

# Integrate
git merge <branch>
git rebase <branch>
git rebase -i <base>

# Recovery
git reflog
```

---

# 🧠 Final Git + Salesforce Architect Mental Model

```text
                  BUSINESS CHANGE
                        │
                        ▼
                  FEATURE BRANCH
                        │
                        ▼
                  ATOMIC COMMITS
                        │
                        ▼
                       PR
                        │
                ┌───────┴───────┐
                │               │
             Review             CI
                │               │
                └───────┬───────┘
                        ▼
                 Git Validation
                        │
                        ▼
             Salesforce Validation
                        │
             ┌──────────┼──────────┐
             │          │          │
          Metadata     Tests     Security
             │          │          │
             └──────────┼──────────┘
                        ▼
                Package / Deploy
                        │
                        ▼
                       UAT
                        │
                        ▼
                    Production
                        │
                        ▼
                     Git Tag
```

> **Architect takeaway:** Git manages the evolution of source. GitHub manages collaboration. CI/CD validates and moves changes. Salesforce DX represents the Salesforce source model. Unlocked packages provide versioned Salesforce artifacts. A strong architect connects all five without confusing their responsibilities.

---

# 📚 Source Preservation Rule

For the full learning script:

**Original source content comes first and is not intentionally removed.**

Enhancements are added around it:

```text
Original Source
      ↓
Explain
      ↓
Simplify
      ↓
Add Practical Example
      ↓
Salesforce Example
      ↓
Advanced
      ↓
Architect
      ↓
Interview
```

The README itself remains intentionally short so it works as a **navigation/glimpse file**, while the detailed MD/script contains the complete learning material.
