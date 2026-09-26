# Work with the GitHub Workflow — Complete Hinglish + Salesforce Architect Guide

> **Source basis:** Uploaded learning material: **Work with the GitHub Workflow**.
>
> **Goal:** Source ke **kisi section, step, command, concept, note, workflow, review point, merge point, sync point, ya quiz question ko intentionally skip nahi kiya gaya hai.**
>
> Additional sections marked **"Gyaan / Enhancement"** are added to make the material more useful for Salesforce Development, DevOps, CI/CD, and Technical/Solution Architect preparation.

---

# 1. Learning Objectives

After completing this unit, you should be able to:

1. GitHub workflow ke steps list karna.
2. Remote aur local working environments ka difference explain karna.
3. New file create karna aur existing file mein changes karna.
4. Code → Collaborate → Ship flow ko samajhna.

## Easy Hinglish

Simple words mein:

```text
Code banao
   ↓
Branch banao
   ↓
Commit karo
   ↓
GitHub par push karo
   ↓
Pull Request
   ↓
Review + Collaboration
   ↓
Tests
   ↓
Deploy
   ↓
Merge
   ↓
Main
```

---

# 2. GitHub Workflow — Overview

GitHub Flow ek lightweight workflow hai jo developers ko new ideas/features safely experiment karne deta hai without directly compromising the project.

Main steps:

```text
1. Create a branch off main
2. Make commits
3. Open a pull request
4. Collaborate
5. Make more commits
6. Discuss and review code with team members
7. Deploy for final testing
8. Merge your branch into main
```

## Hinglish Explanation

Basic idea:

```text
main = stable/project branch

feature branch
      ↓
development
      ↓
Pull Request
      ↓
Review
      ↓
Testing
      ↓
Merge
      ↓
main
```

Source material mein ye complete GitHub Flow sequence diya gaya hai. fileciteturn3file0L10-L20

---

# 3. Create a Branch

Branching Git ka key concept hai.

Git mein project ka kaam branches par hota hai.

Default model mein:

```text
main
```

project ka production version represent karta hai.

Jab new feature ya issue fix karna ho:

```text
main
  ↓
new branch
```

New branch initially `main` jaisi hi hoti hai.

Uske baad jo changes aap new branch mein karte ho, woh initially sirf us branch mein reflect hote hain.

Source explicitly explains that the new branch initially looks like `main`, while changes remain isolated to the branch. fileciteturn3file0L21-L24

## Example

```text
main
 |
 +---- feature/customer-search
```

Developer:

```text
feature/customer-search
```

par kaam karega.

Main branch directly disturb nahi hogi.

---

# 4. Make Commits

Feature branch create karne ke baad files mein changes karo.

Har meaningful change ko commit karo:

```text
Working Directory
       ↓
Staging Area
       ↓
Commit
```

Source says that as changes are made to project files, they should be committed to the feature branch. fileciteturn3file0L26-L27

## Hinglish

Commit ko simple language mein:

> **Code ka ek saved snapshot in Git history.**

Example:

```bash
git add README.md
git commit -m "My first commit"
```

---

# 5. Open a Pull Request and Collaborate

Feature branch par changes ke baad:

```text
Pull Request (PR)
```

open karo.

PR ka purpose sirf merge karna nahi hai.

It is a place for:

- Discussion
- Code review
- Suggestions
- Automated checks
- Refinement
- Collaboration

Source describes a pull request as the starting point for further code refinement, and says it does not need to be perfect when first opened. fileciteturn3file0L29-L30

---

# 6. Best Practice — Open PR Early

Source material specifically recommends opening the Pull Request as early as possible.

Why?

Because early PR gives:

```text
Visibility
   +
Early feedback
   +
Direction correction
```

This can reduce unnecessary work if the implementation later needs to change direction.

Source note: early PRs provide visibility and can help reduce unnecessary work when changes go in a different direction. fileciteturn3file0L31-L33

## Architect Thinking

Senior developer/architect level par:

> PR ko sirf "approval gate" mat samjho. PR is an engineering collaboration mechanism.

---

# 7. Merge to Main Branch

Once:

```text
Team approval
+
Required testing
```

complete ho:

```text
feature branch
      ↓
main
```

merge karo.

Source describes the final step as deploying and merging the approved pull request from the feature branch into the main branch. fileciteturn3file0L35-L36

---

# 8. Complete GitHub Flow

```text
             ┌───────────────┐
             │     main      │
             └───────┬───────┘
                     │
                     │ create branch
                     ↓
             ┌───────────────┐
             │ feature branch│
             └───────┬───────┘
                     │
                     │ changes
                     ↓
                 commits
                     │
                     ↓
              Push to remote
                     │
                     ↓
              Pull Request
                     │
            ┌────────┴────────┐
            │                 │
          Review            Tests
            │                 │
            └────────┬────────┘
                     ↓
                  Deploy
                     ↓
                  Merge
                     ↓
                    main
```

---

# 9. Install Git

Before using the Git examples, install Git on your computer.

The source recommends installing the official Git version and accepting default installation settings. fileciteturn3file0L40-L40

## Why Git?

Git provides:

- Version control
- Branching
- Commit history
- Collaboration
- Merge
- Local development
- Remote synchronization

---

# 10. Sign Up for GitHub and Create a Repository

Source workflow:

1. Sign up for a GitHub personal account.
2. Create a repository.
3. In the header select **Create new → New repository**.
4. For Owner, select your account.
5. Repository name:

```text
best-repo-ever
```

6. Select:

```text
Public
```

7. Under initialization:

```text
Add a README file
```

8. Click:

```text
Create repository
```

These setup steps are part of the supplied learning material. fileciteturn3file0L43-L53

---

# 11. Working on GitHub vs Working Locally

You can modify a project directly on GitHub.

But most developers prefer local development because they can use:

- IDE
- Terminal
- Local tools
- Salesforce CLI
- VS Code
- Git tooling

The source introduces two important terms:

```text
Remote Repository
```

and:

```text
Local Repository
```

---

# 12. Remote Repository

Remote repository means:

> GitHub par stored repository copy.

All collaborators synchronize their changes with the remote repository.

Source describes the remote repository as the copy on GitHub and the source of truth for the group. fileciteturn3file0L54-L58

Conceptually:

```text
                 GitHub
              Remote Repo
                   |
       ┌───────────┼───────────┐
       ↓           ↓           ↓
   Developer A  Developer B  Developer C
```

---

# 13. Local Repository

Local repository means:

> Developer ke computer par stored Git repository.

A linked local repository can contain:

- Files
- Branches
- Commit history
- Git metadata

Source says a local repository is a full copy when linked to the remote repository, including files, branches, and history. fileciteturn3file0L57-L58

---

# 14. Remote ↔ Local Synchronization

Source identifies four important network commands:

```text
git clone
git fetch
git pull
git push
```

The local and remote repositories interact through these network operations. fileciteturn3file0L57-L59

---

# 15. `git clone`

Purpose:

```text
Remote Repository
       ↓
Local Repository
```

Example:

```bash
git clone URL
```

Source workflow:

1. GitHub repository open karo.
2. Code tab par jao.
3. Code button click karo.
4. Clone URL copy karo.
5. Terminal/Git Bash open karo.
6. Run:

```bash
git clone URL
```

Source gives these exact steps. fileciteturn3file0L61-L70

---

# 16. Clone Output

You may see output similar to:

```text
Cloning into 'best-repo-ever'...

remote: Enumerating objects: 3, done.

remote: Counting objects: 100% (3/3), done.

remote: Total 3 (delta 0), reused 0 (from 0)

Receiving objects: 100% (3/3), done.
```

The important meaning:

```text
GitHub
  ↓
Repository copied
  ↓
Local machine
```

---

# 17. Enter the Repository

After clone:

```bash
cd best-repo-ever
```

Now your terminal is inside the local repository.

Source gives this exact navigation step. fileciteturn3file0L81-L82

---

# 18. Configure Your Local Git Environment

Before making changes, basic Git configuration is required.

Git configuration has three levels:

| Level | Command | Scope |
|---|---|---|
| System | `git config --system` | All users on computer |
| Global | `git config --global` | Your user account |
| Local | `git config --local` | Current repository |

Source notes that `--local` is the default configuration scope. fileciteturn3file0L83-L98

---

# 19. `git config --system`

```bash
git config --system
```

System-wide configuration.

Applies to:

```text
All users
```

on that computer.

Usually requires appropriate permissions.

---

# 20. `git config --global`

```bash
git config --global
```

User-level configuration.

Applies to:

```text
Your user account
```

This is commonly used for:

```text
user.name
user.email
```

---

# 21. `git config --local`

```bash
git config --local
```

Repository-level configuration.

Applies only to:

```text
Current repository
```

Default configuration scope is local when no explicit scope is supplied.

---

# 22. View Git Configuration

Use:

```bash
git config --list
```

This displays configuration settings from the relevant configuration levels.

Source explicitly introduces this command. fileciteturn3file0L99-L101

---

# 23. Configure Git Username

Git uses username and email information when creating commit identity.

Set:

```bash
git config --global user.name "First Last"
```

Example:

```bash
git config --global user.name "Himanshu"
```

---

# 24. Configure Git Email

Set:

```bash
git config --global user.email "you@email.com"
```

Use the identity you want associated with your commits.

Source notes that these settings are required for creating commits. fileciteturn3file0L101-L114

---

# 25. `core.autocrlf`

Next important configuration:

```text
core.autocrlf
```

It deals with:

```text
Line endings
```

Different operating systems can use different line-ending conventions.

Without suitable configuration, Git may interpret line-ending differences as file modifications.

Source explains this issue explicitly. fileciteturn3file0L116-L118

---

# 26. Windows Configuration

For Windows:

```bash
git config --global core.autocrlf true
```

Source gives this configuration for Windows. fileciteturn3file0L119-L123

---

# 27. Mac/Linux Configuration

For Mac/Linux:

```bash
git config --global core.autocrlf input
```

Source gives this configuration for Mac/Linux. fileciteturn3file0L124-L128

---

# 28. Authentication / GitHub Desktop

To work with GitHub, you need authentication.

The source mentions:

```text
GitHub CLI
```

for command-line interaction, or:

```text
GitHub Desktop
```

For the learning badge, the material uses GitHub Desktop.

Steps:

1. Download GitHub Desktop.
2. Start it.
3. Select **Sign in to GitHub.com**.
4. Follow authentication steps.
5. On Configure Git, select:
   - Use my GitHub account name and email.
6. Click Finish.

Source gives this exact setup flow. fileciteturn3file0L129-L137

---

# 29. Connect Existing Local Repository to GitHub Desktop

Since the repository has already been cloned:

1. Open GitHub Desktop.
2. Select **Add an Existing Repository from your Local Drive** or:
   - File → Add Local Repository
3. Select:

```text
best-repo-ever
```

4. Click Open.
5. Click Add Repository.

The source shows the local repository with:

```text
Current Repository = best-repo-ever
Current Branch = main
```

fileciteturn3file0L138-L145

---

# 30. Start the GitHub Workflow

Now:

```text
Local repository
+
Git configuration
+
GitHub authentication
```

are ready.

The source then moves between:

```text
Command Line
GitHub Desktop
GitHub Web
```

to make a change to:

```text
README.md
```

fileciteturn3file0L146-L146

---

# 31. Step 1 — Create a Branch

First list branches:

```bash
git branch
```

Initially you may see:

```text
main
```

Create a new branch:

```bash
git branch myfeaturebranch
```

Checkout the branch:

```bash
git checkout myfeaturebranch
```

Source gives these exact commands. fileciteturn3file0L148-L156

---

# 32. Branch Output

You may see:

```text
Switched to branch 'myfeaturebranch'
```

This means HEAD has moved to the new branch.

---

# 33. Important Concept — HEAD

Git mein:

```text
HEAD
```

is an important pointer.

When you checkout a branch:

```bash
git checkout myfeaturebranch
```

HEAD moves to that branch.

Source specifically explains that `checkout` moves the HEAD pointer to a different branch. fileciteturn3file0L159-L164

---

# 34. Step 2 — Modify README.md

Now:

1. Open `README.md`.
2. Add content.
3. Save the file.

At this point:

```text
Working Tree
```

contains modifications.

Source then introduces the two-stage commit process. fileciteturn3file0L166-L172

---

# 35. Git's Three Trees

Git organizes local work using three conceptual trees:

```text
1. Working Tree
2. Staging Area / Index
3. History / Repository
```

---

# 36. Working Tree

This is where you actually modify files.

Example:

```text
README.md
```

gets edited.

State:

```text
Working Tree
   ↓
Modified file
```

---

# 37. Staging Area

Before commit:

```bash
git add README.md
```

moves the selected change into:

```text
Staging Area
```

Think:

> "Ye changes next commit mein include karne hain."

Source describes staging as the place where a discrete unit of work is assembled before taking a snapshot. fileciteturn3file0L172-L180

---

# 38. Commit / History

After staging:

```bash
git commit -m "My first commit"
```

Git creates a snapshot of the staged changes.

Conceptually:

```text
Working Tree
      ↓ git add
Staging Area
      ↓ git commit
History
```

---

# 39. Step 2 Commands

Check status:

```bash
git status
```

Initially README may appear under:

```text
Changes not staged for commit
```

Then:

```bash
git add README.md
```

Check again:

```bash
git status
```

Now README appears under:

```text
Changes to be committed
```

Then:

```bash
git commit -m "My first commit"
```

Finally:

```bash
git status
```

should show:

```text
nothing to commit, working tree clean
```

These are all source steps. fileciteturn3file0L182-L202

---

# 40. Working → Staging → History

```text
             git add
Working  ---------------->  Staging
  Tree                         Area
                               |
                               | git commit
                               ↓
                            History
```

---

# 41. Step 3 — Send Changes to Remote

At this point:

```text
Commit exists locally
```

but:

```text
Remote GitHub repository
```

doesn't have the branch/change yet.

Therefore you need to:

```text
Push
```

Source explicitly explains that the commit is local until pushed to the remote repository. fileciteturn3file0L203-L204

---

# 42. Publish Branch

Using GitHub Desktop:

```text
Open GitHub Desktop
       ↓
Publish branch
```

This sends the local branch to GitHub.

Source gives these steps. fileciteturn3file0L206-L209

---

# 43. Step 4 — Create a Pull Request

Now the branch exists remotely.

Go to GitHub:

```text
Pull Requests
   ↓
New Pull Request
```

Set:

```text
Base:
main
```

and:

```text
Compare:
myfeaturebranch
```

Then:

```text
Create pull request
```

Source gives this exact flow. fileciteturn3file0L210-L223

---

# 44. Pull Request Content

Source example:

Title:

```text
My first commit
```

Description:

```text
Optional
```

Then:

```text
Create pull request
```

---

# 45. Code Quality Through Code Review

Pull Requests are more than branch comparisons.

They are a mechanism for:

```text
Human review
+
Automated validation
```

Source explicitly frames PRs as a way to review code and ensure quality through both human and automated efforts. fileciteturn3file0L224-L225

---

# 46. General Conversation

GitHub PR Conversation tab allows:

```text
General comments
```

Examples:

```text
Why was this design chosen?
Can we simplify this?
Should this be covered by a test?
```

---

# 47. Line Comments

On:

```text
Files Changed
```

you can hover over a line and use the blue `+` icon.

Then add a line-specific comment.

This is useful for:

```text
Specific code feedback
```

rather than generic comments.

Source describes this line-level commenting workflow. fileciteturn3file0L227-L233

---

# 48. Review

While making line comments, you can:

```text
Start a Review
```

A review lets you group:

```text
Multiple line comments
+
Summary
```

When submitting, you can indicate:

```text
Comment
Approval
Request changes
```

---

# 49. Protected Branches

Pull Request reviews become especially powerful when combined with protected branches.

You can configure rules such as:

```text
At least one review required
```

before merge.

Source explicitly mentions protected branches as a way to prevent a PR from being merged without required review. fileciteturn3file0L235-L236

---

# 50. Automated Tests

If CI/CD is integrated:

```text
Tests
```

can report directly on the Pull Request.

This provides:

```text
Code Review
+
Automated Testing
```

in the same workflow.

Source notes that these tests are highly customizable. fileciteturn3file0L238-L239

---

# 51. Deploy

Once:

```text
Team member reviews
+
PR approved
+
Required tests pass
```

the branch can be deployed for final verification.

The source says the branch can be deployed and changes verified in production, and if issues occur, the existing main branch can be redeployed as a rollback mechanism. fileciteturn3file0L241-L242

---

# 52. Merge Your Changes

When merging:

```text
feature branch
       ↓
main
```

Git takes:

```text
Content
+
History
```

from the feature branch and adds them to the main branch.

Source explicitly describes merge this way. fileciteturn3file0L244-L245

---

# 53. Merge in GitHub

Source steps:

1. Go to Conversation tab.
2. Click **Merge pull request**.
3. Add commit message.
4. Add extended description if needed.
5. Confirm merge.

fileciteturn3file0L247-L253

---

# 54. Who Should Merge a Pull Request?

Source lists several possible policies.

## Option 1

PR creator merges.

Benefit:

```text
Creator resolves merge issues
```

---

## Option 2

A single designated team member merges.

Benefit:

```text
Consistency
```

Potential issue:

```text
Bottleneck
```

---

## Option 3

Someone other than the PR creator merges.

Benefit:

```text
At least one independent review
```

The source presents all three options and notes the team should establish its own rules. fileciteturn3file0L254-L258

---

# 55. Keep Everything in Sync

After merging the PR:

```text
Delete branch on GitHub
```

Source instructs deleting the branch from the Pull Request screen. fileciteturn3file0L259-L260

But:

> GitHub par merge/delete karne se local repository automatically update nahi hoti.

You need to sync local state.

---

# 56. Update Local Main

Switch to main:

```bash
git checkout main
```

Then:

```bash
git pull
```

This gets the changes from GitHub and updates your current local branch.

Source gives these exact steps. fileciteturn3file0L262-L269

---

# 57. What Does `git pull` Actually Do?

Source note:

```bash
git pull
```

is a combination of:

```bash
git fetch
+
git merge
```

So:

```text
Remote changes
      ↓
git fetch
      ↓
Local remote-tracking information
      ↓
git merge
      ↓
Current branch updated
```

Source explicitly explains this relationship. fileciteturn3file0L269-L271

---

# 58. Git Commands — Quick Table

| Command | Main Purpose |
|---|---|
| `git clone URL` | Remote repo ki local copy create karna |
| `git branch` | Branches list karna |
| `git branch name` | New branch create karna |
| `git checkout name` | Branch switch karna |
| `git status` | Current working state dekhna |
| `git add file` | Change staging area mein rakhna |
| `git commit -m "..."` | Staged snapshot create karna |
| `git push` | Local commits remote par bhejna |
| `git fetch` | Remote changes/info retrieve karna |
| `git pull` | Fetch + merge |
| `git merge` | Histories/changes combine karna |

---

# 59. Remote vs Local — Interview Explanation

### Remote

```text
GitHub
```

Team-level shared repository.

### Local

```text
Developer machine
```

Individual working copy.

### Sync

```text
Local → Remote
    git push

Remote → Local information
    git fetch

Remote → Current local branch
    git pull
```

---

# 60. `git fetch` vs `git pull`

## `git fetch`

```text
Remote changes retrieve
```

but generally does not automatically merge them into your current branch.

Conceptually:

```text
Remote
  ↓
fetch
  ↓
local remote-tracking refs
```

## `git pull`

```text
fetch
+
merge
```

Source explicitly states this relationship. fileciteturn3file0L269-L271

---

# 61. `git push` vs `git pull`

Easy memory trick:

```text
push = local → remote

pull = remote → local
```

Example:

```text
Developer
   |
   | git push
   ↓
GitHub
```

and:

```text
GitHub
   |
   | git pull
   ↓
Developer
```

---

# 62. `git clone` vs `git pull`

```text
clone
```

is normally used to create the initial local repository copy.

```text
pull
```

is used later to update the current local branch with remote changes.

---

# 63. `git add` vs `git commit`

```text
git add
```

means:

> "Is change ko next snapshot mein include karo."

```text
git commit
```

means:

> "Staged changes ka snapshot history mein save karo."

---

# 64. `git status` — Daily Developer Command

Use:

```bash
git status
```

to understand:

```text
Current branch
Modified files
Untracked files
Staged files
Uncommitted changes
```

It is one of the most useful commands while learning Git.

---

# 65. Git Three-Tree Mental Model

```text
┌────────────────────┐
│   Working Tree     │
│ File modifications │
└─────────┬──────────┘
          │ git add
          ↓
┌────────────────────┐
│   Staging Area      │
│ Next commit content │
└─────────┬──────────┘
          │ git commit
          ↓
┌────────────────────┐
│       History       │
│     Git commits     │
└────────────────────┘
```

---

# 66. HEAD — Advanced

Think:

```text
HEAD
 ↓
Current checked-out branch/commit
```

When you run:

```bash
git checkout myfeaturebranch
```

HEAD moves to that branch.

This is why your next commits are associated with that branch.

---

# 67. Branch Isolation — Why It Matters

Suppose:

```text
main
```

contains stable code.

You create:

```text
feature/payment
```

Then:

```text
feature/payment
```

can move forward:

```text
main:      A---B---C
                 \
feature:          D---E---F
```

Main remains unchanged until merge.

---

# 68. Pull Request — Architect View

PR should be treated as a:

```text
Quality Gate
```

with:

```text
Human Review
+
Automated Tests
+
Deployment Validation
+
Change Discussion
```

This is especially useful in Salesforce projects because metadata changes can have cross-object and deployment dependencies.

---

# 69. Salesforce Connection

In a Salesforce project:

```text
Git branch
   ↓
Salesforce source
   ↓
Scratch org / sandbox
   ↓
Apex tests
   ↓
LWC tests
   ↓
Package validation
   ↓
Deployment
```

GitHub Flow gives the collaboration framework around this.

---

# 70. Salesforce Metadata Example

Suppose developer changes:

```text
force-app/main/default/classes/AccountService.cls
```

and:

```text
force-app/main/default/lwc/accountSearch/
```

Feature branch:

```text
feature/account-search
```

Then:

```text
git status
git add .
git commit
git push
```

PR:

```text
feature/account-search
        ↓
main/develop
```

CI can run:

```text
Apex tests
LWC tests
Static analysis
Deployment validation
```

---

# 71. Salesforce + Unlocked Packages Connection

From the earlier modular-package learning:

```text
GitHub Workflow
      +
Unlocked Packages
```

can become:

```text
Feature Branch
      ↓
Source validation
      ↓
Package creation
      ↓
Package installation
      ↓
Sandbox testing
      ↓
UAT
      ↓
Production
```

Important:

> Git manages source/history/collaboration. Packages manage deployable modular Salesforce artifacts.

---

# 72. GitHub Flow vs Git Flow

Do not confuse:

```text
GitHub Flow
```

with a more elaborate:

```text
Git Flow
```

The supplied unit teaches a lightweight GitHub workflow:

```text
main
  ↓
feature branch
  ↓
PR
  ↓
review
  ↓
merge
```

The earlier Salesforce modular-packaging Part 4 material discussed a more elaborate branch model involving:

```text
feature
packaging
develop
master
hotfix
release
```

These are related ideas but not identical workflows.

---

# 73. Architect Insight — Choose the Simplest Workflow That Works

Don't blindly copy a branching strategy.

Ask:

```text
Team size?
Release frequency?
Production risk?
Package count?
CI speed?
Compliance?
Approval process?
Hotfix frequency?
```

Then choose the workflow.

---

# 74. GitHub Flow for Small/Agile Teams

Typical:

```text
main
 |
 +-- feature-A
 |
 +-- feature-B
 |
 +-- bugfix-C
```

Each goes through:

```text
PR
 ↓
review
 ↓
tests
 ↓
merge
```

Simple and fast.

---

# 75. More Complex Enterprise Flow

For a larger Salesforce enterprise:

```text
feature/*
   ↓
package validation
   ↓
UAT
   ↓
release
   ↓
production
```

May require additional branches or environment gates.

---

# 76. Code Review — What Should a Salesforce Reviewer Check?

## Functional

```text
Does requirement work?
```

## Apex

```text
Bulkification?
SOQL/DML?
Sharing?
Security?
Governor limits?
```

## LWC

```text
Reactive state?
Performance?
Accessibility?
Error handling?
```

## Metadata

```text
Dependencies?
Profiles/permission sets?
Flows?
Layouts?
Flexipages?
Record types?
```

## Deployment

```text
Can it deploy cleanly?
Are dependencies available?
```

---

# 77. Salesforce PR Checklist

```text
[ ] Requirement understood
[ ] Apex bulk-safe
[ ] SOQL selective where needed
[ ] CRUD/FLS considered
[ ] Sharing behavior checked
[ ] Error handling
[ ] Test coverage
[ ] LWC tests if applicable
[ ] Metadata dependencies checked
[ ] Profiles/Permission Sets reviewed
[ ] Flows reviewed
[ ] Deployment validation passed
[ ] Documentation updated
```

---

# 78. CI/CD Enhancement

A mature Salesforce PR pipeline can be:

```text
PR opened
   ↓
Git diff
   ↓
Code formatting
   ↓
Static analysis
   ↓
Validate metadata
   ↓
Run Apex tests
   ↓
Run LWC tests
   ↓
Dependency/package validation
   ↓
Security checks
   ↓
Deployment validation
   ↓
PR status
```

---

# 79. Automated Quality Gate

Ideal rule:

```text
PR cannot merge
        unless
Required checks pass
```

Example:

```text
Apex Tests       ✓
Static Analysis  ✓
Deployment       ✓
Code Review      ✓
Security         ✓
```

Then:

```text
Merge allowed
```

---

# 80. Protected Main Branch

For production-sensitive Salesforce projects:

```text
main
```

should ideally be protected.

Possible controls:

```text
No direct push
Required PR
Required review
Required CI
Status checks
```

This extends the source's protected-branch concept. fileciteturn3file0L235-L236

---

# 81. Rollback Thinking

Source describes redeploying the existing main branch if deployed changes cause issues.

Architect-level improvement:

```text
Before deployment:
Known good version

After deployment:
New version

If failure:
Restore known-good state
```

For Salesforce, exact rollback mechanics depend on whether you are using:

```text
Metadata deployment
Unlocked packages
Data changes
Destructive changes
```

Data rollback is a separate concern from source rollback.

---

# 82. Important Gotcha — Git Rollback ≠ Data Rollback

If you revert:

```text
Git commit
```

you have not automatically reverted:

```text
Salesforce data
```

Example:

```text
Apex rollback
≠
Record rollback
```

Architect must treat:

```text
Code
Metadata
Configuration
Data
```

as different rollback domains.

---

# 83. Important Gotcha — Main Is Not Always Production

The learning material uses:

```text
main
```

as the default production version representation.

In real enterprise projects, teams may use:

```text
main
master
release/*
```

depending on their process.

Always document:

```text
Which branch represents Production?
```

---

# 84. Important Gotcha — Local Branch Can Be Stale

Suppose:

```text
Remote main
```

has new changes.

Your local main doesn't automatically update.

You need:

```bash
git pull
```

or a deliberate:

```bash
git fetch
git merge
```

workflow.

---

# 85. Important Gotcha — Delete Remote Branch ≠ Delete Local Branch

After PR merge:

```text
GitHub branch deleted
```

does not necessarily mean your local branch is automatically deleted.

Local cleanup may still be needed.

---

# 86. Important Gotcha — PR Should Not Be the First Time Anyone Sees the Work

Source recommends opening PR early.

Why?

```text
Early visibility
        ↓
Early feedback
        ↓
Less rework
```

This is particularly valuable for architectural changes.

---

# 87. Important Gotcha — Commit Granularity

Avoid:

```text
commit everything
```

with unclear history.

Prefer meaningful commits:

```text
Add Account service
Add test coverage
Fix null handling
Update metadata
```

This improves:

```text
Review
Debugging
History
Cherry-pick/recovery
```

---

# 88. Advanced — Commit vs Pull Request

Commit:

```text
Unit of source history
```

PR:

```text
Unit of collaboration/review
```

Merge:

```text
Integration into target branch
```

Deployment:

```text
Delivery to environment
```

These are related but different concepts.

---

# 89. Advanced — Four Important Boundaries

```text
Code Boundary
   = Commit

Collaboration Boundary
   = Pull Request

Integration Boundary
   = Merge

Deployment Boundary
   = Environment deployment
```

Architects should keep these concepts separate.

---

# 90. Advanced — Source of Truth

The remote repository is described in the source as the team's source of truth.

But in Salesforce enterprise architecture, distinguish:

```text
Git source of truth
+
Package artifact
+
Org runtime state
```

They must be deliberately synchronized.

---

# 91. Advanced — Salesforce Org Drift

If someone makes manual Production changes:

```text
Production
   ≠
Git
```

then drift exists.

Example:

```text
Git says field label = A
Production says field label = B
```

A mature team should detect and resolve this.

---

# 92. Advanced — Metadata Deployment

A Salesforce CI pipeline may use:

```text
Git
  ↓
Salesforce CLI
  ↓
Scratch Org / Sandbox
  ↓
Validation
  ↓
Deployment
```

or:

```text
Git
  ↓
Package Version
  ↓
Package Install
  ↓
Environment
```

Choice depends on project architecture.

---

# 93. Advanced — Package vs Source-Based Delivery

### Source-based

```text
Git source
   ↓
Deployment
```

### Package-based

```text
Git source
   ↓
Package version
   ↓
Install
```

Package-based delivery provides a versioned artifact boundary.

---

# 94. Advanced — Pull Request as Architecture Review

For major Salesforce changes, PR can ask:

```text
Why this object relationship?
Why this integration pattern?
Why this sharing model?
Why this package boundary?
Why this deployment strategy?
```

This turns code review into architecture governance.

---

# 95. Advanced — Dependency Review

Before merging:

```text
What does this change depend on?
What depends on this change?
```

For Salesforce:

```text
Object
 ↓
Field
 ↓
Flow
 ↓
Apex
 ↓
LWC
 ↓
Permission Set
 ↓
Lightning Page
```

Cross-metadata dependency should be considered.

---

# 96. Super Advanced — PR as Change Contract

A strong PR can document:

```text
Business problem
Technical solution
Metadata changed
Dependencies
Security impact
Deployment impact
Testing
Rollback
```

Example:

```text
Problem:
Reservation search slow

Solution:
New indexed/search approach

Metadata:
Apex + LWC + Custom Metadata

Dependencies:
Reservation object

Testing:
Apex + LWC + integration

Rollback:
Restore previous package/version
```

---

# 97. Super Advanced — Release Traceability

Ideal chain:

```text
Requirement
   ↓
Story/Ticket
   ↓
Feature Branch
   ↓
Commit
   ↓
Pull Request
   ↓
CI Build
   ↓
Package Version / Deployment Artifact
   ↓
UAT
   ↓
Production
```

This is extremely useful for:

```text
Audit
Troubleshooting
Compliance
Release management
Incident response
```

---

# 98. Super Advanced — GitHub Flow + Salesforce Packages

A mature modular Salesforce flow can look like:

```text
Developer
   ↓
feature/package-x
   ↓
Commit
   ↓
Push
   ↓
PR
   ↓
Code Review
   ↓
CI
   ├── Apex Tests
   ├── LWC Tests
   ├── Static Analysis
   ├── Metadata Validation
   └── Package Validation
   ↓
UAT
   ↓
Approved Package Version
   ↓
Production
   ↓
Tag
```

---

# 99. Super Advanced — Build Once, Promote Same Artifact

Preferred concept:

```text
Create artifact
       ↓
Test artifact
       ↓
UAT artifact
       ↓
Production artifact
```

not:

```text
Build for UAT
       ↓
Rebuild for Production
```

Same artifact reduces "works in UAT but not Production" uncertainty.

---

# 100. Super Advanced — Branch Protection

Recommended enterprise controls:

```text
main
 |
 +-- No direct push
 |
 +-- PR required
 |
 +-- Review required
 |
 +-- CI required
 |
 +-- Security checks
 |
 +-- Deployment validation
```

This aligns with the source's protected-branch concept while extending it for Salesforce delivery.

---

# 101. Super Advanced — GitHub Flow vs Salesforce Release Governance

GitHub Flow tells you:

```text
How source changes move
```

Salesforce release governance tells you:

```text
How Salesforce changes become production changes
```

Enterprise architecture combines both:

```text
GitHub Workflow
       +
Salesforce Deployment Strategy
       +
Package Strategy
       +
Environment Strategy
```

---

# 102. Interview Q&A

## Q1. What is GitHub Flow?

**Answer:**

A lightweight Git workflow where developers create a branch from the main branch, make commits, open a PR, collaborate/review, test/deploy, and merge back into main.

---

## Q2. Why use branches?

**Answer:**

Branches isolate feature or bug-fix work from the stable main line until the change is reviewed and approved.

---

## Q3. What is a remote repository?

**Answer:**

The repository hosted on GitHub that collaborators synchronize with and use as the shared source of truth.

---

## Q4. What is a local repository?

**Answer:**

The Git repository stored on a developer's machine, containing files, branches, and history.

---

## Q5. What is `git clone`?

**Answer:**

It creates a local copy of a remote repository.

---

## Q6. What is `git add`?

**Answer:**

It moves selected changes from the working tree into the staging area.

---

## Q7. What is `git commit`?

**Answer:**

It creates a snapshot of staged changes in Git history.

---

## Q8. What is `git push`?

**Answer:**

It sends local commits/branch changes to the remote repository.

---

## Q9. What is `git pull`?

**Answer:**

It retrieves remote changes and integrates them into the current branch; the source explains it as `git fetch` plus `git merge`.

---

## Q10. What is `git fetch`?

**Answer:**

It retrieves changes/information from the remote without automatically merging those changes into your current branch.

---

## Q11. What is a Pull Request?

**Answer:**

A collaboration and review mechanism where proposed branch changes are discussed, reviewed, tested, and then merged.

---

## Q12. Why open a PR early?

**Answer:**

It provides early visibility and feedback, reducing unnecessary work if the implementation needs to change direction.

---

## Q13. What is the difference between commit and PR?

**Answer:**

A commit is a unit of source history. A PR is a unit of collaboration/review around a set of branch changes.

---

## Q14. What is HEAD?

**Answer:**

HEAD is Git's pointer to the currently checked-out branch/commit context. Checkout changes where HEAD points.

---

## Q15. Why use staging?

**Answer:**

It lets you assemble a deliberate unit of changes before creating a commit snapshot.

---

## Q16. How can protected branches improve quality?

**Answer:**

They can require reviews and status checks before a PR can be merged.

---

## Q17. What happens after PR merge?

**Answer:**

The remote branch may be deleted, and the local repository should be synchronized by switching to the target branch and pulling the latest changes.

---

# 103. Salesforce Architect Interview Q&A

## Q18. How would you integrate GitHub Flow with Salesforce?

**Answer:**

I would use feature branches for Salesforce source changes, PRs for review, CI for Apex/LWC/static/deployment validation, and then deploy either source metadata or a versioned package depending on the application's modularization strategy.

---

## Q19. Where do unlocked packages fit?

**Answer:**

They fit between source control and environment deployment as versioned Salesforce artifacts:

```text
Git
 ↓
Package Version
 ↓
Install
 ↓
Environment
```

---

## Q20. Why not deploy directly from every feature branch to Production?

**Answer:**

Because Production should be protected by review, automated testing, environment validation, release controls, and an explicit deployment process.

---

## Q21. What is the Salesforce equivalent of a build artifact?

**Answer:**

Depending on the strategy, it can be a validated deployment artifact or an unlocked package version.

---

## Q22. What should a Salesforce PR validate?

**Answer:**

At minimum:

```text
Code correctness
Apex tests
Metadata validation
Security
Dependencies
Deployment/package compatibility
```

---

# 104. Errors & Gotchas

### Error 1 — Working directly on main

```text
main
 ↓
direct changes
```

Problem:

```text
No isolation
No review boundary
Higher risk
```

---

### Error 2 — Commit without staging

If a file isn't staged:

```bash
git commit
```

won't include that change.

---

### Error 3 — Forgetting to push

Local:

```text
commit exists
```

Remote:

```text
doesn't have it
```

Fix:

```bash
git push
```

---

### Error 4 — Forgetting to pull

Remote has newer changes:

```text
GitHub = newer
Local = stale
```

Fix:

```bash
git pull
```

when appropriate.

---

### Error 5 — Treating `git pull` as magic

Remember:

```text
pull ≈ fetch + merge
```

---

### Error 6 — Ignoring line endings

Cross-platform teams can see noisy changes if line-ending configuration is inconsistent.

Use appropriate:

```text
core.autocrlf
```

settings.

---

### Error 7 — No PR review

Without review:

```text
More defects
+
Less shared knowledge
+
Less architecture visibility
```

---

### Error 8 — No protected main

Direct changes can bypass:

```text
Review
CI
Approval
```

---

### Error 9 — Git rollback mistaken for Salesforce data rollback

Reverting code doesn't automatically revert business data.

---

# 105. Limits / Constraints

From the supplied material:

- Remote and local repositories are separate until synchronized.
- Local commits are not visible remotely until pushed.
- Local repository doesn't automatically update when GitHub changes.
- Line-ending differences can appear as modifications.
- Commit identity requires configured username/email.
- PR quality depends on configured review and CI policies.
- Deployment/rollback behavior depends on the deployment model.

---

# 106. Practical Developer Workflow

```bash
# Clone
git clone URL

# Enter repository
cd best-repo-ever

# Check branches
git branch

# Create branch
git branch myfeaturebranch

# Switch
git checkout myfeaturebranch

# Check status
git status

# Stage
git add README.md

# Commit
git commit -m "My first commit"

# Push
git push

# Later switch back
git checkout main

# Sync
git pull
```

---

# 107. Better Modern Branch Creation

> **Gyaan / Enhancement**

Modern Git commonly supports creating and switching in one command:

```bash
git switch -c myfeaturebranch
```

This is an enhancement beyond the supplied learning material, which uses:

```bash
git branch myfeaturebranch
git checkout myfeaturebranch
```

Both concepts lead to the same goal:

```text
Create + switch to feature branch
```

---

# 108. Better Daily Developer Routine

```text
Before work:
git status
git pull

Create branch:
git switch -c feature/...

Work:
edit files

Review:
git diff

Stage:
git add ...

Commit:
git commit -m "..."

Push:
git push

PR:
review + CI

Merge:
PR approval

After merge:
git checkout main
git pull
```

---

# 109. Git Diff — Useful Enhancement

Before staging:

```bash
git diff
```

Use it to inspect:

```text
What exactly changed?
```

After staging:

```bash
git diff --staged
```

Use it to inspect:

```text
What exactly will be committed?
```

This is extremely useful for senior developers.

---

# 110. Commit Quality

Bad:

```text
update
fix
changes
test
```

Better:

```text
Add reservation search service
Fix null handling in account selector
Add Apex tests for reservation validation
Update permission set for reservation search
```

Good commit messages improve:

```text
History
Debugging
Release notes
Code review
```

---

# 111. PR Description Template

Use:

```markdown
## Business Problem
What problem are we solving?

## Solution
What changed?

## Salesforce Metadata
- Apex
- LWC
- Flow
- Objects
- Permission Sets
- Custom Metadata

## Dependencies
What does this change depend on?

## Testing
What tests were executed?

## Deployment
What environments were validated?

## Security
CRUD/FLS/sharing considerations?

## Rollback
What is the recovery strategy?
```

---

# 112. Salesforce CI Example

```text
Pull Request
    |
    +--> Git diff
    |
    +--> Apex static analysis
    |
    +--> LWC lint/test
    |
    +--> Salesforce metadata validation
    |
    +--> Apex tests
    |
    +--> Package validation
    |
    +--> Security checks
    |
    +--> Status = PASS/FAIL
```

Only after required gates pass:

```text
Merge
```

---

# 113. GitHub Flow + Unlocked Package Flow

```text
Feature Branch
       ↓
Commit
       ↓
Push
       ↓
PR
       ↓
CI
       ↓
Package Version
       ↓
Install in Test Org
       ↓
Tests
       ↓
UAT
       ↓
Production
       ↓
Git Tag
```

---

# 114. Part 4 Connection — Branching + Packages

Earlier modular development material discussed:

```text
feature/package-name/*
packaging/package-name/*
packaging
develop
master
hotfix
release
```

GitHub Flow material here teaches the simpler core:

```text
main
 ↓
feature branch
 ↓
PR
 ↓
review
 ↓
merge
```

Architect should understand both.

---

# 115. When to Use Simple GitHub Flow

Good fit when:

```text
Small/medium team
Frequent releases
Strong CI
Few long-lived release branches
```

Typical:

```text
main
feature/*
```

---

# 116. When More Branches May Be Needed

Potential reasons:

```text
Long UAT cycle
Multiple supported releases
Complex package lifecycle
Strict Production approval
Hotfix requirements
Parallel release trains
```

Then:

```text
feature
release
hotfix
develop
main/master
```

may be introduced.

But every extra branch adds complexity.

---

# 117. Architect Principle

> **Branching strategy should serve the release strategy, not the other way around.**

Ask:

```text
How often do we release?
What must be tested?
What must be approved?
How independent are packages?
How quickly must hotfixes ship?
```

Then design branches.

---

# 118. Complete End-to-End Mental Model

```text
                GIT / GITHUB
                     |
                     v
              Feature Branch
                     |
                     v
                  Commits
                     |
                     v
               Pull Request
                     |
            +--------+--------+
            |                 |
          Review             CI
            |                 |
            +--------+--------+
                     |
                     v
             Salesforce Validation
                     |
                     v
               Package / Deploy
                     |
                     v
                   UAT
                     |
                     v
               Production
                     |
                     v
                 Git Tag
```

---

# 119. Final Cheat Sheet

## GitHub Flow

```text
Branch
 ↓
Commit
 ↓
Push
 ↓
PR
 ↓
Review
 ↓
Tests
 ↓
Deploy
 ↓
Merge
 ↓
Main
```

## Remote/Local

```text
clone = initial remote → local
push  = local → remote
fetch = remote → local tracking information
pull  = fetch + merge
```

## Three Trees

```text
Working
   ↓ add
Staging
   ↓ commit
History
```

## PR

```text
Discussion
+
Review
+
Automation
+
Approval
```

## Salesforce

```text
Git
 ↓
CI
 ↓
Salesforce validation
 ↓
Package/deployment
 ↓
Sandbox/UAT
 ↓
Production
```

---

# 120. Quiz — Source Questions

The supplied unit ends with a quiz and asks the learner to answer all questions correctly. fileciteturn3file0L273-L274

## Question 1

**What is the first step of the GitHub flow?**

A. Merging  
B. Committing  
C. Branching  
D. Cloning

### Answer

**C — Branching**

Why?

Because the GitHub Flow sequence begins with:

```text
Create a branch off main
```

Source lists branching as the first step. fileciteturn3file0L10-L20

---

# 121. Quiz — Question 2

**Which command is used to synchronize the remote and local working environments?**

A. `git pull`  
B. `git merge`  
C. `git status`  
D. `git commit`

### Answer

**A — `git pull`**

Why?

The source specifically explains that:

```bash
git pull
```

retrieves changes from GitHub and updates the current local branch. It also explains that `git pull` combines:

```bash
git fetch
+
git merge
```

fileciteturn3file0L262-L271

---

# 122. Extra Interview Quiz

## Q3

What is the difference between `git push` and `git pull`?

**Answer:**

```text
push = local → remote
pull = remote → local
```

---

## Q4

What does `git add` do?

**Answer:**

Moves selected changes from working tree to staging area.

---

## Q5

What does `git commit` do?

**Answer:**

Creates a snapshot of staged changes in Git history.

---

## Q6

Why create a feature branch?

**Answer:**

To isolate feature/fix work from the stable main branch.

---

## Q7

Why use PRs?

**Answer:**

For collaboration, discussion, review, automated checks, and controlled merge.

---

## Q8

Why protect main?

**Answer:**

To prevent unreviewed/unvalidated changes from directly entering the stable/production branch.

---

## Q9

What are Git's three trees?

**Answer:**

```text
Working Tree
Staging Area
History
```

---

## Q10

What is HEAD?

**Answer:**

Git's pointer to the currently checked-out branch/commit context.

---

# 123. LinkedIn-Style Summary

## GitHub Workflow — Salesforce Developer/Architect Perspective 🚀

A simple GitHub workflow looks like:

```text
Branch
   ↓
Commit
   ↓
Pull Request
   ↓
Review
   ↓
CI/CD
   ↓
Deploy
   ↓
Merge
   ↓
Main
```

But Salesforce projects add another layer:

```text
Git
 ↓
Salesforce Source
 ↓
Scratch Org / Sandbox
 ↓
Apex + LWC + Metadata Tests
 ↓
Package / Deployment Artifact
 ↓
UAT
 ↓
Production
```

The important distinction:

**Git manages source control and collaboration.**

**Packages/deployment artifacts manage how Salesforce changes are delivered.**

And a Pull Request is not just a merge request.

It is a:

```text
Code Review
+
Architecture Review
+
Security Review
+
Automated Validation
+
Deployment Gate
```

For an architect, the real goal isn't:

> "Which Git command should I use?"

It is:

> "How do I create a controlled, traceable, repeatable path from developer change to Production?"

That is where GitHub Flow, Salesforce DevOps, unlocked packages, CI/CD, and release management come together.

#Salesforce #SalesforceArchitect #Git #GitHub #DevOps #CICD #SalesforceDX #UnlockedPackages #SoftwareArchitecture

---

# 124. Final Architect Notes

Remember these five layers:

```text
1. SOURCE
   Git repository

2. COLLABORATION
   Branch + Pull Request + Review

3. VALIDATION
   CI + Tests + Security + Deployment validation

4. ARTIFACT
   Source deployment / Package Version

5. RELEASE
   UAT → Production → Tag
```

And the most important practical flow:

```text
Developer
   ↓
Feature Branch
   ↓
Commit
   ↓
Push
   ↓
Pull Request
   ↓
Review
   ↓
Automated Tests
   ↓
Salesforce Validation
   ↓
Package / Deployment
   ↓
UAT
   ↓
Production
   ↓
Tag
```

---

# 125. One-Line Memory Trick

```text
BRANCH → COMMIT → PUSH → PR → REVIEW → TEST → DEPLOY → MERGE → SYNC
```

And for Salesforce:

```text
BRANCH → COMMIT → PR → CI → VALIDATE/PACKAGE → UAT → PROD → TAG
```

---

# 126. Final Conclusion

GitHub Workflow ka main purpose hai:

> **Safe, collaborative, reviewable, and repeatable software delivery.**

Salesforce Architect ke perspective se isko aur broad dekho:

```text
Change
 ↓
Isolation
 ↓
Review
 ↓
Automation
 ↓
Validation
 ↓
Artifact
 ↓
Environment
 ↓
Production
 ↓
Traceability
```

Agar ye chain controlled hai, to development team sirf code nahi bana rahi hoti — woh **controlled release architecture** operate kar rahi hoti hai.
