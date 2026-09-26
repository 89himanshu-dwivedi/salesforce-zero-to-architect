# Work with Your History in Git — Complete Hinglish + Development + Salesforce Architect Guide

> **Source-preservation rule:** Is document mein uploaded unit ka **koi original topic, command, explanation, note, resource, merge strategy, quiz question, ya answer intentionally skip nahi kiya gaya hai**. Original sequence aur terminology preserve ki gayi hai.
>
> **Enhancement:** Original content ke baad practical Hinglish explanation, development examples, Salesforce-specific application, advanced/super-advanced concepts, errors & gotchas, limits, interview Q&A, checklists aur architect mindset add kiya gaya hai.
>
> **Important distinction:** Jahan content original unit se directly liya gaya hai, usko source flow ke according explain kiya gaya hai. Added material ko **Gyaan / Enhancement** sections mein rakha gaya hai.

---

# 1. Learning Objectives

After completing this unit, you’ll be able to:

- Describe how Git stores data and outline one practical application of this knowledge.
- View your project history and changes with Git.
- Use Git commands that let you undo previous changes.
- Summarize how rebase is related to common merge strategies.

## Easy Hinglish

Is unit mein Git ko sirf commands ke level par nahi, balki **andar se kaise history store karta hai** us level par samjhenge.

Main areas:

```text
Git Data Model
      ↓
Commit History
      ↓
Diff / Show
      ↓
Undo Changes
      ↓
Reset
      ↓
Merge Strategies
      ↓
Rebase
```

---

# 2. How Git Stores Data

## Original Concept

Jab hum commits discuss karte hain, commit ko project ka **snapshot** samjha ja sakta hai.

Har snapshot mein bahut information hoti hai.

Git mein files compressed form mein store hoti hain aur har file/object ko ek unique **SHA-1 hash** milta hai.

In file contents ko:

```text
Blob
```

kehte hain.

Blobs ko:

```text
Tree
```

reference karta hai.

Aur tree ko:

```text
Commit
```

reference karta hai.

So simplified structure:

```text
Commit
  |
  └── Tree
       |
       +── Blob
       +── Blob
       +── Blob
```

### Easy Hinglish

Git directly sirf:

```text
"file.txt"
```

store karne ke idea par dependent nahi hai.

Instead conceptually:

```text
File Content
    ↓
Blob / object
    ↓
Tree
    ↓
Commit
```

Commit ek snapshot ko point karta hai.

---

# 3. Blob, Tree, Commit — Mental Model

## Blob

Blob basically file content ko represent karta hai.

```text
README.md
   ↓
content
   ↓
blob
```

## Tree

Tree directory/file structure ko represent karta hai.

```text
Project
 |
 +-- README.md
 +-- src/
 |    +-- app.js
 |
 +-- tests/
```

Tree is structure ko organize karta hai.

## Commit

Commit:

```text
Snapshot
+
Metadata
+
Parent reference
```

represent karta hai.

---

# 4. SHA-1 Hash

Jab file ka content change hota hai, Git us changed content ke liye new SHA-1 hash generate karta hai.

Example conceptual:

```text
README version 1
       ↓
SHA-1 = abc123...

README version 2
       ↓
SHA-1 = xyz789...
```

Agar file unchanged hai, uska existing hash reuse ho sakta hai.

---

# 5. Changed vs Unchanged Files

Suppose:

```text
Commit A
 ├── README → hash-111
 ├── app.js → hash-222
 └── test.js → hash-333
```

Now only `app.js` changes.

New snapshot conceptually:

```text
Commit B
 ├── README → hash-111   ← unchanged
 ├── app.js → hash-999   ← changed
 └── test.js → hash-333  ← unchanged
```

Isliye Git ka content-addressed object model efficient ho sakta hai.

---

# 6. Parent Commit Relationship

Jab file ka SHA-1 hash change hota hai, new object previous state se relationship maintain karta hai.

Similarly commits bhi previous commit ko parent ke roop mein reference karte hain.

Example:

```text
Commit A
   ↓
Commit B
   ↓
Commit C
   ↓
Commit D
```

Conceptually:

```text
D → parent C
C → parent B
B → parent A
```

---

# 7. Why Parent Relationship Matters

Commit history ka:

```text
parent / child relationship
```

Git ke liye extremely important hai.

It creates:

```text
Consistent History
```

and this relationship helps Git understand how branches relate to each other and how changes can be merged.

---

# 8. SHA-1 Visibility in GitHub

GitHub UI mein commits ke liye generally SHA-1 identifier ka shortened form dikh sakta hai.

Source specifically notes that the first seven characters of the SHA-1 hash can be seen on a commit.

Example:

```text
Full:
4e3dc9b7f0....

Displayed:
4e3dc9b
```

Ye short identifier commands mein commit ko refer karne ke liye commonly useful hota hai.

---

# 9. Important Note — SHA-256

Source ka note:

Although SHA-1 hashes are being used, Git project has chosen **SHA-256** as successor to SHA-1 and the transition work was in progress when this learning material was written.

### Practical understanding

Is note ka main learning point:

```text
Git object identity
        ↓
Hash-based
```

Aur hash algorithm evolution possible hai.

> **Important:** Ye source-era note hai. Modern Git environments mein SHA-256 transition ki current state ko alag se verify karna chahiye if implementing a specific production strategy.

---

# 10. Explore Your History with Git

Project work ke during commit history dekhna useful hota hai.

GitHub.com par:

```text
Repository
   ↓
Code
   ↓
Commits
```

area se project history dekhi ja sakti hai.

Local machine par:

```bash
git log
```

use kar sakte ho.

---

# 11. `git log`

Command:

```bash
git log
```

Current branch ke commits ki list display karta hai.

By default output detailed ho sakta hai.

Example:

```text
commit abc123...
Author: ...
Date: ...

    Add reservation validation
```

Agar repository mein many commits hain, output large ho sakta hai.

Isliye modifiers useful hain.

---

# 12. `git log -10`

Command:

```bash
git log -10
```

Sirf latest **10 commits** show karta hai.

Use case:

```text
Recent work quickly inspect karna
```

---

# 13. `git log --oneline`

Command:

```bash
git log --oneline
```

Readable compact history deta hai.

Example:

```text
a91d3f2 Add reservation validation
b18e4aa Add reservation tests
c9a77d1 Update README
```

Isme:

```text
Short SHA
+
Commit message
```

show hota hai.

Source notes that first seven characters of the SHA-1 hash are displayed in this compact history style.

---

# 14. `git log --oneline --graph`

Command:

```bash
git log --oneline --graph
```

ASCII graph mein history show karta hai.

Example:

```text
* a91d3f2 Add feature
* b18e4aa Add tests
| * c9a77d1 Other branch work
|/
* d112abc Initial commit
```

Branches aur commits ka relationship samajhne mein useful hai.

---

# 15. `git log --oneline --graph --decorate`

Command:

```bash
git log --oneline --graph --decorate
```

Ye:

```text
Graph
+
Commit
+
Branch names / references
```

show karta hai.

Example:

```text
* a91d3f2 (HEAD -> feature/search) Add search
* b18e4aa Add tests
| * c9a77d1 (main) Release fix
|/
* d112abc Initial commit
```

`decorate` branch/tag references ko visible banata hai.

---

# 16. History Commands Cheat Sheet

```bash
git log
git log -10
git log --oneline
git log --oneline --graph
git log --oneline --graph --decorate
```

Mental model:

```text
git log
   ↓
Detailed

git log -10
   ↓
Recent 10

git log --oneline
   ↓
Compact

git log --oneline --graph
   ↓
Compact + branches

git log --oneline --graph --decorate
   ↓
Compact + branches + references
```

---

# 17. Compare Versions of Files

Commit create karne se pehle changes inspect karna useful hai.

Question:

> "Mere working directory aur staging area/history mein actually kya change hua?"

Iske liye:

```bash
git diff
```

use hota hai.

---

# 18. `git diff`

By default:

```bash
git diff
```

working directory ke changes ko compare/review karne mein help karta hai.

Simple mental model:

```text
Last committed state
        ↓
Working directory
        ↓
git diff
```

---

# 19. Working Directory vs Staging Area

Git ke important states:

```text
HEAD / Last Commit
        ↓
Working Directory
        ↓
Staging Area
        ↓
Next Commit
```

More precise mental model:

```text
HEAD
 ↓
Index / Staging
 ↓
Working Tree
```

`git diff` aur `git diff --staged` different comparisons karte hain.

---

# 20. `git diff --staged`

Added enhancement:

Agar tumne:

```bash
git add file.txt
```

kar diya hai aur ab dekhna hai ki **next commit mein exactly kya jayega**, use:

```bash
git diff --staged
```

ya:

```bash
git diff --cached
```

use kar sakte ho.

Mental model:

```text
git diff
=
Unstaged changes

git diff --staged
=
Staged changes
```

---

# 21. Compare Any Two Commits

Source explains that `git diff` can compare:

- Two commits
- Two branches
- Two tags

Example source command:

```bash
git diff 4e3dc9b 0cd75d4
```

This compares the two referenced commits.

---

# 22. Compare Branches

Example:

```bash
git diff main feature/search
```

This can help inspect what differs between the two refs.

Useful before:

```text
PR
merge
release
```

---

# 23. Compare Tags

Example:

```bash
git diff v1.0 v1.1
```

This can show changes between release points.

Useful for:

```text
Release review
Regression investigation
Change summary
```

---

# 24. `git show <SHA-1>`

If you want to inspect a specific previous commit:

```bash
git show <SHA-1>
```

Example:

```bash
git show 4e3dc9b
```

This displays details of that commit.

---

# 25. What `git show` Can Tell You

Source notes that it includes information such as:

```text
Commit author
Commit date/time
Changes made
Affected assets/files
```

So:

```text
git log
=
History overview

git show
=
Specific commit details
```

---

# 26. History Investigation Flow

```text
Something changed?
       ↓
git log
       ↓
Find suspicious commit
       ↓
git show <SHA>
       ↓
Inspect exact change
       ↓
git diff if needed
```

This is a very useful debugging workflow.

---

# 27. Undo a Previous Change

Developers make mistakes.

Examples:

```text
Wrong code
Wrong commit message
Unwanted change
Bad configuration
Incorrect refactor
```

Git provides multiple ways to fix mistakes.

But important warning:

> Some commands modify commit history and therefore can affect collaborators.

---

# 28. Safe Rule from the Source

Source gives a practical rule:

> If a commit has already been pushed to the remote, generally prefer `git revert` to undo it.

Why?

Because:

```text
revert
=
new commit

reset/amend
=
history modification
```

---

# 29. `git revert`

Command:

```bash
git revert <commit>
```

`git revert` creates a **new commit** whose changes are the functional opposite of the commit being undone.

Example:

```text
A
↓
B  ← bad change
↓
C  ← revert B
```

History remains visible.

---

# 30. Example of `git revert`

Suppose commit B:

```text
Change all Heading 3 → Heading 5
```

If you run:

```bash
git revert B
```

Git creates a new commit that effectively changes:

```text
Heading 5 → Heading 3
```

without deleting commit B from history.

---

# 31. Why `git revert` Is Safer for Shared History

History:

```text
A
|
B
|
C
|
D
```

Suppose B is wrong.

Revert:

```text
A
|
B
|
C
|
D
|
E = revert B
```

Everyone can see:

```text
B happened
+
B was later reverted
```

No rewriting of already shared history is required.

---

# 32. `git revert` Can Target Older Commits

Source says `git revert` can be used on commits at any point in repository history without affecting other work.

Example:

```bash
git revert <old-commit>
```

Git applies the inverse change as a new commit.

---

# 33. Important `git revert` Limitation

Source explicitly notes:

> `git revert` cannot itself resolve conflicts.

If newer changes conflict with the inverse change being applied, Git may stop and ask you to resolve the conflict like a merge conflict.

So:

```text
git revert
   ↓
Potential conflict
   ↓
Manual resolution
   ↓
Continue
```

---

# 34. Why Revert Is Useful

Use case:

```text
Production bug
```

Suppose bad commit:

```text
abc123
```

was already deployed/shared.

A typical safe approach is:

```bash
git revert abc123
```

rather than rewriting public history.

---

# 35. Amending Commits

Source then discusses:

```bash
git commit --amend
```

Commit messages are important for project history.

But sometimes:

```text
Typo
```

or a missing staged file happens.

---

# 36. `git commit --amend`

Command:

```bash
git commit --amend
```

can modify the **most recent commit**.

It can be used to:

- Fix the latest commit message.
- Include additional files in the latest commit.
- Modify the most recent commit.

---

# 37. Amend Commit Message

Suppose:

```text
git commit -m "Fix acount bug"
```

Typo:

```text
acount
```

Instead of creating another useless commit:

```text
Fix typo
```

you can amend the latest commit.

Example:

```bash
git commit --amend -m "Fix account bug"
```

---

# 38. Add Missing File with Amend

Suppose:

```bash
git commit -m "Add reservation service"
```

but forgot:

```text
ReservationServiceTest.cls
```

You can:

```bash
git add ReservationServiceTest.cls
git commit --amend
```

Now the latest commit includes the staged test file.

---

# 39. Amend Warning

Source explicitly warns:

> `git commit --amend` alters commit history.

Therefore avoid using it casually after pushing to remote.

Why?

Suppose:

```text
Remote:
A → B

Your local:
A → B'
```

Now local history no longer matches the shared history.

This can create collaboration problems.

---

# 40. Simple Rule

```text
Not pushed?
    ↓
amend can be appropriate

Already pushed/shared?
    ↓
avoid history rewrite unless team explicitly coordinates it
```

---

# 41. Rewind to an Earlier Point in History

Developers experiment.

Sometimes:

```text
Idea
 ↓
Implementation
 ↓
More commits
 ↓
"Oh, this path is wrong."
```

Git provides:

```bash
git reset
```

to rewind history.

---

# 42. `git reset`

Important warning:

`git reset` changes commit history.

Therefore source strongly recommends using it when commits have **not been pushed** to the remote branch.

Three main forms:

```text
git reset --soft
git reset --mixed
git reset --hard
```

---

# 43. `git reset --soft`

Command:

```bash
git reset --soft <commit>
```

The selected commit(s) are removed from the branch history, but their changes remain in the **staging area**.

Use case:

```text
Multiple small commits
        ↓
soft reset
        ↓
changes staged
        ↓
create one clean commit
```

---

# 44. Soft Reset Example

History:

```text
A
↓
B
↓
C
```

Run:

```bash
git reset --soft HEAD~2
```

Branch moves back by two commits.

The changes introduced by those commits remain staged.

Then:

```bash
git commit -m "Combine changes"
```

can create a cleaner single commit.

---

# 45. `git reset --mixed`

Command:

```bash
git reset --mixed <commit>
```

This is the default reset mode.

The selected commit(s)' changes are moved into the **working directory**, rather than remaining staged.

Concept:

```text
History rewound
      ↓
Changes remain
      ↓
Working directory
```

---

# 46. Why Use Mixed Reset?

Useful when you want to:

```text
Combine commits
+
Modify files
+
Re-stage selectively
+
Create a new clean history
```

Example:

```bash
git reset HEAD~2
```

Because `--mixed` is default.

---

# 47. `git reset --hard`

Command:

```bash
git reset --hard <commit>
```

This is dangerous.

Source says it will:

- Remove the identified commits from the branch history.
- Destroy their changes from the repository state.
- Delete uncommitted working-directory changes.
- Delete staged changes.
- Potentially cause irreversible loss of work.

Conceptually:

```text
History
+
Staging
+
Working Tree changes
```

can be discarded.

---

# 48. Why `git reset --hard` Is Dangerous

Example:

```text
You modified 3 files
but didn't commit.

Then:

git reset --hard HEAD~1
```

Those uncommitted changes can be lost.

There is no normal:

```text
Recycle Bin
```

for this.

So:

> **Use `--hard` only when you are absolutely sure the changes can be discarded.**

---

# 49. Reset Comparison

| Command | History | Changes | Staging |
|---|---|---|---|
| `reset --soft` | Rewinds | Preserved | Staged |
| `reset --mixed` | Rewinds | Preserved | Unstaged |
| `reset --hard` | Rewinds | Discarded | Discarded |

Mental trick:

```text
SOFT  → Keep staged
MIXED → Keep files, unstage
HARD  → Throw changes away
```

---

# 50. Source Reset Example

The source gives:

```bash
git reset --soft HEAD~2
```

Meaning:

```text
Current branch
     ↓
move back 2 commits
     ↓
keep those changes staged
```

`HEAD` points to the tip/current commit of your branch.

---

# 51. What Is HEAD?

Simple:

```text
HEAD
 ↓
Current checked-out commit/branch position
```

Example:

```text
A → B → C
        ↑
       HEAD
```

If you reset two commits:

```text
A → B → C
↑
HEAD
```

depending on command/target, the branch pointer moves.

---

# 52. Git Merge Strategies

Source discusses two primary merge methods:

```text
Recursive Merge
Fast-Forward Merge
```

The important point is not that one is universally right.

Instead:

> Understand how each strategy affects history.

---

# 53. Recursive Merge

A recursive merge occurs when the branch you are merging into has newer changes than the base from which your feature branch was created.

Example:

```text
main:
A → B → C

feature:
     \
      D → E
```

Meanwhile main receives:

```text
F
```

Now:

```text
A → B → C → F
          \
           D → E
```

Feature is behind current main.

---

# 54. Recursive Merge — What Happens?

When PR is merged:

```text
main current state
        +
feature changes
        ↓
merge commit
```

A new merge commit is created.

Conceptually:

```text
        D---E
       /     \
A---B---C---F---M
```

`M` combines the two histories.

---

# 55. Why Merge Commit Exists

Because both lines of development contain unique commits.

Git needs a new commit that has both histories as parents.

Conceptually:

```text
        D---E
       /     \
      C-------M
       \     /
        F---
```

Exact graph depends on history, but core idea:

```text
Two lines
   ↓
Merge commit
```

---

# 56. Fast-Forward Merge

Fast-forward merge occurs when the target/original branch has not received additional commits since the feature branch was created.

Example:

```text
A → B → C
         \
          D → E
```

and main is still at:

```text
C
```

Feature contains:

```text
D
E
```

So main can simply move forward:

```text
A → B → C → D → E
```

No merge commit required.

---

# 57. Why It Is Called Fast-Forward

Because the branch pointer simply moves forward.

Before:

```text
main → C
feature → E
```

After:

```text
main → E
feature → E
```

The main pointer "fast-forwards" to the feature tip.

---

# 58. Fast-Forward Does Not Create Merge Commit

Important:

```text
Fast-forward
=
No additional merge commit
```

History stays linear.

---

# 59. Recursive vs Fast-Forward

| Scenario | Result |
|---|---|
| Target branch has new commits | Merge commit may be created |
| Target branch has no new commits | Fast-forward possible |
| Fast-forward | No merge commit |
| Recursive merge | Merge commit |

---

# 60. Important Terminology Note

The source calls the non-fast-forward strategy:

```text
Recursive Merge
```

This is the terminology used in the source material.

### Gyaan / Modern Git note

Modern Git versions have evolved merge implementation terminology; for current Git usage, you may encounter the `ort` merge strategy/algorithm. The architectural idea you should retain from this unit is:

```text
Fast-forward
vs
Non-fast-forward merge
```

and how each affects history.

---

# 61. Turn Recursive Merge into Fast-Forward Merge

Source introduces:

```bash
git rebase
```

as a powerful command.

One popular use:

> Rebase your feature branch so that a merge can become fast-forward where appropriate.

---

# 62. What Does Rebase Do?

Suppose:

```text
main:
A → B → C → F

feature:
A → B → C → D → E
```

Feature started at C.

Main moved to F.

Rebase can take:

```text
D
E
```

and reapply them after:

```text
F
```

Conceptually:

```text
main:
A → B → C → F

feature after rebase:
A → B → C → F → D' → E'
```

The feature commits get new identities because their ancestry changed.

---

# 63. Rebase = Replay Commits

Very useful mental model:

```text
rebase
=
Take my commits
+
move base
+
replay my commits on new base
```

Example:

```text
Old:

A---B---C---D---E
     \
      F---G

New base = E

After rebase:

A---B---C---D---E---F'---G'
```

---

# 64. Why Rebase Can Enable Fast-Forward

Before rebase:

```text
main:
A---B---C---D

feature:
     \
      E---F
```

Target has diverged.

After rebase:

```text
A---B---C---D---E'---F'
```

Now main can potentially:

```text
fast-forward
```

to `F'`.

---

# 65. Rebase Rewrites History

This is extremely important.

Original:

```text
E
F
```

After rebase:

```text
E'
F'
```

Even if changes are logically identical, commit IDs can change because commit ancestry/metadata changes.

Therefore:

> Rebase is a history-rewriting operation.

---

# 66. Rebase Warning

Do not casually rebase commits that other developers are already using/shared.

Why?

Because:

```text
Shared history
        ↓
rebase
        ↓
new commit identities
        ↓
collaboration confusion
```

Use team policy.

---

# 67. Interactive Rebase

Source mentions:

```bash
git rebase -i
```

Interactive rebase can modify how recent commit history looks.

It can allow you to:

- Edit commit messages
- Squash commits
- Combine commits
- Rearrange commits

---

# 68. Interactive Rebase Example

Suppose:

```text
A
↓
B "add feature"
↓
C "fix typo"
↓
D "fix test"
```

You may want:

```text
A
↓
B' "add feature"
```

where B' includes the relevant corrections.

Interactive rebase can help clean up local history before sharing, according to team policy.

---

# 69. Rebase vs Merge Mental Model

### Merge

```text
Keep divergent history
        ↓
Combine histories
        ↓
May create merge commit
```

### Rebase

```text
Move base
        ↓
Replay commits
        ↓
Rewrite commit ancestry
        ↓
Can create linear history
```

---

# 70. When to Think About Rebase

Use cases can include:

```text
Local feature branch cleanup
Updating feature branch onto latest main
Preparing cleaner history
Enabling fast-forward integration
```

But exact team workflow matters.

---

# 71. `rebase` vs `reset`

These are very different.

### Reset

Moves branch/HEAD history pointer:

```text
A → B → C
      ↑
   reset here
```

### Rebase

Replays commits on a new base:

```text
Old base
   ↓
New base
   ↓
Replay feature commits
```

---

# 72. `revert` vs `reset` vs `amend` vs `rebase`

| Command | Main purpose | Rewrites history? | Typical shared-history safety |
|---|---|---:|---|
| `git revert` | Undo a commit with a new commit | No | Safer |
| `git commit --amend` | Modify latest commit | Yes | Avoid after push |
| `git reset` | Move branch history pointer | Yes | Usually local/unpushed |
| `git rebase` | Replay commits on new base | Yes | Avoid casually on shared history |

This table is one of the most important takeaways of the unit.

---

# 73. Practical Decision Tree

```text
Need to undo a pushed/shared commit?
        |
        +-- YES → git revert
        |
        +-- NO
             |
             +-- Need to fix latest commit?
             |       ↓
             |   git commit --amend
             |
             +-- Need to rewind local history?
             |       ↓
             |   git reset
             |
             +-- Need to move feature onto newer base?
                     ↓
                  git rebase
```

---

# 74. Development Example — Bad Commit Already in Remote

Scenario:

```text
main
 ↓
bad commit
 ↓
already pushed
 ↓
other developers pulled it
```

Don't casually:

```bash
git reset --hard
```

and force history replacement.

Safer conceptual solution:

```bash
git revert <bad-commit>
```

Then:

```text
New commit
=
undo bad change
```

---

# 75. Development Example — Local Commit Message Typo

Scenario:

```text
commit not pushed
```

Command:

```bash
git commit --amend -m "Correct commit message"
```

Good use case for amend.

---

# 76. Development Example — Three Messy Local Commits

History:

```text
A
↓
B fix
↓
C typo
↓
D another fix
```

You want one clean local commit.

Possible approach:

```bash
git reset --soft HEAD~3
git commit -m "Implement reservation validation"
```

This is conceptually aligned with the source's soft-reset example.

---

# 77. Development Example — Feature Behind Main

```text
main:
A-B-C-D-E

feature:
A-B-C-X-Y
```

You can consider:

```bash
git rebase main
```

Conceptually:

```text
A-B-C-D-E-X'-Y'
```

Then, depending on repository policy, integration can be fast-forward.

---

# 78. Errors & Gotchas

## Gotcha 1 — `reset --hard` on uncommitted work

Danger:

```bash
git reset --hard
```

can delete uncommitted changes.

---

## Gotcha 2 — Amend after push

If others already pulled the commit, amending rewrites history they already know.

---

## Gotcha 3 — Rebase shared branch

Rebase changes commit identities.

Don't casually rewrite shared history.

---

## Gotcha 4 — Revert doesn't erase history

`git revert` creates a new commit.

Old commit remains in history.

---

## Gotcha 5 — Revert can conflict

A revert may require manual conflict resolution.

---

## Gotcha 6 — `git diff` vs staged diff

```bash
git diff
```

and:

```bash
git diff --staged
```

answer different questions.

---

## Gotcha 7 — Short SHA collision

Git generally accepts abbreviated hashes when they uniquely identify an object. If an abbreviation becomes ambiguous, use more characters/full SHA.

---

# 79. Limits / Constraints

- Git history operations can be powerful but destructive.
- `reset --hard` can cause loss of uncommitted work.
- Amend changes the latest commit identity.
- Rebase rewrites commit ancestry.
- Revert can create conflicts if later changes overlap.
- Merge strategy affects history presentation but does not replace functional testing.
- Git history tells you what was committed, not whether the application is business-correct.
- A clean graph does not guarantee clean Salesforce deployment.
- Large repositories/history may require additional tooling for efficient analysis.

---

# 80. Best Practices

## 1. Before destructive commands

Check:

```bash
git status
git log --oneline --graph --decorate -10
```

---

## 2. Before resetting

Know exactly:

```text
Which commit?
Which changes?
Do I need them?
Are they pushed?
```

---

## 3. Before rebase

Ask:

```text
Is this branch shared?
Has someone else based work on it?
```

---

## 4. Before revert

Understand:

```text
What commit am I undoing?
What later changes depend on it?
```

---

## 5. Before PR

Use:

```bash
git diff
git diff --staged
git log --oneline --graph --decorate
```

to inspect your work.

---

# 81. Salesforce Development Application

Git history becomes especially important in Salesforce because a single business feature can span:

```text
Apex
LWC
Flows
Objects
Fields
Permission Sets
Custom Metadata
Custom Labels
Layouts
FlexiPages
Integration configuration
Tests
```

So when something breaks, architect/developer may need to answer:

```text
What changed?
When?
Who changed it?
Which files?
Which commit?
Which release?
```

Git history provides the source-control side of these answers.

---

# 82. Salesforce Debugging Flow

Example:

```text
Production issue
      ↓
Identify affected functionality
      ↓
git log
      ↓
Find recent relevant commit
      ↓
git show <SHA>
      ↓
Inspect changed metadata/code
      ↓
git diff against previous release/tag
      ↓
Determine fix
      ↓
Revert / fix-forward
      ↓
Test
      ↓
Deploy
```

---

# 83. Salesforce `git diff` Example

Suppose releases:

```text
v1.4
v1.5
```

You can inspect release differences conceptually:

```bash
git diff v1.4 v1.5
```

Useful for release impact analysis.

---

# 84. Salesforce Tags and Release History

A team may tag releases:

```text
v1.0
v1.1
v1.2
```

Then:

```bash
git log --oneline --decorate
```

can help visualize release points.

And:

```bash
git diff v1.1 v1.2
```

can help understand what changed between releases.

---

# 85. Salesforce Revert vs Deployment Rollback

Important distinction:

```text
Git revert
```

changes source history.

It does not automatically mean:

```text
Production Salesforce org is instantly rolled back.
```

A Salesforce deployment pipeline must actually deploy the reverted source/configuration.

So:

```text
Git revert
   ↓
New source state
   ↓
CI/CD validation
   ↓
Salesforce deployment
   ↓
Production state updated
```

---

# 86. Salesforce Architect Insight

Source control and deployment are related but different concerns.

```text
Git
=
Version history + collaboration

CI/CD
=
Validation + automation

Salesforce deployment
=
Org metadata state change
```

Architect should design all three together.

---

# 87. Advanced — Fix Forward vs Revert

Production issue ke time teams often consider:

### Revert

```text
Undo previous change
```

### Fix Forward

```text
Create a new corrective change
```

Choice depends on:

```text
Severity
Release state
Dependencies
Risk
Rollback feasibility
Business impact
```

Do not assume one approach is universally correct.

---

# 88. Advanced — Revert Is a New Change

Suppose:

```text
A = Add feature
B = Add feature tests
C = Bug fix
```

Revert A does not magically remove history.

It creates:

```text
D = Revert A
```

Therefore history tells the complete story.

---

# 89. Advanced — Why History Matters for Architects

Architect can inspect:

```text
Change frequency
Hot areas
Repeated fixes
Large commits
Release patterns
```

This can help identify:

```text
Technical debt
High-change components
Risky areas
Ownership gaps
```

This is an analytical use of history, not just debugging.

---

# 90. Advanced — Commit Quality

A high-quality commit message should communicate:

```text
What changed
```

and ideally provide enough context to understand:

```text
Why
```

Examples:

### Weak

```text
update
```

### Better

```text
Fix reservation validation for cancelled bookings
```

---

# 91. Advanced — History as Documentation

Good history can answer:

```text
Why was this changed?
What was changed?
When?
Which release?
```

This makes Git history a lightweight technical record.

---

# 92. Super Advanced — DAG Mental Model

Git history is not fundamentally just a straight list.

It is a:

```text
Directed Acyclic Graph
(DAG)
```

Commits point to parent commits.

Example:

```text
       B---C
      /
A----D
      \
       E---F
```

Branches are references/pointers to commits.

---

# 93. Branch Is Not a Folder

Important:

```text
branch
≠
copy of repository
```

A branch is essentially a movable reference to a commit.

Example:

```text
main → C
feature → F
```

Both references point into the commit graph.

---

# 94. HEAD + Branch Mental Model

Example:

```text
A---B---C---D
        ↑
      main
```

Checkout feature:

```text
A---B---C---D
        ↑
      main
        \
         E---F
             ↑
          feature
             ↑
            HEAD
```

This makes:

```text
checkout
branch
merge
rebase
reset
```

much easier to understand.

---

# 95. Super Advanced — Why Rebase Changes SHA

A commit identity depends on information including its parent.

Suppose:

```text
D
```

has parent:

```text
C
```

After rebase:

```text
D'
```

has parent:

```text
F
```

Because parent relationship changes, commit identity changes.

Hence:

```text
D ≠ D'
```

even if the code change is logically similar.

---

# 96. Super Advanced — Merge vs Rebase as History Design

### Merge

Preserves the fact that two development lines existed.

```text
       feature
      /      \
main -------- M
```

### Rebase

Presents the feature commits as if they were applied after the new base.

```text
main → base → feature-1' → feature-2'
```

So choice can be about:

```text
History fidelity
vs
Linear readability
```

according to team policy.

---

# 97. Super Advanced — Rebase and Conflict Resolution

Rebase can itself encounter conflicts.

Conceptually:

```text
git rebase main
      ↓
Conflict
      ↓
resolve
      ↓
git add
      ↓
git rebase --continue
```

If you decide to abandon:

```bash
git rebase --abort
```

> These are practical Git commands added as enhancement; the source unit specifically introduces rebase as a history-rewriting tool for replaying commits and creating a fast-forward integration.

---

# 98. Super Advanced — Reset and Reflog

Added development knowledge:

If you accidentally move a local branch with reset/rebase, Git often maintains a reflog that can help locate previous reference positions.

Common command:

```bash
git reflog
```

This is **not a guarantee against data loss**, especially after cleanup/garbage collection, so it should not be treated as a replacement for backups or disciplined workflows.

---

# 99. Super Advanced — Shared History Rule

One of the most useful rules:

```text
Public/shared history
    ↓
Prefer additive changes
    ↓
revert / new commit
```

Private/local history:

```text
Can be cleaned more aggressively
    ↓
amend
reset
rebase
```

subject to team policy.

---

# 100. Interview Q&A

## Q1. How does Git store data?

**Answer:**

Git stores content as objects. File contents are represented as blobs, trees organize references to those objects, and commits reference trees and parent commits. This creates the commit graph/history.

---

## Q2. What is a SHA-1 hash used for?

**Answer:**

It identifies Git objects/content in the Git object model. Commit history can reference these identifiers.

---

## Q3. What is `git log`?

**Answer:**

It displays commit history for the current branch.

---

## Q4. Difference between `git log` and `git show`?

**Answer:**

`git log` provides history/commit listing, while `git show <SHA>` provides details of a specific commit and its changes.

---

## Q5. Difference between `git diff` and `git show`?

**Answer:**

`git diff` compares states/references and shows differences. `git show` inspects a specific commit and its metadata/content changes.

---

## Q6. How do you safely undo a pushed commit?

**Answer:**

Typically use:

```bash
git revert <commit>
```

because it creates a new inverse commit instead of rewriting shared history.

---

## Q7. What does `git commit --amend` do?

**Answer:**

It modifies the most recent commit, such as correcting its message or adding staged changes. It rewrites that commit, so it should generally be avoided after the commit has been pushed/shared.

---

## Q8. Difference between soft, mixed and hard reset?

**Answer:**

```text
soft  → changes remain staged
mixed → changes remain in working tree
hard  → changes are discarded
```

---

## Q9. What is the biggest risk of `git reset --hard`?

**Answer:**

It can destroy uncommitted changes and remove commits from the current branch history.

---

## Q10. What is a fast-forward merge?

**Answer:**

When the target branch has no new commits since the feature branch was created, Git can simply move the target branch pointer forward without creating a merge commit.

---

## Q11. What is a recursive/non-fast-forward merge?

**Answer:**

When both the target and feature histories have diverged, Git can combine them through a merge commit.

---

## Q12. What does rebase do?

**Answer:**

It takes commits from a branch and reapplies them on top of another base, rewriting their ancestry. This can create a linear history and can allow fast-forward integration.

---

## Q13. Does rebase change commit IDs?

**Answer:**

Usually yes, because the parent relationship/commit ancestry changes.

---

## Q14. When should you avoid rebase?

**Answer:**

Avoid casually rebasing shared/public history because it rewrites commit identities and can disrupt collaborators.

---

# 101. Salesforce Architect Interview Q&A

## Q15. Why does Git history matter in Salesforce architecture?

**Answer:**

Salesforce features span many metadata and code components. Git history lets teams trace changes across Apex, LWC, metadata, tests, permissions and configuration, helping with debugging, releases and governance.

---

## Q16. Can `git revert` roll back Salesforce production automatically?

**Answer:**

No. `git revert` changes the source repository by creating an inverse commit. The resulting source/configuration still needs to go through the Salesforce deployment process.

---

## Q17. Would you use `git reset --hard` on production branch?

**Answer:**

Not as a casual rollback mechanism for shared production history. For shared history, an additive corrective commit such as `git revert`, followed by controlled deployment, is generally safer.

---

## Q18. Why does merge strategy matter to an architect?

**Answer:**

It affects commit history, traceability, release workflow, collaboration and how developers integrate changes. The strategy should fit the organization's delivery model.

---

# 102. Real-World Salesforce Release Example

Imagine:

```text
Release v2.0
```

contains:

```text
Apex
LWC
Flow
Permission Set
Custom Metadata
```

After production deployment, a bug is discovered.

Investigation:

```text
git log --oneline --decorate
```

Find relevant commit:

```text
abc123 Fix reservation validation
```

Inspect:

```bash
git show abc123
```

Compare releases:

```bash
git diff v1.9 v2.0
```

Then decide:

```text
Revert?
or
Fix-forward?
```

After correction:

```text
CI
 ↓
Salesforce validation
 ↓
Tests
 ↓
UAT if required
 ↓
Production
```

---

# 103. Practical Git History Investigation

When someone asks:

> "What changed between release 1.2 and 1.3?"

Start:

```bash
git diff v1.2 v1.3
```

Then:

```bash
git log --oneline --decorate
```

Find important commits.

Then:

```bash
git show <SHA>
```

This is a strong practical workflow.

---

# 104. Developer Daily Commands

### Check state

```bash
git status
```

### View recent history

```bash
git log --oneline -10
```

### Visualize history

```bash
git log --oneline --graph --decorate -20
```

### See unstaged changes

```bash
git diff
```

### See staged changes

```bash
git diff --staged
```

### Inspect commit

```bash
git show <SHA>
```

---

# 105. Undo Commands Quick Reference

### Undo shared commit

```bash
git revert <SHA>
```

### Fix latest local commit

```bash
git commit --amend
```

### Rewind while keeping staged changes

```bash
git reset --soft HEAD~N
```

### Rewind while keeping changes unstaged

```bash
git reset --mixed HEAD~N
```

### Discard changes — dangerous

```bash
git reset --hard HEAD~N
```

### Move feature commits onto new base

```bash
git rebase main
```

---

# 106. Merge Strategy Quick Reference

```text
Target has no new commits
        ↓
Fast-forward possible
        ↓
No merge commit
```

versus:

```text
Target has new commits
        ↓
Histories diverged
        ↓
Non-fast-forward merge
        ↓
Merge commit may be created
```

Rebase:

```text
Feature commits
      ↓
Move/replay onto latest target
      ↓
Linear history possible
      ↓
Fast-forward integration possible
```

---

# 107. Final Cheat Sheet

## History

```bash
git log
git log -10
git log --oneline
git log --oneline --graph
git log --oneline --graph --decorate
```

## Compare

```bash
git diff
git diff --staged
git diff <commit1> <commit2>
git diff <branch1> <branch2>
git diff <tag1> <tag2>
```

## Inspect

```bash
git show <SHA>
```

## Undo

```bash
git revert <SHA>
git commit --amend
git reset --soft HEAD~N
git reset --mixed HEAD~N
git reset --hard HEAD~N
```

## Rebase

```bash
git rebase main
git rebase -i HEAD~N
```

---

# 108. Decision Matrix

| Situation | Recommended concept |
|---|---|
| Need to inspect history | `git log` |
| Need compact history | `git log --oneline` |
| Need branch graph | `git log --graph` |
| Need exact commit details | `git show` |
| Need compare changes | `git diff` |
| Need undo pushed commit | `git revert` |
| Need fix latest local commit | `git commit --amend` |
| Need combine local commits | `git reset --soft` |
| Need rewind but keep files unstaged | `git reset --mixed` |
| Need discard local changes | `git reset --hard` |
| Need move feature onto new base | `git rebase` |
| Need clean local commit history | interactive rebase |

---

# 109. Final Mental Model

Remember:

```text
Git Object Model
      ↓
Blob
      ↓
Tree
      ↓
Commit
      ↓
Parent
      ↓
Commit Graph
```

Then:

```text
git log
      ↓
Understand history

git diff
      ↓
Understand differences

git show
      ↓
Understand a specific commit

git revert
      ↓
Safely undo shared history

git amend
      ↓
Fix latest local commit

git reset
      ↓
Rewrite local history

git rebase
      ↓
Replay commits on another base
```

---

# 110. Architect Mental Model

The most important distinction is:

```text
REVERT
=
Undo by ADDING a new commit

RESET
=
Move history pointer

AMEND
=
Modify latest commit

REBASE
=
Rewrite/replay commit ancestry

MERGE
=
Combine development histories
```

And:

```text
Shared history
      ↓
Prefer safe/additive operations

Private history
      ↓
History cleanup is more flexible
```

---

# 111. Final Takeaways

### 1. Git is a graph, not just a list

Commits reference parents.

### 2. Hashes identify Git objects

The source explains SHA-1 and notes SHA-256 as the successor direction.

### 3. `git log` is your history window

Use modifiers to make history readable.

### 4. `git diff` tells you what changed

Use it before commits and during debugging.

### 5. `git show` tells you what a specific commit did

Very useful for production investigation.

### 6. `git revert` is the normal safe choice for shared/pushed history

It creates a new inverse commit.

### 7. `git reset` rewrites history

Be careful, especially with `--hard`.

### 8. `git amend` is for the latest commit

Prefer it before sharing the commit.

### 9. Merge strategies affect history

Fast-forward gives linear movement without a merge commit.

Non-fast-forward merging can create a merge commit.

### 10. Rebase replays commits on a new base

It can turn a divergent integration into a fast-forward scenario, but rewrites history.

---

# 112. Quiz

The source unit requires all quiz questions to be answered correctly.

## Question 1

### What command safely undoes a change at any point in a repository's history?

Options:

A. `git reset --soft`

B. `git commit --amend`

C. `git revert`

D. `git rebase`

### Correct Answer

**C — `git revert`**

### Why?

`git revert` creates a new commit that functionally undoes the target commit without rewriting the existing shared history.

---

# 113. Quiz Question 2

### How is rebase used in relation to a merge?

Options:

A. A rebase is the same as a merge.

B. A rebase lets you move from a fast-forward to a recursive merge scenario.

C. A rebase enables you to move from a recursive to a fast-forward merge scenario.

D. A rebase lets you select the merge strategy you would like to use.

### Correct Answer

**C — A rebase enables you to move from a recursive to a fast-forward merge scenario.**

### Why?

If your feature branch is behind the target branch:

```text
Target:
A-B-C-D

Feature:
A-B-C-X-Y
```

Rebase can replay:

```text
X-Y
```

after:

```text
D
```

resulting conceptually in:

```text
A-B-C-D-X'-Y'
```

The target can then potentially fast-forward to `Y'`.

---

# 114. Quiz Memory Trick

## Undo a shared commit

```text
REVERT
```

Think:

> **Revert = Reverse with a new commit**

## Rewrite latest local commit

```text
AMEND
```

Think:

> **Amend = Modify latest**

## Move branch history pointer

```text
RESET
```

Think:

> **Reset = Rewind**

## Move commits to a new base

```text
REBASE
```

Think:

> **Rebase = Replay**

---

# 115. Final Architect-Level Flow

```text
                   GIT HISTORY
                       |
          +------------+-------------+
          |            |             |
          ↓            ↓             ↓
        LOG           DIFF         SHOW
          |            |             |
          +------------+-------------+
                       |
                  INVESTIGATE
                       |
              +--------+--------+
              |                 |
              ↓                 ↓
        Need to undo?       Need new base?
              |                 |
        +-----+-----+           ↓
        |           |        REBASE
        ↓           ↓
   Shared?       Local?
      |             |
      ↓             ↓
   REVERT       AMEND/RESET
```

---

# 116. Salesforce Architect Final Flow

```text
Production Issue
      ↓
Identify release/tag
      ↓
git log
      ↓
Find commit
      ↓
git show
      ↓
git diff release tags
      ↓
Understand dependency impact
      ↓
Choose:
  Revert
  OR
  Fix Forward
      ↓
Update source
      ↓
CI/CD
      ↓
Salesforce Validation
      ↓
Apex/LWC Tests
      ↓
Security/Metadata Review
      ↓
UAT
      ↓
Production
```

---

# 117. One-Line Memory Formula

```text
LOG = WHERE DID WE GO?
DIFF = WHAT CHANGED?
SHOW = WHAT DID THIS COMMIT DO?
REVERT = UNDO SAFELY
AMEND = FIX LAST COMMIT
RESET = REWIND
MERGE = COMBINE HISTORIES
REBASE = MOVE + REPLAY COMMITS
```

---

# 118. Final Architect Conclusion

Git ko architect level par samajhne ka matlab commands ratna nahi hai.

Real understanding hai:

```text
How history is represented
        ↓
How changes are compared
        ↓
How mistakes are corrected
        ↓
How shared history is protected
        ↓
How branches are integrated
        ↓
How release history remains traceable
```

Salesforce mein:

```text
Git History
      +
Salesforce Metadata
      +
Apex/LWC
      +
CI/CD
      +
Deployment
      +
Testing
      +
Security
```

milkar ek reliable engineering delivery system banate hain.

**Golden rule:**

```text
Shared history → prefer safe/additive operations.

Local/private history → cleanup tools like amend/reset/rebase
can be used carefully according to team policy.
```

---


# Appendix A — Original Uploaded Unit (Verbatim)

> The complete original uploaded text is preserved below without intentionally removing any source content.

Work with Your History in Git
Learning Objectives
After completing this unit, you’ll be able to:

Describe how Git stores data and outline one practical application of this knowledge.
View your project history and changes with Git.
Use Git commands that let you undo previous changes.
Summarize how rebase is related to common merge strategies.
How Git Stores Data
When we previously discussed commits, we identified them as snapshots of your project. Each snapshot contains a lot of information. The files compressed in the snapshot are each given a unique SHA-1 hash—referred to as blobs. Those blobs are referenced by a tree, and that tree is referenced by the commit. Diagram of a git commit tree

As you make changes to your files and create new commits, Git identifies the changed files and applies a new SHA-1 hash to the file, while unchanged files retain their existing SHA-1 hash. As the SHA-1 hash for a file changes, it references the previous SHA-1 hash as the parent. In GitHub, you can see the first seven characters of the SHA-1 hash (A) on any commit.

Note
Although SHA-1 hashes are being used today, it's important to note that the Git project has decided to pick a new hashing algorithm moving forward. Git has chosen SHA-256 as the successor to SHA-1, and work is currently in progress towards this transition.

This is very similar to how your commit history operates. As you create new commits, they reference the previous commit as its parent. This reference point is very important. This linear, parent/child relationship creates a consistent history and is what enables Git to merge branches together.

Explore Your History with Git
While working on your project it can be helpful to review your commit history. On GitHub.com, you can access your project history by selecting the commit button from the code tab on your project. Locally, you can use git logCopy.

The git logCopy command enables you to display a list of all of the commits on your current branch. By default, the git logCopy command presents a lot of information all at once.

Use some of the git logCopy modifiers to cultivate an easy-to-read list that provides some valuable information.

git log -10Copy will only show the 10 most recent commits.
git log --onelineCopy is a great way to view commit history by displaying the first seven characters of the SHA-1 hash and commit message of the commits on the current branch.
git log --oneline --graphCopy presents commit history in a ASCII graph displaying the different branches in the repository and their commits.
git log --oneline --graph --decorateCopy displays the same ASCII graph that is displayed using the --graph modifier, but also includes the branch name(s) for the different commits being displayed.
Screenshot showing the output of the `git log --oneline --graph --decorate` command.

Compare Versions of Files
As you prepare to craft that perfect commit, viewing the differences between what is currently in your working directory and staging area helps you git addCopy the right files to your commit.

By default, the git diffCopy command helps you review the changes between the last commit of your project and the various states of your files (for example, those in the working directory or your staging area). 

Diagram of the git diff options for comparing the changes in the working directory, staging area, and history.

You can also use git diffCopy to compare between any two commits, branches, or tags in the repository. For example, to compare two commits with SHA-1 hash references 4e3dc9bCopy and 0cd75d4Copy, enter command: git diff 4e3dc9b 0cd75d4 Copy

Finally, if you would like to view the changes that were made in a previous commit, you can use the git show <SHA-1>Copy command to display the details of that specific commit. It includes things like commit author, time and date of the commit, and a list of the changes that were made to the various assets within the repository.

Screenshot showing the output of the `git show <SHA-1>` command.

Undo a Previous Change
Sometimes we make small mistakes (or big ones!). Thankfully, Git comes equipped with commands that allow us to fix our mistakes. But beware, some commands that help us fix mistakes destructively modify the commit ID. Since these commit IDs are immutable, you could potentially cause issues for other collaborators. As a general rule, you should only use git revertCopy if the commit has been pushed to the remote.

Undoing Changes
git revertCopy creates a new commit with changes that are the opposite of the commit that is functionally being 'undone'.

For example, imagine within the commit history of a repository, there was a commit that changed all of your Heading 3s to Heading 5s. You could use git revertCopy to change all of your headings back to Heading 3 instead of going back through every Heading 5 and changing them back to Heading 3s.

git revertCopy can be used on commits at any point in the repository’s history without affecting other work.

Note
You can’t use git revertCopy to resolve conflicts—if there are more recent conflicting changes, Git will ask you to resolve as if it’s a merge conflict.

Using git revertCopy is a safe way to undo a specific change, preventing any of the typical complications that occur when altering the commit history of a project.

Amending Commits
Earlier, we discussed git commitCopy and identified that commit messages are an important aspect of crafting a detailed project history.

Sometimes in the excitement of creating a commit, you might make a typo within the commit message. You can use git commit --amendCopy to make a modification to the last commit you made. This will alter the commit history of your project, so it is recommended that you don't use git commit --amendCopy if you have already pushed your commits to the remote.

You can also use git commit --amendCopy to rewrite the most recent commit to include files in your staging area.

Rewind to an Earlier Point in History
We know programmers love to experiment and try new things. However, sometimes during the course of that experimentation we realize we went down the wrong path and the commits we were making might not be as useful as we originally thought.

Git has a git resetCopy command that can help rewind the history of our project, but, it alters the commit history, which as mentioned before, might cause issues for other collaborators. It is highly recommended that you use git resetCopyonly when you have not pushed your commits to your remote branch. git resetCopy comes in three distinct flavors, --softCopy, --mixedCopy, and --hardCopy.

git reset --softCopy takes the identified commit(s) and places all of the changes in the staging area. This is helpful if you want to take a group of commits and squash them into a single larger commit.
git reset --mixedCopy, the default mode for git resetCopy, takes the identified commit(s) and places all of the changes in the working directory. Like --softCopy, this is helpful if you want to take a group of small commits and combine some of the changes to make larger commits. But you can also use it to make additional changes to the files and then re-create the commit history.
git reset --hardCopy will take the identified commit(s) and destroy them. Be careful with this, because they don’t go in your trash or recycle bin—the files essentially don't exist and are completely removed from your repository. Any uncommitted changes to files that are currently in the working directory or staging area will also be deleted. You can lose work with git reset --hardCopy.
An example of using reset could look something like this:

git reset --soft HEAD~2Copy would rewind the branch you are on by two commits (remember HEAD is a pointer to the tip of your branch). The changes that had been made in those last two commits would be reflected in the staging area.

Git Merge Strategies
When it comes to merging, there are two primary methods that Git uses to apply your changes to the main branch (or whatever branch you are merging into): Recursive Merge and Fast-Forward Merge.

There is nothing right or wrong about either merge strategy, but it is important to know how each affects how history will look. At first glance these might seem less like merge strategies, and more like this is just how git handles merges—but we talk through that after we discuss the two merges. 

Recursive Merge
A recursive merge occurs when your feature branch doesn’t have the latest version of code in the branch you’re trying to merge into (sometimes, but not always, main).

Diagram of a recursive merge.

When you create a feature branch, you’re basing it off the original branch at its current state. As you make changes to your branch, other collaborators might be merging their own changes into the original branch.

When you create a pull request and merge your changes, a merge commit is created. This takes the changes you made to your branch and the current state of the branch you’re merging into and creates a new commit that combines those changes.

Fast-Forward Merge
A fast-forward merge occurs when there have been no new commits, other than the ones you’re trying to merge, on the original branch since you created your feature branch from it.

Diagram of a fast-forward merge.

Since the original branch doesn't have any changes, the tip of the branch is simply fast forwarded to include the changes on your branch. With a fast-forward merge, Git does not create a new merge commit.

Turn a Recursive Merge into a Fast-Forward Merge
The git rebaseCopy command is powerful and can do a lot of cool things in your repository (all of which rewrite your history, so use it with care). One of the most popular uses of git rebaseCopy is to create a fast-forward merge, when Git would have defaulted to a recursive merge. 

Remember, a recursive merge happens when the branch you’re merging into has new changes since you created your branch. Rebase picks up the commit you made on your branch and reapplies them after the last commit on the branch you select. You can also use git rebaseCopy to modify the way your commit history looks. Using the interactive modifier, -iCopy with rebase allows you to edit previous commit messages, squash commits into a larger commit or commits, and rearrange your commit history.

Resources
Visualization tool for Git
Learn Git Branching
Quiz
To complete this unit, you need to answer all the quiz questions correctly.
+100 Points

1
What command safely undoes a change at any point in a repository's history?

A
git reset --soft

B
git commit --amend

C
git revert

D
git rebase

2
How is rebase used in relation to a merge?

A
A rebase is the same as a merge.

B
A rebase lets you move from a fast-forward to a recursive merge scenario.

C
A rebase enables you to move from a recursive to a fast-forward merge scenario.

D
A rebase lets you select the merge strategy you would like to use.
