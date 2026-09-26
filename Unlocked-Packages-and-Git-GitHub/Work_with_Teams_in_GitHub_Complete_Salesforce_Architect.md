# Work with Teams in GitHub — Complete Hinglish + Salesforce Architect Guide

> **Purpose:** Is unit ke **kisi bhi original content, step, command, note, example, conflict-resolution step, atomic-commit concept, `git add -p` flow, ya quiz question ko skip nahi kiya gaya hai.**
>
> Maine original learning flow ko preserve kiya hai aur uske baad **Development, Salesforce DevOps, Advanced, Super Advanced, Interview, Errors & Gotchas, Best Practices aur Architect perspective** add kiya hai.
>
> **Important:** Added sections ko clearly **Gyaan / Enhancement** ke roop mein diya gaya hai, taaki source content aur extra practical knowledge alag samajh aaye.

---

# 1. Learning Objectives

After completing this unit, you’ll be able to:

- GitHub workflow ko teams ke liye effective branching strategy mein translate karna.
- GitHub par merge conflicts resolve karna.
- Atomic commits create karna jo ek single unit of work represent karein.

## Easy Hinglish

Is unit ke 3 main goals hain:

```text
Team Branching
      +
Merge Conflict Resolution
      +
Atomic Commits
```

Agar individual developer ka GitHub Flow samajh aa gaya hai, next challenge hai:

> **"Ab same repository par 5–20 developers kaam karenge to workflow kaise maintain karenge?"**

---

# 2. Team ke Saath GitHub Workflow

Individual workflow:

```text
main
 ↓
feature branch
 ↓
commit
 ↓
PR
 ↓
review
 ↓
merge
```

Team workflow mein ye multiply ho jata hai:

```text
                    main
                      |
        +-------------+-------------+
        |             |             |
        ↓             ↓             ↓
    feature-A     feature-B     feature-C
        |             |             |
      commits       commits       commits
        |             |             |
        +-------------+-------------+
                      |
                    PRs
                      |
                   Review
                      |
                    Merge
```

Team mein active development ke saath:

- Branches badhenge
- Commits badhenge
- Pull Requests badhenge
- Parallel changes badhenge
- Merge conflicts ka chance badhega

Isliye branching strategy important ho jati hai.

---

# 3. Branching Strategies That Work

Team environment mein generally branches ko:

- **Short-lived** hona chahiye.
- Ek specific function/feature/bug ke liye create kiya jana chahiye.
- Merge hone ke baad delete kar dena chahiye.

## Short-Lived Branch

Example:

```text
main
  ↓
feature/reservation-search
  ↓
development
  ↓
PR
  ↓
merge
  ↓
delete branch
```

Short-lived branches ka benefit:

```text
Less confusion
+
More up-to-date code
+
Smaller changes
+
Easier review
+
Easier merge
+
Iterative development
```

Source specifically recommends short-lived branches and explains that they reduce confusion and encourage up-to-date code. 

---

# 4. Why Long-Running Branches Can Become a Problem

Long-running branches intentionally useful ho sakti hain, for example:

```text
development branch
```

ya situations jahan:

```text
multiple deployment levels
```

required hon.

But problem tab hoti hai jab branch bahut time tak main/development se disconnected rahe.

Example:

```text
main
 |
 A---B---C---D---E---F
       \
        feature-X
        |
        X---Y---Z
```

Meanwhile main:

```text
A---B---C---D---E---F---G---H---I
```

feature branch:

```text
A---B---C---X---Y---Z
```

Ab merge ke time differences bahut zyada ho sakte hain.

---

# 5. Long-Running Branch Problems

Source highlights these common problems:

### 1. Out-of-date code

Developer ke paas branch ka latest version nahi hota.

### 2. Merge conflicts

Different branches same areas mein changes kar sakti hain.

### 3. Confusion

Team ko samajhna difficult ho sakta hai ki:

```text
Which branch is current?
Which branch is deployable?
Which branch contains latest changes?
```

### 4. Unnecessary work

Developers ko old branch ko catch-up karne mein extra time lag sakta hai.

---

# 6. Important Principle — Complexity Is Not Automatically Better

Kabhi-kabhi team sochti hai:

> "More branches = more control."

But source ka key message hai:

> More complex branching workflows frequently over-complicated ho sakte hain, aur simpler GitHub Flow often more effectively work karta hai.

Isliye:

```text
Simple
+
Clear
+
Easy to learn
+
Easy to maintain
```

often better starting point hai.

---

# 7. Team se Poochne Wale Branching Questions

Branching strategy define karne se pehle team ko ye questions discuss karne chahiye:

### 1. Which branching strategy will we use?

Example:

```text
GitHub Flow
Git Flow
Feature branches
Release branches
```

---

### 2. Which branch will serve as our main or deployed code?

Clearly define:

```text
main
```

ya:

```text
master
```

ya:

```text
release/*
```

as appropriate.

Most important point:

> Team ko clear hona chahiye ki deployable/stable code kis branch mein represent hota hai.

---

### 3. Will we use naming conventions?

Example:

```text
feature/customer-search
feature/reservation-api
bugfix/null-pointer
hotfix/payment-timeout
release/2026.09
```

Naming conventions se branch purpose immediately clear hota hai.

---

### 4. How will we use labels and assignees?

Issues/PRs par labels:

```text
bug
feature
security
documentation
urgent
salesforce
deployment
```

Assignees:

```text
Responsible developer/reviewer
```

---

### 5. Will we use milestones?

Milestones release/grouping ke liye useful ho sakte hain:

```text
Release 1.0
Release 1.1
Q4 Migration
Salesforce Upgrade
```

---

### 6. Will we use Project Boards / Projects?

Project management ke liye:

```text
Backlog
In Progress
Review
Testing
Done
```

jaise states use kiye ja sakte hain.

---

### 7. Will we have required templates/elements for Issues or Pull Requests?

Example PR template:

```markdown
## Business Problem

## Solution

## Changes

## Testing

## Deployment

## Rollback

## Checklist
- [ ] Tests passed
- [ ] Security checked
- [ ] Documentation updated
```

Source example specifically shipping checklists ko mention karta hai.

---

### 8. How will we indicate sign-off on Pull Requests?

Possible workflow:

```text
Developer Review
      ↓
Technical Review
      ↓
QA
      ↓
Architect Approval
      ↓
Release Approval
```

Exact approvals team policy par depend karenge.

---

### 9. Who will merge Pull Requests?

Team ko pehle decide karna chahiye:

```text
Author?
Tech lead?
Reviewer?
Release manager?
Any approved developer?
```

Clear ownership bottlenecks aur confusion reduce kar sakta hai.

---

# 8. Core Philosophy of Branching

Source ka important concept:

> Branching ki power safe place to make changes mein hai.

Aur:

> Pull Requests ki power review aur testing mein hai.

Therefore:

```text
Branch
  =
Safe Change Isolation

Pull Request
  =
Review + Testing + Collaboration
```

Team branching expectations:

```text
Lightweight
+
Easy to learn
+
Easy to follow
+
Focused on collaboration
```

---

# 9. Recommended Team Mental Model

```text
Developer
   ↓
Short-lived branch
   ↓
Small logical changes
   ↓
Commit
   ↓
Push
   ↓
Pull Request
   ↓
Review + CI
   ↓
Merge
   ↓
Delete branch
```

---

# 10. Handle Merge Conflicts

Team development mein merge conflicts kabhi-kabhi naturally aa sakte hain.

Even individual developer ke workflow mein bhi possible hain.

Initial reaction:

```text
Merge conflict 😨
```

Actual reality:

```text
Merge conflict
      ↓
Git tells you where the conflict is
      ↓
Choose correct final content
      ↓
Remove conflict markers
      ↓
Stage
      ↓
Commit
```

Merge conflicts intimidating lag sakte hain, but basic conflict resolution straightforward hai.

---

# 11. Why Merge Conflicts Happen

Source ke scenario mein conflict tab create hota hai jab:

```text
Multiple commits
+
Separate branches
+
Same section
+
Same file
```

par changes kiye jate hain.

Example:

```text
main:
README line 10 = "Hello"

branch-1:
README line 10 = "Hello Team"

branch-2:
README line 10 = "Hello Developers"
```

Git automatically decide nahi kar sakta ki final line kya honi chahiye.

Therefore:

```text
CONFLICT
```

---

# 12. Conflict Practice Scenario

Source exercise mein:

```text
main
```

se two branches create hoti hain:

```text
new-branch-1
new-branch-2
```

Dono:

```text
README.md
```

ki same line modify karti hain.

---

# 13. Create First Conflicting Branch

Local repository initially:

```text
main
```

par hona chahiye.

## Step 1 — Create `new-branch-1`

```bash
git checkout -b new-branch-1
```

Ye:

```text
branch create
+
branch checkout
```

ek saath karta hai.

---

# 14. Make First Change

`README.md` open karo.

Change karo.

Important:

> Note karo ki kaunsi line change ki.

For conflict practice, same line ko second branch mein bhi change karna hai.

---

# 15. Stage First Change

```bash
git add README.md
```

Meaning:

```text
README change
     ↓
Staging Area
```

---

# 16. Commit First Change

```bash
git commit -m "Changes to the README"
```

Now:

```text
new-branch-1
```

has its own commit.

---

# 17. Publish First Branch

GitHub Desktop mein:

```text
Publish branch
```

click karo.

Ab branch remote GitHub repository mein available hai.

---

# 18. Create First Pull Request

GitHub open karo.

Create PR:

```text
new-branch-1
      ↓
main
```

Example PR title:

```text
Changes to the README
```

---

# 19. Important — No Conflict Yet

At this point:

```text
PR 1
```

mein conflict nahi dikh raha.

Reason:

```text
main
```

mein abhi first branch ka change merged nahi hua.

---

# 20. Create Second Branch

Ab main par wapas jao:

```bash
git checkout main
```

---

# 21. Create `new-branch-2`

```bash
git checkout -b new-branch-2
```

Now:

```text
main
  |
  +--- new-branch-2
```

---

# 22. Make Conflicting Change

`README.md` mein:

> Same line ko change karo jo `new-branch-1` mein change ki thi.

Example:

```text
Branch 1:
Line 10 → Version A

Branch 2:
Line 10 → Version B
```

---

# 23. Stage Second Change

```bash
git add README.md
```

---

# 24. Commit Second Change

```bash
git commit -m "More changes to the README"
```

---

# 25. Publish Second Branch

GitHub Desktop:

```text
Publish branch
```

---

# 26. Create Second Pull Request

GitHub mein:

```text
new-branch-2
      ↓
main
```

Pull Request create karo.

Abhi bhi necessarily conflict visible nahi hoga.

Why?

Because main ne abhi tak branch-1 ke change ko accept nahi kiya tha.

---

# 27. Merge One Pull Request

First PR:

```text
"Changes to the README"
```

from:

```text
new-branch-1
```

merge karo.

Now:

```text
main
```

update ho gaya.

History conceptually:

```text
        branch-1
       /
main--A
       \
        branch-2
```

After branch-1 merge:

```text
main
 |
 A---B
      \
       branch-2
```

where `B` contains branch-1's change.

---

# 28. Why Second PR Now Has a Conflict

Second branch was created from old main:

```text
old main
   |
   +--- branch-2
```

But current main now contains:

```text
branch-1 changes
```

Both branch-1 and branch-2 changed the same section.

Therefore:

```text
branch-2
   +
updated main
   =
CONFLICT
```

---

# 29. Resolve the Conflict on the Other Branch

Open PR:

```text
More changes to the README
```

from:

```text
new-branch-2
```

GitHub will show a merge conflict.

Don't panic.

There are two possibilities:

```text
Simple conflict
   ↓
Resolve in GitHub browser

Complex conflict
   ↓
Resolve locally
```

Source explicitly gives this distinction.

---

# 30. Local Conflict Resolution Strategy

To resolve locally:

```text
main
 ↓
merge main into your branch
 ↓
resolve conflict
 ↓
stage
 ↓
commit
 ↓
push
 ↓
PR becomes conflict-free
```

---

# 31. Step 1 — Check Conflict with `git status`

Run:

```bash
git status
```

Git will identify files with conflicts under:

```text
Unmerged Paths
```

This tells you:

```text
Which file needs resolution?
```

Source specifically instructs using `git status` and looking under `Unmerged Paths`.

---

# 32. Step 2 — Open Conflicted File

Open the conflicted file in your text editor.

For example:

```text
README.md
```

Git inserts special conflict markers.

---

# 33. Merge Conflict Markers

You may see:

```text
<<<<<<<
your/current branch
=======
other branch
>>>>>>>
```

The three important markers are:

```text
<<<<<<<
=======
>>>>>>>
```

These are created by Git to show competing versions.

---

# 34. Understand the Conflict Marker

Example:

```text
<<<<<<< HEAD
Changes from your branch
=======
Changes from incoming/main branch
>>>>>>> main
```

Meaning:

```text
<<<<<<<
Start conflict section

=======
Boundary between versions

>>>>>>>
End conflict section
```

---

# 35. Step 3 — Choose the Correct Code

Both branches' versions are present.

Your job:

```text
Understand both changes
        ↓
Choose correct content
        ↓
Or combine them if business logic requires
```

Then:

```text
Delete conflict markers
```

and save the file.

---

# 36. Important — Conflict Resolution Is Not Always "Pick Mine"

A common beginner mistake:

```text
Always choose current
```

or:

```text
Always choose incoming
```

Wrong approach.

Instead:

```text
Understand requirement
        ↓
Understand both changes
        ↓
Create correct final state
```

Sometimes:

```text
Version A
+
Version B
```

both need to be preserved.

---

# 37. Step 4 — Stage Resolved File

After fixing:

```bash
git add README.md
```

This tells Git:

> "Conflict resolve ho gaya; final file stage kar do."

---

# 38. Step 4 — Commit Resolution

Source command:

```bash
git commit -m "Commit to resolve merge conflict"
```

Now Git records the resolution.

---

# 39. Step 5 — Publish Branch

GitHub Desktop:

```text
Publish branch
```

or, depending on local/remote setup, push the branch using Git.

The important concept:

```text
Resolved local changes
        ↓
Remote branch update
```

---

# 40. Step 6 — Return to Pull Request

GitHub PR refresh karo.

Now:

```text
Conflict
   ↓
Resolved
```

PR should become free of conflicts.

---

# 41. Step 7 — Merge Pull Request

Now second PR merge karo.

Final:

```text
main
 |
 +--- branch-1 changes
 |
 +--- branch-2 resolved changes
```

---

# 42. Clean Up Branches

Branches complete hone ke baad:

```text
Delete branches in GitHub
```

Then local repository sync karo:

```bash
git checkout main
```

and:

```bash
git pull
```

Source explicitly instructs these final cleanup/synchronization steps.

---

# 43. Complete Merge Conflict Flow

```text
main
 |
 +---- new-branch-1
 |          |
 |        change
 |          |
 |         PR
 |          |
 |        merge
 |
 +---- new-branch-2
            |
          change
            |
           PR
            |
         CONFLICT
            |
        git status
            |
       open file
            |
    conflict markers
            |
    choose/combine code
            |
         git add
            |
        git commit
            |
          push
            |
       PR conflict-free
            |
          merge
            |
       delete branches
            |
       git checkout main
            |
         git pull
```

---

# 44. Craft Atomic Commits

Atomic commits are an important part of creating:

```text
Readable history
+
Informative history
```

A good commit should represent:

> **One small, logical unit of change.**

---

# 45. Why Atomic Commits Matter

In a perfect world:

```text
History never needed
```

But real projects mein history useful hoti hai for:

- Debugging
- Understanding changes
- Finding regressions
- Reviewing implementation
- Reverting changes
- Release investigation
- Auditing development history

Therefore commit history should tell a story.

---

# 46. What Is an Atomic Commit?

Atomic commit:

```text
One logical unit of work
```

Example:

```text
Add reservation validation
```

Good.

Another:

```text
Add reservation validation tests
```

Also good if it represents a logically separate unit in the team's workflow.

---

# 47. Bad Commit Example

Suppose one commit contains:

```text
Fix Account bug
+
Add new LWC
+
Rename fields
+
Update documentation
+
Refactor unrelated Apex
+
Change formatting
```

This becomes difficult to:

```text
Review
Understand
Revert
Cherry-pick
Debug
```

---

# 48. Better Commit History

Instead:

```text
Commit 1:
Fix Account validation

Commit 2:
Add Account validation tests

Commit 3:
Add Account search LWC

Commit 4:
Update permission set
```

Each commit has a clear purpose.

---

# 49. `git add --patch`

The unit introduces:

```bash
git add --patch
```

or short:

```bash
git add -p
```

Purpose:

> A file ke different parts ko selectively staging area mein add karna.

This helps create genuinely logical/atomic commits.

---

# 50. Why `git add -p` Is Powerful

Suppose one file has two unrelated changes:

```text
Line 10:
Feature A

Line 100:
Feature B
```

But you want:

```text
Commit 1 → Feature A
Commit 2 → Feature B
```

You can use:

```bash
git add -p
```

and stage only one hunk at a time.

---

# 51. Practice — Download `bigFile.md`

The source exercise asks you to download:

```text
bigFile.md
```

from the Salesforce developer resource:

```text
https://developer.salesforce.com/files/bigFile.md
```

If needed, use browser **Save Target As**.

> **Source exercise link preserved:** the URL above is the one provided in the learning material.

---

# 52. Upload `bigFile.md` to GitHub

In your:

```text
best-repo-ever
```

repository:

1. Click **Upload files**.
2. Select `bigFile.md`.
3. Choose **Commit directly to the main branch**.
4. Click **Commit changes**.

This creates the baseline file in main.

---

# 53. Sync Local Main

First:

```bash
git checkout main
```

Then:

```bash
git pull
```

Now local main contains the uploaded `bigFile.md`.

---

# 54. Check Git Status

Run:

```bash
git status
```

Expected:

```text
nothing to commit
```

Meaning:

```text
Working tree clean
```

---

# 55. Make Two Changes

Now modify:

```text
bigFile.md
```

at:

```text
Line 1
Line 100
```

The actual content of the changes does not matter for this exercise.

Important is:

```text
Two different hunks / areas
```

---

# 56. Use Patch Mode

Run:

```bash
git add -p
```

Git will interactively show chunks/hunks of changes.

You decide:

```text
Stage this?
Yes / No / Other action
```

---

# 57. Press `y`

Source instructs:

```text
y
```

to stage the hunks.

In Git language:

```text
hunk = unit of change
```

So:

```text
y
```

means:

> Yes, is hunk ko staging area mein add karo.

---

# 58. Other Patch Options

If you are unsure what the patch options mean:

```text
?
```

press karo.

Git available options ki list show karega.

This is explicitly part of the source exercise.

---

# 59. `git add -p` Mental Model

```text
File
 |
 +---- Change A
 |
 +---- Change B
 |
 +---- Change C
 |
git add -p
 |
 +---- stage A
 +---- don't stage B
 +---- stage C
```

Then:

```bash
git commit -m "Logical change A and C"
```

and later:

```bash
git add -p
git commit -m "Logical change B"
```

---

# 60. Atomic Commit Example — Salesforce

Suppose one Apex class contains:

```text
Change A:
Bulkification fix

Change B:
New business rule

Change C:
Debug logging
```

You may want separate logical commits:

```text
Commit 1:
Fix bulk processing in ReservationService

Commit 2:
Implement reservation validation rule

Commit 3:
Improve error logging
```

This gives much clearer history.

---

# 61. Atomic Commit + Pull Request

A strong workflow:

```text
Atomic commits
      ↓
Clear PR
      ↓
Easy review
      ↓
Easy debugging
      ↓
Easier rollback
```

---

# 62. Atomic Commit ≠ Tiny Commit

Important distinction:

> Atomic does **not** mean "one line per commit."

Bad interpretation:

```text
Commit 1 → change one line
Commit 2 → change next line
Commit 3 → change another line
```

Better:

```text
One commit = one logical unit of work
```

A logical unit may contain:

```text
Multiple files
Multiple classes
Tests
Metadata
```

if they collectively represent one coherent change.

---

# 63. Atomic Commit Example

Feature:

```text
Add Account Search
```

Potential atomic commit:

```text
Apex service
+
Apex test
+
LWC
+
required metadata
```

if all of those together form one coherent unit of work according to the team's convention.

---

# 64. Advanced — Atomicity and Revertability

Atomic commits improve the ability to reason about:

```text
What changed?
Why changed?
What can be reverted?
```

Example:

```text
Commit A = feature
Commit B = unrelated formatting
Commit C = bug fix
```

If feature A needs rollback:

```text
Reverting A
```

is safer/easier when unrelated changes are not mixed into it.

---

# 65. Advanced — Atomicity and Code Review

Reviewer sees:

```text
Commit 1
   ↓
Logical change

Commit 2
   ↓
Tests

Commit 3
   ↓
Metadata
```

instead of:

```text
Massive mixed commit
```

Review becomes easier.

---

# 66. Advanced — Atomicity and Debugging

Suppose Production regression starts after:

```text
Commit A
```

If commits are meaningful, investigation can focus on:

```text
Commit A
```

rather than a huge commit containing 15 unrelated changes.

---

# 67. Advanced — Atomicity and Git History

Good history should tell a story:

```text
Add validation
 ↓
Add tests
 ↓
Update metadata
 ↓
Fix edge case
```

This is much easier to understand months later.

---

# 68. Advanced — Merge Conflict Prevention

Short-lived branches + frequent synchronization + atomic changes can reduce conflict surface.

Conceptually:

```text
Small branch lifetime
      ↓
Smaller divergence
      ↓
Smaller merge surface
      ↓
Fewer/less complex conflicts
```

This is a practical inference from the source's guidance on short-lived branches and merge conflicts.

---

# 69. Gyaan / Enhancement — Keep Branches Updated

If branch is long-lived, periodically integrate latest target branch.

For example:

```text
feature branch
      ↓
bring latest main
      ↓
resolve conflicts early
      ↓
continue work
```

This can prevent one massive conflict at the end.

Exact strategy may be:

```text
merge main
```

or:

```text
rebase
```

depending on team policy.

---

# 70. `merge` vs `rebase` — Advanced

### Merge

```text
main ----A----B----C
             \
feature       D----E
```

After merge:

```text
main ----A----B----C----M
             \         /
              D-------E
```

Preserves the branch relationship.

### Rebase

Conceptually moves feature commits on top of a newer base:

```text
main ----A----B----C
                  \
feature             D'---E'
```

This can create a cleaner linear history but rewrites commit ancestry.

> Team policy should decide which approach is appropriate.

---

# 71. Important Rebase Gotcha

Avoid rebasing shared/public branches casually.

Why?

Because rebase can rewrite commit history.

For a private feature branch, teams may allow it.

For a shared branch, coordination is required.

---

# 72. Advanced — Conflict Resolution Is a Business Decision

Git can identify:

```text
Text conflict
```

but Git does not know:

```text
Business correctness
```

Example:

```text
Developer A:
Discount = 10%

Developer B:
Discount = 20%
```

Git cannot decide whether final value should be:

```text
10%
20%
15%
```

The team must understand business requirements.

---

# 73. Salesforce-Specific Conflict Example

Imagine:

```text
Developer A:
Permission Set gets new Apex Class

Developer B:
Same Permission Set is modified for another feature
```

Text may merge cleanly, but architect still needs to check:

```text
Security impact
Access impact
Deployment impact
```

So:

> **No Git conflict does not mean no architectural conflict.**

This is a very important Salesforce Architect concept.

---

# 74. Salesforce Metadata Conflicts

Potential areas:

```text
Profiles
Permission Sets
Flows
FlexiPages
Layouts
Custom Objects
Custom Metadata
Apex
LWC
Sharing configuration
```

Two developers may modify related metadata.

Even if Git can merge files, functional behavior should still be reviewed.

---

# 75. Salesforce PR Review Checklist

```text
[ ] Requirement understood
[ ] Branch is focused
[ ] Commit history is logical
[ ] Apex tests included
[ ] LWC tests where needed
[ ] CRUD/FLS checked
[ ] Sharing checked
[ ] Permission Sets reviewed
[ ] Flow dependencies reviewed
[ ] Metadata dependencies reviewed
[ ] Deployment validation passed
[ ] No unrelated changes
[ ] Rollback considered
```

---

# 76. Team Branch Naming Convention

Recommended examples:

```text
feature/<ticket>-<short-description>
bugfix/<ticket>-<short-description>
hotfix/<ticket>-<short-description>
chore/<short-description>
```

Examples:

```text
feature/CRM-142-reservation-search
bugfix/CRM-151-null-pointer
hotfix/CRM-199-payment-timeout
```

Naming should be standardized at team level.

---

# 77. Short-Lived Branch Lifecycle

```text
Create
  ↓
Develop
  ↓
Commit
  ↓
Push
  ↓
PR
  ↓
Review
  ↓
Merge
  ↓
Delete
```

Branch ka purpose complete:

```text
Branch deleted
```

---

# 78. Why Delete Merged Branches?

Too many old branches create:

```text
Noise
Confusion
Maintenance burden
```

Clean repository:

```text
main
+
active feature branches
```

is easier to understand.

---

# 79. PR Template for Team

A team can standardize PRs:

```markdown
## What changed?

## Why?

## Related ticket

## Salesforce metadata

## Testing performed

## Deployment validation

## Security impact

## Dependencies

## Rollback plan

## Checklist
- [ ] Tests passed
- [ ] Review completed
- [ ] No unrelated changes
- [ ] Documentation updated
```

---

# 80. Issue / PR Labels

Example:

```text
feature
bug
hotfix
security
performance
technical-debt
deployment
blocked
needs-review
ready-for-uat
```

Labels should remain lightweight.

Too many labels can create noise.

---

# 81. Milestones

Milestone can group work around:

```text
Release
Project phase
Migration
Major feature
```

Example:

```text
Salesforce Release 2026.10
```

with multiple PRs.

---

# 82. Project Boards

Possible flow:

```text
Backlog
 ↓
Ready
 ↓
In Progress
 ↓
Code Review
 ↓
Testing
 ↓
UAT
 ↓
Done
```

This connects:

```text
Project management
+
GitHub development
```

---

# 83. Architect View — Branch Strategy Is an Operating Model

Branch strategy is not just Git syntax.

It determines:

```text
How developers collaborate
How releases happen
How approvals happen
How conflicts are managed
How production changes are controlled
```

Therefore architect/release lead should understand it.

---

# 84. Architect Question — What Should Be Lightweight?

Avoid unnecessary:

```text
10 permanent branches
20 approval steps
manual duplication
unclear ownership
```

unless business/regulatory needs justify them.

The source's key philosophy is:

```text
Keep branching lightweight
+
Easy to learn
+
Focus on collaboration
```

---

# 85. Advanced Team Model

A practical Salesforce team might use:

```text
main
 |
 +-- feature/CRM-101
 +-- feature/CRM-102
 +-- bugfix/CRM-103
 +-- hotfix/CRM-104
```

Each feature branch:

```text
short-lived
focused
reviewed
tested
merged
deleted
```

---

# 86. More Complex Enterprise Model

If the organization has multiple release levels:

```text
feature/*
     ↓
develop
     ↓
release/*
     ↓
main
```

Hotfix:

```text
main
 ↓
hotfix/*
 ↓
main
 ↓
back-merge as required
```

But don't add branches just because they look "enterprise."

Use them only when release requirements justify them.

---

# 87. GitHub Flow vs More Complex Branching

### Lightweight

```text
main
 ↓
feature
 ↓
PR
 ↓
main
```

### More complex

```text
feature
 ↓
develop
 ↓
release
 ↓
main
```

Complexity increases:

```text
Branch count
+
Merge points
+
Synchronization requirements
+
Conflict possibilities
```

---

# 88. Super Advanced — Branch Divergence

Think of branch divergence as:

```text
How far your branch has moved away
from the target branch.
```

Example:

```text
main:
A-B-C-D-E-F-G

feature:
A-B-C-X-Y-Z
```

Feature branch has diverged from current main.

The longer it lives:

```text
Divergence ↑
Conflict surface ↑
Integration effort ↑
```

This is why short-lived branches are valuable.

---

# 89. Super Advanced — Integration Frequency

A strong team integrates frequently.

Instead of:

```text
2 months feature branch
      ↓
massive merge
```

prefer:

```text
small change
 ↓
PR
 ↓
merge
 ↓
next change
```

where practical.

---

# 90. Super Advanced — Feature Flags

Sometimes a feature is incomplete but needs to merge early.

One strategy is:

```text
Code merged
+
Feature disabled
```

using a feature flag/configuration.

This can allow:

```text
Short-lived branches
+
Continuous integration
```

without exposing unfinished functionality to users.

For Salesforce, feature enablement may be controlled using appropriate configuration patterns, but exact mechanism depends on the application.

---

# 91. Super Advanced — Trunk-Based Thinking

Another development philosophy emphasizes very short-lived branches and frequent integration into the main/trunk line.

Conceptually:

```text
Developer
   ↓
small branch
   ↓
PR
   ↓
main
```

This is close to the lightweight GitHub Flow philosophy.

---

# 92. Super Advanced — Merge Conflict Prevention Strategy

```text
1. Keep branches short-lived
2. Keep commits focused
3. Pull/fetch frequently
4. Avoid unnecessary overlapping edits
5. Communicate ownership
6. Break large work into smaller pieces
7. Use feature flags where appropriate
8. Resolve conflicts early
```

---

# 93. Super Advanced — Conflict Ownership

When conflict occurs:

```text
Git
 ↓
Identifies textual conflict
```

Developer/team:

```text
Determines correct business behavior
```

Architect:

```text
Determines system/design impact
```

QA:

```text
Validates behavior
```

This creates a collaborative conflict-resolution model.

---

# 94. Super Advanced — Atomic Commits + Release Traceability

Ideal:

```text
Requirement
   ↓
Issue
   ↓
Branch
   ↓
Atomic commits
   ↓
Pull Request
   ↓
CI
   ↓
Deployment artifact
   ↓
UAT
   ↓
Production
```

This gives strong traceability.

---

# 95. Super Advanced — Atomic Commits + Rollback

Suppose:

```text
Commit A = feature
Commit B = unrelated refactor
Commit C = documentation
```

If A causes a problem, clean history makes it easier to reason about rollback.

If all are mixed:

```text
One giant commit
```

then rollback becomes more difficult.

---

# 96. Super Advanced — Atomic Commits + Cherry-Pick

If a fix is isolated in a clean commit:

```text
commit abc123
```

it may be easier to apply that specific fix to another branch using:

```bash
git cherry-pick abc123
```

This is one reason logical commits are valuable.

> Use cherry-pick carefully and according to team release policy.

---

# 97. Salesforce Release Example

Suppose:

```text
Production
```

has a critical bug.

Developer creates:

```text
hotfix/CRM-500-payment-error
```

Then:

```text
Fix
 ↓
Atomic commit
 ↓
PR
 ↓
Review
 ↓
Tests
 ↓
Production
```

Afterward the fix should be integrated into the appropriate ongoing development/release line according to the team's branching model.

---

# 98. Errors & Gotchas

## Gotcha 1 — Long-lived feature branch

Problem:

```text
Huge divergence
```

Solution:

```text
Short-lived branch
```

where practical.

---

## Gotcha 2 — Resolving conflict blindly

Don't simply choose:

```text
ours
```

or:

```text
theirs
```

without understanding the requirement.

---

## Gotcha 3 — Forgetting conflict markers

Never commit:

```text
<<<<<<<
=======
>>>>>>>
```

into final source.

---

## Gotcha 4 — Conflict file staged too early

After editing, verify the final content before:

```bash
git add
```

---

## Gotcha 5 — No test after conflict resolution

A successful merge means:

```text
Git conflict resolved
```

It does **not** necessarily mean:

```text
Application behavior correct
```

Always run relevant tests.

---

## Gotcha 6 — Huge commit

Large mixed commits are hard to:

```text
Review
Revert
Debug
Understand
```

---

## Gotcha 7 — One-line commits

Atomic does not mean:

```text
one line
```

Atomic means:

```text
one logical unit
```

---

## Gotcha 8 — Mixing unrelated changes

Avoid:

```text
Feature A
+
Formatting
+
Random refactor
+
Documentation
```

in one commit unless the work is intentionally one coherent unit.

---

# 99. Errors & Gotchas — Salesforce Specific

## Gotcha 9 — Git says clean, Salesforce still broken

Possible:

```text
Metadata dependency
Permission issue
Flow behavior
Sharing issue
Runtime integration issue
Data issue
```

Git cleanliness is not Salesforce functional correctness.

---

## Gotcha 10 — No Git conflict but business conflict exists

Two developers can edit different files while creating incompatible business behavior.

Example:

```text
Flow A expects status = Approved
Apex B changes status model
```

Git may merge without conflict.

Architecture review still matters.

---

## Gotcha 11 — Profile/Permission conflicts

Profiles and permissions can create noisy or risky changes.

Prefer controlled permission management strategies appropriate to your Salesforce architecture.

---

# 100. Limits / Constraints

Important practical constraints:

- Short-lived branches reduce risk but cannot guarantee zero conflicts.
- Git can detect textual conflicts but cannot understand business intent.
- Atomic commits improve history but require developer discipline.
- `git add -p` requires understanding which hunks belong together.
- Complex branching can support complex release requirements but increases operational overhead.
- A conflict-free PR is not automatically a functionally correct PR.
- Git history does not represent Salesforce runtime data history by itself.
- Branch cleanup must be coordinated with any process that still depends on the branch.

---

# 101. Best Option — General Team Strategy

For many teams, a practical baseline is:

```text
main
  |
  +-- short-lived feature branches
  |
  +-- Pull Requests
  |
  +-- required CI
  |
  +-- review
  |
  +-- merge
  |
  +-- delete branch
```

Why?

Because it keeps:

```text
Process simple
+
Branches fresh
+
Review focused
+
Integration frequent
```

This is consistent with the source's lightweight GitHub Flow philosophy.

---

# 102. Best Option — Salesforce Team Enhancement

For a Salesforce team:

```text
feature branch
      ↓
Apex/LWC/metadata changes
      ↓
Atomic commits
      ↓
PR
      ↓
Code review
      ↓
Salesforce validation
      ↓
Apex tests
      ↓
Security checks
      ↓
UAT
      ↓
Production
```

---

# 103. Practical Command Sheet

## Create branch

```bash
git checkout -b new-branch-1
```

## Return to main

```bash
git checkout main
```

## Create second branch

```bash
git checkout -b new-branch-2
```

## Check status

```bash
git status
```

## Stage file

```bash
git add README.md
```

## Commit

```bash
git commit -m "Changes to the README"
```

## Sync main

```bash
git checkout main
git pull
```

## Atomic staging

```bash
git add -p
```

## Show patch options

```text
?
```

## Stage hunk

```text
y
```

---

# 104. Conflict Resolution Command Flow

```bash
git checkout your-branch

git status

# Merge latest target branch
git merge main

# Resolve file manually

git status

git add README.md

git commit -m "Commit to resolve merge conflict"

git push
```

> The source's exact exercise uses the same conceptual process: inspect conflict, edit markers, `git add`, commit resolution, publish/update the branch, then return to the PR.

---

# 105. Atomic Commit Command Flow

```bash
git status

git diff

git add -p

# choose hunks with:
y

git status

git commit -m "Describe one logical change"
```

Optional:

```bash
git diff --staged
```

to inspect exactly what is going into the commit.

---

# 106. Interview Q&A

## Q1. What is a good branching strategy for a team?

**Answer:**

Keep branches short-lived, focused on a specific unit of work, reviewed through Pull Requests, and deleted after merge. Add long-lived branches only when release/deployment requirements justify them.

---

## Q2. Why are short-lived branches preferred?

**Answer:**

They reduce branch divergence, confusion, stale code, and merge complexity while encouraging frequent integration.

---

## Q3. Are long-lived branches always bad?

**Answer:**

No. They can be appropriate for development/release workflows or multiple deployment levels. The source specifically says they can make sense when intentionally used.

---

## Q4. What causes a merge conflict?

**Answer:**

When changes from different branches overlap in a way Git cannot automatically reconcile, such as changes to the same section of the same file.

---

## Q5. How do you resolve a merge conflict?

**Answer:**

Check `git status`, identify conflicted files, inspect conflict markers, choose or combine the correct changes, remove markers, save, `git add`, commit the resolution, push, and verify the PR.

---

## Q6. What are Git conflict markers?

**Answer:**

```text
<<<<<<<
=======
>>>>>>>
```

They identify the competing versions in a conflicted file.

---

## Q7. What is an atomic commit?

**Answer:**

A commit representing one small, logical unit of change.

---

## Q8. Does atomic mean one line?

**Answer:**

No. Atomic means one logical unit, which can include multiple files and related changes.

---

## Q9. What is `git add -p`?

**Answer:**

Interactive patch staging. It lets you stage selected parts/hunks of a file instead of staging the entire file.

---

## Q10. Why use atomic commits?

**Answer:**

They make history easier to read, review, debug, understand, and selectively revert or reuse.

---

# 107. Salesforce Architect Interview Q&A

## Q11. How would you design Git branching for a Salesforce team?

**Answer:**

I would start with short-lived feature branches and a protected integration/deployment branch, then add release/hotfix branches only if the release model requires them. Every branch should have a clear purpose, naming convention, owner, and merge policy.

---

## Q12. How do you handle Salesforce merge conflicts?

**Answer:**

First identify whether the conflict is textual or architectural. Resolve the Git conflict locally or in GitHub, then run Salesforce-specific validation and tests because a Git-clean merge does not guarantee correct runtime behavior.

---

## Q13. Can Git resolve Salesforce metadata conflicts automatically?

**Answer:**

Git can resolve textual file conflicts when the changes are structurally mergeable, but it cannot determine Salesforce business semantics, dependencies, permissions, runtime behavior, or architectural correctness.

---

## Q14. Why are atomic commits important in Salesforce?

**Answer:**

Salesforce changes can span Apex, LWC, metadata, permission sets, flows, and configuration. Logical commits make the change history easier to review, trace, debug, and potentially revert.

---

## Q15. What should happen after resolving a conflict?

**Answer:**

I would stage and commit the resolution, push the updated branch, verify the PR, run relevant Salesforce tests/validation, and only then merge.

---

## Q16. What if there is no Git conflict?

**Answer:**

I would still review functional and architectural compatibility. No textual conflict only means Git could combine the files; it doesn't prove that the resulting Salesforce behavior is correct.

---

# 108. Advanced Interview Scenario

### Scenario

Two developers modify the same Salesforce Flow.

Developer A:

```text
Adds approval condition
```

Developer B:

```text
Changes decision logic
```

PR merge conflict occurs.

### Architect approach

```text
1. Identify both intended business changes
2. Inspect conflict
3. Determine desired final behavior
4. Merge correct logic
5. Validate Flow
6. Run impacted tests
7. Review dependencies
8. Validate deployment
9. Merge PR
```

The key is:

> Don't treat conflict resolution as only a Git operation.

---

# 109. Real-World Salesforce Example

Imagine:

```text
Team:
10 Salesforce Developers

Repository:
Salesforce DX

Branches:
main
feature/*
bugfix/*
hotfix/*
```

Developer A:

```text
feature/CRM-101-account-search
```

Developer B:

```text
feature/CRM-102-contact-search
```

Both work independently.

Flow:

```text
Developer A
    ↓
feature branch
    ↓
atomic commits
    ↓
PR
    ↓
review + CI
    ↓
merge

Developer B
    ↓
feature branch
    ↓
atomic commits
    ↓
PR
    ↓
review + CI
    ↓
merge
```

Branches deleted after merge.

---

# 110. Real-World Conflict Example

Suppose both developers edit:

```text
AccountService.cls
```

same method:

```apex
getAccounts()
```

Developer A:

```text
Adds filtering
```

Developer B:

```text
Changes pagination
```

Git may show:

```text
<<<<<<<
A's implementation
=======
B's implementation
>>>>>>>
```

Team must create a final implementation that supports:

```text
Filtering
+
Pagination
```

if both requirements are valid.

---

# 111. Real-World Atomic Commit Example

Instead of:

```text
"Big Account Feature"
```

with 40 files, use meaningful logical commits where practical:

```text
Add Account search service
Add Account search tests
Add Account search LWC
Add required permission metadata
```

The exact split depends on what the team considers a coherent unit of change.

---

# 112. Developer Checklist

Before opening PR:

```text
[ ] Branch is focused
[ ] Branch is reasonably current
[ ] No unrelated changes
[ ] Commits are logical
[ ] Tests added
[ ] Local status clean after commit
[ ] Conflict check completed
[ ] PR description complete
```

---

# 113. Reviewer Checklist

```text
[ ] Requirement matches implementation
[ ] Commit history understandable
[ ] No unrelated changes
[ ] Error handling
[ ] Security
[ ] Performance
[ ] Tests
[ ] Salesforce metadata dependencies
[ ] Deployment impact
[ ] Rollback strategy
```

---

# 114. Conflict Resolution Checklist

```text
[ ] Run git status
[ ] Find Unmerged Paths
[ ] Open conflicted file
[ ] Find <<<<<<<
[ ] Understand both versions
[ ] Decide final business behavior
[ ] Remove conflict markers
[ ] Save file
[ ] git add
[ ] git commit
[ ] git push/publish
[ ] Verify PR
[ ] Run tests
[ ] Merge
[ ] Delete branch
[ ] checkout main
[ ] git pull
```

---

# 115. Atomic Commit Checklist

```text
[ ] One logical purpose
[ ] No unrelated formatting
[ ] No random refactor
[ ] Relevant tests included
[ ] Commit message explains change
[ ] Review staged diff
[ ] Use git add -p if necessary
```

---

# 116. Quiz

The unit asks you to answer all quiz questions correctly.

---

## Quiz 1 — Typically, how long should branches exist?

### Options

1. It doesn't matter.
2. Shorter is better all the time.
3. Shorter is best, but long-lived branches may be appropriate in some cases.
4. Long-living branches are better for small teams.

### Correct Answer

**3. Shorter is best, but long-lived branches may be appropriate in some cases.**

### Why?

Source explicitly says:

- Branches should generally be short-lived.
- Long-lived branches can make sense intentionally for development branches or multiple deployment-level code situations.

So:

```text
Short-lived = default preference
Long-lived = valid when intentionally justified
```

---

# 117. Quiz 2 — What Is the Right Amount of Change in a Commit?

### Options

1. About 2 hours' worth of work
2. All changes necessary to implement the requested feature
3. A single line of code
4. Enough for a small, logical unit of change

### Correct Answer

**4. Enough for a small, logical unit of change.**

### Why?

The unit's atomic commit section says:

> Each commit should be a small, logical unit of change and tell the story of the repository.

Therefore:

```text
Atomic ≠ one line
Atomic ≠ fixed number of hours
Atomic ≠ necessarily entire feature
```

The key is:

```text
Small + Logical + Coherent
```

---

# 118. Quiz Memory Trick

### Branch lifetime

```text
SHORT by default
LONG only when justified
```

### Commit size

```text
LOGICAL UNIT
```

Remember:

```text
Branch = focused work
Commit = logical change
PR = collaboration/review
Merge = integration
```

---

# 119. Final Cheat Sheet

## Branching

```text
Short-lived
Focused
Named consistently
Reviewed
Merged
Deleted
```

## Merge Conflict

```text
git status
   ↓
Unmerged Paths
   ↓
Open file
   ↓
<<<<<<<
=======
>>>>>>>
   ↓
Choose/combine correct code
   ↓
Remove markers
   ↓
git add
   ↓
git commit
   ↓
push
   ↓
PR
   ↓
test
   ↓
merge
```

## Atomic Commits

```text
One logical unit
```

Use:

```bash
git add -p
```

to stage selected hunks.

---

# 120. Complete Team Workflow

```text
                       GitHub Repository
                              |
                            main
                              |
          +-------------------+-------------------+
          |                   |                   |
          ↓                   ↓                   ↓
   feature/101         feature/102         feature/103
          |                   |                   |
      commits             commits             commits
          |                   |                   |
          ↓                   ↓                   ↓
         PR                  PR                  PR
          |                   |                   |
       Review              Review              Review
          |                   |                   |
         CI                  CI                  CI
          |                   |                   |
          +-------------------+-------------------+
                              |
                           Merge
                              |
                            main
                              |
                       Delete branches
```

---

# 121. Salesforce Team Workflow

```text
Salesforce Developer
        ↓
Feature Branch
        ↓
Atomic Commits
        ↓
Pull Request
        ↓
Code Review
        ↓
Salesforce CI
   ├── Apex tests
   ├── LWC checks
   ├── Metadata validation
   ├── Security checks
   └── Deployment validation
        ↓
UAT
        ↓
Production
        ↓
Tag / Release
```

---

# 122. Architect Mental Model

Remember these four layers:

```text
BRANCH
Safe isolation

COMMIT
Logical history

PULL REQUEST
Collaboration + review

MERGE
Controlled integration
```

And for conflicts:

```text
Git identifies the conflict
Developer resolves the code
Business decides the correct behavior
Tests validate the result
Architect evaluates system impact
```

---

# 123. Final Takeaways

### 1. Keep branches short-lived

```text
Short branch
→ less divergence
→ less confusion
→ easier merge
```

### 2. Keep branching simple

Don't introduce complexity unless release requirements justify it.

### 3. Learn conflict resolution

Conflict is not failure.

It is Git asking:

> "I found two changes that I cannot safely combine automatically. You decide."

### 4. Understand conflict markers

```text
<<<<<<<
=======
>>>>>>>
```

### 5. Create atomic commits

```text
One logical unit of work
```

### 6. Use `git add -p`

```bash
git add -p
```

for selectively staging hunks.

### 7. A clean Git merge is not enough

For Salesforce:

```text
Git success
≠
Salesforce functional success
```

Always validate behavior, metadata, permissions, dependencies, and tests.

---

# 124. One-Line Memory Trick

```text
SHORT BRANCH → SMALL LOGICAL COMMITS → PR → REVIEW → TEST → RESOLVE → MERGE → DELETE → SYNC
```

For Salesforce:

```text
SHORT BRANCH
→ ATOMIC CHANGES
→ PR
→ CODE + SECURITY REVIEW
→ SALESFORCE VALIDATION
→ TEST
→ UAT
→ PRODUCTION
```

---

# 125. Final Architect Conclusion

Team GitHub workflow ka real goal sirf:

```text
Git commands yaad karna
```

nahi hai.

Real goal hai:

```text
Safe Collaboration
        ↓
Controlled Changes
        ↓
Reviewable History
        ↓
Predictable Integration
        ↓
Validated Deployment
        ↓
Traceable Production Release
```

Salesforce environment mein isko aur expand karo:

```text
Developer
   ↓
Branch
   ↓
Atomic Commit
   ↓
Pull Request
   ↓
Code Review
   ↓
CI/CD
   ↓
Salesforce Metadata Validation
   ↓
Apex/LWC Tests
   ↓
Security Validation
   ↓
UAT
   ↓
Production
   ↓
Release Traceability
```

**Architect mindset:**

> Branching strategy should support the team's delivery model, not become the delivery model itself.

