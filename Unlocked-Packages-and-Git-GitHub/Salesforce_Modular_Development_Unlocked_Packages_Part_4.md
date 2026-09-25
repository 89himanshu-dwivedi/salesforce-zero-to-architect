# Working with Modular Development and Unlocked Packages — Part 4
## Git Branching Strategy • CI/CD • Unlocked Package Versioning • Salesforce DX • Release Management

> **Source:** Salesforce Developer Blog — *Working with Modular Development and Unlocked Packages: Part 4*  
> **Author:** René Winkelmeyer  
> **Publication date:** June 26, 2018
>
> **Important:** This document preserves the source article's content, terminology, branch names, workflow decisions, commands, examples, warnings, and reasoning. Additional Architect / Advanced / Interview / Real-world guidance is clearly marked as an extension.
>
> **Historical note:** The article describes Salesforce DX / unlocked-package behavior and `sfdx force:*` commands as they existed in 2018. For a current project, verify modern `sf` CLI syntax, package behavior, limits, and supported metadata types in current Salesforce documentation.

---

# 1. Part 4 — What Are We Learning?

This is the fourth installment in the series about:

- Working with applications in modular pieces
- Incorporating packages into the application development lifecycle
- Understanding what packaging means for team change management
- Understanding what packaging means for release processes

The series covers:

```text
Part 1
What is a package?
How can you experiment with segmenting an org?

        ↓

Part 2
How do you organize metadata from an app/org into packages?
How do you organize metadata and projects in source control?

        ↓

Part 3
What do these changes mean for app-builder workflows?
What happens if an unlocked package is installed into Production?

        ↓

Part 4
How do you define a successful Git branching strategy?
How, when, and where should packaging be added to Continuous Deployment?
```

Part 4's main focus is:

```text
Git Branching Strategy
        +
Package Version Management
        +
Continuous Integration
        +
Continuous Deployment
        +
Environment Promotion
```

The source article explicitly frames Part 4 around creating a Git branching strategy and deciding how packaging should fit into continuous deployment. fileciteturn2file0L11-L17

---

# 2. Part 4 Ka Core Idea

Part 3 mein humne seekha:

```text
Module
   ↓
Package
   ↓
Package Version
   ↓
Dependencies
   ↓
Validation
   ↓
Promotion
   ↓
Production
```

Part 4 asks:

> **Ab Git aur CI/CD ke saath is package lifecycle ko systematically kaise manage karein?**

So the overall evolution is:

```text
Metadata
   ↓
Modules
   ↓
Packages
   ↓
Package Versions
   ↓
Git Branching
   ↓
CI
   ↓
Sandbox Validation
   ↓
UAT / Full Sandbox
   ↓
Production
```

---

# 3. Part 3 Se Part 4 Tak Transition

Part 3 mein:

```text
Packages + dependencies + versions
```

established ho gaye.

Ab problem hai:

```text
Developer changes code
        ↓
Multiple branches
        ↓
Multiple package versions
        ↓
Multiple environments
        ↓
Which version goes where?
```

Part 4 ka answer:

```text
Branching Strategy
+
CI/CD Strategy
+
Package Version Strategy
```

---

# 4. Why Git Strategy Matters

Source article says that after the application has been untangled into modules and package directories have been set up, the next step is managing:

- package versions,
- active development projects,
- source control.

A good and practical Git strategy is crucial for:

```text
Continuous Integration
+
Continuous Deployment
```

The article explicitly calls the Git strategy crucial to CI/CD workflows. fileciteturn2file0L17-L17

---

# 5. Git Branching — Basic Idea

Salesforce developers may already know source control and Git.

The article introduces a widely adapted Git branching strategy.

At a high level:

```text
master
   ↑
develop
   ↑
feature / hotfix
```

---

# 6. Core Git Branching Rules

The source gives four core rules.

## Rule 1 — Active Development

All active development work happens in:

```text
develop
```

---

## Rule 2 — Feature and Hotfix Branches

New:

```text
Features
```

and:

```text
Bug fixes
```

are developed in dedicated branches.

These branches inherit from:

```text
develop
```

Examples:

```text
feature/customer-search
feature/reservation-ui
hotfix/payment-error
```

---

## Rule 3 — Merge Tested Work Back to Develop

After a feature or bug fix has been successfully tested:

```text
feature branch
       ↓
develop
```

The code gets merged back into the development branch.

---

## Rule 4 — Production Code Goes to Master

Code shipped to Production gets merged:

```text
develop
   ↓
master
```

And because versioning is important:

```text
master
   +
tag
```

The source explicitly describes `master` being tagged with a version number. fileciteturn2file0L26-L32

---

# 7. Basic Branching Diagram

```text
                 feature branch
                /
develop --------+
                \
                 feature branch
                      |
                      v
                    develop
                      |
                      v
                    master
                      |
                      +--> tag 0.1
                      +--> tag 0.2
                      +--> tag 1.0
```

---

# 8. Why It Gets Complicated

The basic model looks simple.

But real software teams often have:

```text
Feature A
Feature B
Feature C
Hotfix
Release preparation
```

running simultaneously.

So you may have:

```text
                    feature-A
                   /
develop ----------+-------- feature-B
                   \
                    feature-C

master <----------- hotfix
```

This creates coordination challenges.

The article explicitly notes that multiple simultaneous branches can make the strategy more complicated. fileciteturn2file0L31-L32

---

# 9. Understanding the Branching Image

The provided Git-flow image shows:

```text
feature branches
       |
       v
develop
       |
       v
release branches
       |
       v
master

hotfixes
   |
   +------> master
   |
   +------> develop
```

The image also shows tags such as:

```text
Tag 0.1
Tag 0.2
Tag 1.0
```

and demonstrates that:

```text
feature development
        ↓
develop
        ↓
release
        ↓
master
```

while:

```text
hotfix
```

can be merged into Production-related history and then back into development.

---

# 10. Feature Branches

Feature branches are temporary development branches.

Example:

```text
feature/reservation-search
```

Purpose:

```text
Develop one feature
       ↓
Test feature
       ↓
Pull Request
       ↓
Merge into develop
```

---

# 11. Hotfix Branches

Hotfix branches are used for severe Production issues.

Example:

```text
hotfix/payment-failure
```

Conceptually:

```text
master
  |
  +--> hotfix
          |
          +--> master
          |
          +--> develop
```

Why merge back into `develop`?

Because otherwise the Production fix may exist only in Production history and get lost from future development.

---

# 12. Release Branches

The image also demonstrates:

```text
release branches
```

A release branch is used when a set of changes is being prepared for a particular release.

Example:

```text
develop
   |
   +--> release/1.0
```

The release branch can stabilize the version while development continues.

---

# 13. Tags

The image shows tags:

```text
0.1
0.2
1.0
```

A tag identifies a specific point in Git history.

Conceptually:

```text
master
  |
  +--> commit
  |
  +--> commit
  |
  +--> TAG 1.0
```

This gives you a historical reference for what was released.

---

# 14. Why Tags Matter for Salesforce

For a Salesforce package-based application:

```text
Git tag
   ↔
Production release
   ↔
Package version
```

This creates traceability.

You can answer:

> Which source-code state corresponds to the version currently in Production?

---

# 15. Important Git Strategy Questions

The source says that before designing the Git strategy, you should answer several questions.

## Question 1

> At what stage should automated tests run?

---

## Question 2

> When should unlocked package versions be created, tested, and cleaned up?

---

## Question 3

> How should promotion / installation of package versions to environments such as QA sandboxes or Production be executed?

These questions are explicitly listed in the article. fileciteturn2file0L40-L46

---

# 16. "Run Everything on Every Commit" Sounds Good — But...

A team may say:

```text
Every commit:
   ↓
Run tests
   ↓
Create package
   ↓
Install package
```

But there is a problem.

Package version creation takes time.

The article gives an example where waiting around eight minutes for CI feedback can feel long, especially when developers commit multiple times per day.

---

# 17. CI Feedback Time

Imagine:

```text
Developer commits
       ↓
CI starts
       ↓
Scratch org
       ↓
Package version creation
       ↓
Installation
       ↓
Tests
       ↓
Feedback
```

If this takes:

```text
~8 minutes
```

then frequent commits can make development slow.

---

# 18. Package Version Creation Is Not Free

There are also daily limits for:

```text
Scratch org creation
```

and:

```text
Package version creation requests
```

Therefore:

> Don't create package versions unnecessarily.

The article warns that an overly aggressive package-version strategy can clutter the environment and consume available resources. fileciteturn2file0L43-L46

---

# 19. The CI/CD Trade-off

You want:

```text
Fast feedback
```

but also:

```text
Realistic package testing
```

These goals can conflict.

```text
More testing
   ↓
More confidence
   ↓
More time/resources
```

versus:

```text
Less testing
   ↓
Faster feedback
   ↓
Potentially later package problems
```

Architectural goal:

> Put the expensive package-level validation at the stage where it provides the best confidence without unnecessarily slowing everyday development.

---

# 20. Pull Request Strategy

Another option:

```text
Run package creation only on Pull Requests.
```

But then another question appears:

```text
Which branches?
```

Possible choices:

```text
Feature branches?
Develop?
Master?
Packaging branches?
All?
```

The source explicitly points out that there are many possible options and the team should detect problems as early as possible without unnecessarily slowing development. fileciteturn2file0L48-L48

---

# 21. Easy Spaces Branch Naming Convention

For the Easy Spaces sample application, the authors defined:

```text
feature/package-name/*
```

```text
packaging/package-name/*
```

```text
packaging
```

```text
develop
```

```text
master
```

Each branch has a different responsibility.

---

# 22. `feature/package-name/*`

This is the feature / bug-fix branch for an individual package.

Example:

```text
feature/es-base-code/customer-service
```

Conceptually:

```text
feature/package-name/*
```

means:

```text
Feature work
+
specific package context
```

---

# 23. `packaging/package-name/*`

This is a per-package branch used to test:

```text
Package creation
+
Package installation
```

for an individual package.

Example:

```text
packaging/es-base-code/*
```

Purpose:

```text
Feature works
        ↓
Now verify:
Feature works as a package
```

---

# 24. `packaging` Branch

The source says:

```text
packaging
```

reflects code changes for:

```text
Partial sandbox
```

or:

```text
UAT sandbox
```

It represents the stage where packages are installed into a sandbox for broader testing.

---

# 25. `develop` Branch

The source says:

```text
develop
```

reflects:

```text
Pre-production
```

For example:

```text
Full-copy sandbox
```

So:

```text
develop
   =
pre-production state
```

---

# 26. `master` Branch

The source says:

```text
master
```

reflects:

```text
Production
```

So:

```text
master
   =
Production source state
```

---

# 27. Branch Naming Is Customizable

The article explicitly says you do not have to use these exact names.

For example:

```text
packaging
```

could be:

```text
partial-sandbox
```

and:

```text
develop
```

could be:

```text
full-sandbox
```

The important requirement:

> The naming must make sense to your organization and remain manageable.

---

# 28. Environment-Oriented Naming

A practical mental model:

```text
feature/*
     ↓
package validation
     ↓
UAT / partial sandbox
     ↓
full-copy sandbox
     ↓
Production
```

Names can reflect:

```text
Technical workflow
```

or:

```text
Environment workflow
```

---

# 29. Branch Preconditions

The authors also defined:

```text
Preconditions
```

before:

```text
Branch merge
```

or:

```text
Code commit
```

They also defined specific CI tasks for each branch.

The source references a CircleCI workflow and notes that its graphic did not yet include branch filtering. fileciteturn2file0L57-L59

---

# 30. Full CI Workflow From the Source

The workflow is:

```text
feature/package-name/*
        |
        v
packaging/package-name/*
        |
        v
packaging
        |
        v
develop
        |
        v
master
```

Each stage performs different work.

---

# 31. Feature Branch CI

For:

```text
feature/package-name/*
```

the workflow runs:

```text
After every commit
```

Tasks:

```text
1. Deploy all package folders using source push
2. Run tests
3. Do NOT create package versions
4. Delete scratch org
```

The source explicitly lists these steps. fileciteturn2file0L63-L69

---

# 32. Feature Branch — Step 1

Deploy all package folders using:

```bash
sfdx force:source:push
```

Important:

> At this stage, the team is testing source-level development rather than creating package versions.

---

# 33. Feature Branch — Step 2

Run all tests.

Examples given by the article:

```text
Apex
+
Lightning Testing Service
```

So:

```text
Source Push
    ↓
Apex Tests
    +
Lightning Tests
```

---

# 34. Feature Branch — Step 3

Do not create package versions.

Why?

Because every commit is not necessarily worth turning into a package version.

At feature level:

```text
Development speed
```

has higher priority than:

```text
Package artifact creation
```

---

# 35. Feature Branch — Step 4

Delete the scratch org.

Purpose:

```text
Clean environment
+
Resource management
```

---

# 36. Packaging/Package Branch CI

For:

```text
packaging/package-name/*
```

the workflow runs:

```text
On every Pull Request
```

coming from:

```text
feature/package-name/*
```

---

# 37. Packaging/Package Branch — Step 1

Create a new package version for:

```text
package-name
```

This is the first stage where package-version creation becomes part of the automated workflow.

---

# 38. Packaging/Package Branch — Step 2

Deploy in:

```text
package directory order
```

based on:

```text
sfdx-project.json
```

to:

```text
sandbox org
```

All package directories are deployed:

```text
as packages
```

---

# 39. Packaging/Package Branch — Step 3

Run all tests:

```text
Apex
+
Lightning Testing Service
```

---

# 40. Packaging/Package Branch — Step 4

Delete scratch org.

So the complete flow is:

```text
PR
 ↓
Create package version
 ↓
Install/deploy packages
 ↓
Run tests
 ↓
Cleanup scratch org
```

The source explicitly defines this workflow. fileciteturn2file0L69-L74

---

# 41. `packaging` Branch CI

The `packaging` branch runs:

```text
On every Pull Request
```

coming from:

```text
packaging/package-name/*
```

---

# 42. Packaging Branch Tasks

Deploy package directories in:

```text
sfdx-project.json order
```

to:

```text
sandbox org
```

All package directories are deployed:

```text
as packages
```

Then:

```text
Run all tests
```

The source lists Apex and/or Lightning Testing Service as examples. fileciteturn2file0L75-L79

---

# 43. `develop` Branch CI

The `develop` branch runs on:

```text
Pull Request
```

coming from:

```text
packaging
```

---

# 44. Develop Branch Tasks

Deploy package directories in:

```text
sfdx-project.json order
```

to:

```text
full-copy sandbox
```

All package directories are deployed:

```text
as packages
```

Then:

```text
Run tests
```

Again:

```text
Apex
+
Lightning Testing Service
```

The source explicitly defines this flow. fileciteturn2file0L79-L82

---

# 45. `master` Branch

The source workflow defines:

```text
master
```

as:

```text
No automated actions
```

At first this may seem strange.

Why?

Because Production deployment requires a manual decision about:

```text
Which exact package version
```

should be promoted.

---

# 46. Why Master Has No Automatic Action

Suppose:

```text
Version A
```

is currently tested.

Meanwhile development continues and:

```text
Version B
```

has also entered testing.

If Production automatically takes:

```text
latest
```

you might accidentally promote:

```text
Version B
```

when the intended Production candidate was:

```text
Version A
```

Therefore the authors recommend manually selecting the exact package version.

---

# 47. Feature-Level Package Creation — Why Not?

The article says organizations may make different choices.

For example:

```text
Create package versions at feature level
```

if you want to ensure that developers don't introduce features that cannot be packaged.

But the Easy Spaces team chose not to do this.

---

# 48. Why Easy Spaces Didn't Create Package Versions on Feature Branches

Their reasoning:

```text
Feature development
>
Package testing
```

At feature branch level.

They did not want a package version for every simple commit.

Also, because package dependencies were already defined, mixing:

```text
source push
```

with:

```text
package installation
```

could fail.

---

# 49. Critical Dependency Example

Suppose:

```text
es-base-code
```

depends on:

```text
es-base-objects
```

If you do:

```text
source push es-base-objects
```

and then try:

```text
install es-base-code package
```

it will fail.

Why?

Because:

```text
es-base-code
```

requires:

```text
installed es-base-objects package
```

not merely source-pushed metadata.

The source explicitly calls out this dependency behavior. fileciteturn2file0L91-L96

---

# 50. Very Important — Source Push vs Package Installation

This is one of the most important Part 4 concepts.

These are not equivalent:

```text
Source Push
```

and:

```text
Package Installation
```

Example:

```text
source push ESBaseObjects
```

does NOT necessarily satisfy:

```text
ESBaseCode
depends on installed ESBaseObjects package
```

because the package dependency is part of the created package version.

---

# 51. "I'll Just Remove Dependencies" — Doesn't Work

A tempting idea:

```text
Remove dependencies from sfdx-project.json
        ↓
source push base package
        ↓
install dependent package
```

The article says this does not work.

Why?

Because:

> The dependencies are part of the created package version.

Changing the project JSON afterward does not alter the already-created package version.

This is a critical concept. fileciteturn2file0L94-L96

---

# 52. Package Version Captures Dependency Information

Think:

```text
sfdx-project.json
       |
       v
Package Version Creation
       |
       v
Dependency information becomes part
of package version
```

After version creation:

```text
Edit project JSON
       X
Does not rewrite old package version
```

You need to create the appropriate package version again.

---

# 53. Feature Completion → Package Version Preparation

Once the feature is complete:

```text
Feature branch
       |
       v
Feature works
       |
       v
Prepare package version command
```

The team modifies CI configuration with:

```text
Version number
Description
Other package-version data
```

This is important because the next PR moves the code into:

```text
packaging/package-name/*
```

and package-version creation kicks off.

---

# 54. Packaging Branch — Purpose

The source gives a useful distinction:

### Feature branch

```text
"Does the feature work?"
```

### Packaging branch

```text
"Does the feature work AND can it be packaged?"
```

This is an excellent architect-level mental model.

---

# 55. Packaging Branch as Intermediary

The source describes:

```text
packaging/package-name/*
```

as an intermediary between:

```text
Active Development
```

and:

```text
First Sandbox Testing
```

So:

```text
Developer
   ↓
Feature
   ↓
Package validation
   ↓
Sandbox
```

---

# 56. Optional Packaging Branch

The article explicitly says your organization may choose to:

```text
Skip packaging/package-name/*
```

and instead have someone manually create the package version.

That is a legitimate design choice.

The principle is:

> Choose what works for your organization.

---

# 57. Why the Authors Kept the Packaging Branch

They chose it because it allows them to see:

```text
All involved steps
+
All outputs
```

inside their CI solution UI.

They also built a script that can be:

```text
Used from CI
```

and:

```text
Manually invoked from developer workstation
```

This gives automation plus local reproducibility.

---

# 58. Packaging Branch — Environment Meaning

Once:

```text
New feature works
```

and:

```text
New package exists
```

the next step is:

```text
Install package into sandbox
```

This could be:

```text
Partial sandbox
```

or:

```text
UAT sandbox
```

---

# 59. Why Installation, Not Just Deployment?

At this stage, the focus is:

```text
Package Installation
```

The team fetches the package versions to use and deploys them through a Bash script.

The result:

```text
Sandbox
```

gets the latest code across the package directories defined in:

```text
sfdx-project.json
```

---

# 60. Important — Package Directory Order

Package deployment/installation follows:

```text
Package directory order
```

as defined by:

```text
sfdx-project.json
```

This connects directly to Part 3's dependency discussion.

Example:

```text
1. ESBaseObjects
2. ESBaseCode
3. ESBaseStyles
4. ESSpaceMgmt
```

---

# 61. `develop` — Full Copy Sandbox

The `develop` branch represents what could be:

```text
Full-copy sandbox
```

The workflow is intentionally similar to the `packaging` branch.

Focus:

```text
Package installation
+
Testing
```

---

# 62. Why Move to Another Testing Environment?

The lifecycle is:

```text
Feature
   ↓
Package validation
   ↓
Partial/UAT sandbox
   ↓
Full-copy sandbox
   ↓
Production
```

Each stage provides broader confidence.

---

# 63. The Steps Don't Change

The source emphasizes that the steps on:

```text
develop
```

are the same as on:

```text
packaging
```

The main change is the:

```text
Testing environment
```

before Production.

---

# 64. Master — Production Decision Point

Now we reach:

```text
master
```

The source says:

```text
No automated actions
```

because there is still an outstanding decision:

> Which package version should actually be promoted to Production?

---

# 65. Manual Production Package Selection

The team needs to choose:

```text
Exact package version
```

using:

```bash
sfdx force:package:version:promote
```

Historical command from the article.

---

# 66. Why Not Always Promote "Latest"?

Because:

```text
Development continues
```

while:

```text
Production candidate is being evaluated
```

Example:

```text
Version 1
   ↓
Testing

Meanwhile:

Version 2
   ↓
Development
```

If you blindly say:

```text
promote latest
```

you may promote the wrong artifact.

---

# 67. Production Candidate Selection

The source recommends:

```text
Manually pick the package version
```

and make an:

```text
Educated decision
```

because it affects Production and potentially:

```text
5 users
```

or:

```text
5,000 users
```

The exact source wording emphasizes the potential user impact of this decision. fileciteturn2file0L115-L120

---

# 68. Production Flow

Conceptually:

```text
develop
   ↓
Packaging validation
   ↓
Sandbox
   ↓
Full-copy sandbox
   ↓
Select exact package version
   ↓
Promote
   ↓
Install in Production
   ↓
Merge/update master
```

---

# 69. After Promotion and Installation

The source says:

```text
After promotion
+
installation
```

submit the code to:

```text
master
```

So:

```text
Production
   ↔
master
```

should remain aligned.

---

# 70. Master Tags = Source of Truth

The article strongly recommends:

```text
Tags on master
```

when releasing a new version.

Why?

Because this keeps:

```text
Source of Truth
```

aligned with:

```text
Production
```

---

# 71. Release Tag Example

Suppose Production receives:

```text
Package Version 1.0
```

Then Git master can have:

```text
Tag: 1.0
```

So:

```text
Production
    ↕
Git master
    ↕
Tag 1.0
    ↕
Package Version 1.0
```

This gives traceability.

---

# 72. Why Tags Are Useful

Tags let you:

```text
Go back
Compare releases
Inspect historical state
Understand Production source
```

The article explicitly says tags let the team compare changes between custom application releases. fileciteturn2file0L120-L120

---

# 73. Multiple Simultaneous Branches

The second branching image demonstrates a complex scenario with:

```text
feature branches
develop
release branches
hotfixes
master
```

at the same time.

The architectural concern is:

> Ensure the correct version becomes the Production/master version.

---

# 74. The Biggest Risk — Wrong Version Promotion

Imagine:

```text
Version A
   ↓
UAT

Version B
   ↓
Development

Version C
   ↓
Feature branch
```

If Production automation simply chooses:

```text
latest
```

you may promote:

```text
B or C
```

instead of:

```text
A
```

Therefore:

```text
Artifact identity
+
Release approval
```

must be explicit.

---

# 75. Complete Branch + Package Architecture

```text
                    ┌─────────────────────┐
                    │ feature/package/*   │
                    │ Source Push         │
                    │ Tests               │
                    │ No package version  │
                    └─────────┬───────────┘
                              │ PR
                              ↓
                    ┌─────────────────────┐
                    │ packaging/package/* │
                    │ Create version      │
                    │ Install as package  │
                    │ Tests               │
                    └─────────┬───────────┘
                              │ PR
                              ↓
                    ┌─────────────────────┐
                    │ packaging           │
                    │ Package install     │
                    │ Tests               │
                    │ Partial/UAT sandbox │
                    └─────────┬───────────┘
                              │ PR
                              ↓
                    ┌─────────────────────┐
                    │ develop             │
                    │ Package install     │
                    │ Tests               │
                    │ Full-copy sandbox   │
                    └─────────┬───────────┘
                              │
                              │ Manual selection
                              ↓
                    ┌─────────────────────┐
                    │ Production          │
                    │ Promote exact       │
                    │ package version     │
                    └─────────┬───────────┘
                              ↓
                           master
                              +
                         release tag
```

---

# 76. Part 4 — CI/CD Strategy in One Picture

```text
Developer
    |
    v
Feature Branch
    |
    +--> source:push
    +--> tests
    +--> scratch cleanup
    |
    v
Packaging Package Branch
    |
    +--> create package version
    +--> package installation
    +--> tests
    |
    v
Packaging Branch
    |
    +--> install packages
    +--> tests
    +--> partial/UAT sandbox
    |
    v
Develop
    |
    +--> install packages
    +--> tests
    +--> full-copy sandbox
    |
    v
Manual Production Candidate Selection
    |
    v
Promote Package Version
    |
    v
Production
    |
    v
Master + Tag
```

---

# 77. Why Feature Branch Uses `source:push`

The key reason is speed and developer feedback.

Feature branch:

```text
Every commit
```

Package version creation:

```text
Not every commit
```

So:

```text
Feature development
=
fast source-level validation
```

while:

```text
Packaging branch
=
package-level validation
```

---

# 78. Why Package Branch Uses Package Installation

At packaging level, the question changes.

Feature branch asks:

> Does the code work?

Packaging branch asks:

> Does the code work when delivered as a package?

This distinction is fundamental.

---

# 79. Source Push vs Package Install — Architect View

## Source Push

```text
Source
   ↓
Scratch Org
```

Good for:

```text
Fast development
Frequent feedback
Local feature validation
```

## Package Install

```text
Package Version
   ↓
Target Org
```

Good for:

```text
Real package validation
Dependency validation
Release artifact validation
Environment promotion
```

---

# 80. Why You Should Not Mix Them Blindly

Example:

```text
ESBaseObjects
   ↓
source push
```

then:

```text
ESBaseCode
   ↓
package install
```

If ESBaseCode declares:

```text
dependency on installed ESBaseObjects
```

the package install can fail because the source-pushed metadata is not equivalent to an installed package dependency.

---

# 81. Dependency State Is Part of the Artifact

This is the key:

```text
sfdx-project.json
      ↓
Package Version Creation
      ↓
Dependency embedded in package version
```

Therefore:

```text
Changing JSON later
      X
doesn't modify existing package version
```

You must create the appropriate new version.

---

# 82. Version Preparation

Before moving a feature into:

```text
packaging/package-name/*
```

the team modifies CI configuration with information such as:

```text
Version Number
Description
```

This means the package version creation is intentional.

---

# 83. Practical Version Naming

Instead of:

```text
0.1
0.2
0.3
```

use meaningful information where your process supports it.

Example:

```text
Customer Search MVP
Reservation Fix
Shared Services Refactor
```

This makes CI output easier to understand.

---

# 84. Environment Promotion Model

The source's model can be understood as:

```text
Developer Environment
        ↓
Scratch Org
        ↓
Partial/UAT Sandbox
        ↓
Full-Copy Sandbox
        ↓
Production
```

with:

```text
Git branch
```

representing the state associated with each stage.

---

# 85. Branch ↔ Environment Mapping

| Branch | Conceptual Environment | Main Purpose |
|---|---|---|
| `feature/package-name/*` | Scratch org | Active development |
| `packaging/package-name/*` | Package validation | Create/test package |
| `packaging` | Partial/UAT sandbox | Package installation testing |
| `develop` | Full-copy sandbox | Pre-production testing |
| `master` | Production | Production source state |

This table reflects the source's Easy Spaces model. fileciteturn2file0L50-L57

---

# 86. Branch ↔ CI Action Matrix

| Branch | Trigger | Package Version? | Tests | Environment |
|---|---|---:|---|---|
| `feature/package-name/*` | Every commit | No | Apex/LTS | Scratch |
| `packaging/package-name/*` | PR from feature | Yes | Apex/LTS | Sandbox |
| `packaging` | PR from package branch | Existing versions installed | Apex/LTS | Partial/UAT |
| `develop` | PR from packaging | Existing versions installed | Apex/LTS | Full-copy |
| `master` | Production release | Manual selection | Release process | Production |

The source specifically defines the first four workflows and leaves master with no automated action. fileciteturn2file0L63-L85

---

# 87. Errors & Gotchas

## 87.1 Creating a Package Version on Every Commit

Problem:

```text
Too many package versions
+
Slow CI
+
Resource consumption
+
Daily limits
```

Source recommendation for Easy Spaces:

```text
No package versions on feature commits.
```

---

## 87.2 Mixing Source Push and Package Install

Problem:

```text
Base package source-pushed
+
Dependent package installed
```

Can fail because dependency expects:

```text
Installed package
```

not just source metadata.

---

## 87.3 Removing Dependencies from `sfdx-project.json`

Problem:

```text
Remove dependency
```

after package version creation.

It doesn't rewrite the existing package version.

---

## 87.4 Always Promoting "Latest"

Problem:

```text
Latest development version
≠
Approved Production version
```

Manual package-version selection is safer in the source's model.

---

## 87.5 No Git Tag

Problem:

```text
Production changed
```

but:

```text
Git history has no release marker
```

Traceability becomes harder.

---

## 87.6 Too Many Branches

Problem:

```text
feature A
feature B
feature C
release
hotfix
packaging
develop
master
```

can create merge complexity.

Branching strategy should remain manageable.

---

## 87.7 Branch Names Don't Explain Their Purpose

If:

```text
branch-1
branch-2
test-new
temp
```

are used without a clear convention, the release process becomes difficult to understand.

Use consistent naming.

---

# 88. Advanced — CI Pipeline Design

A mature pipeline can be visualized as:

```text
                    COMMIT
                       |
                       v
                Source Validation
                       |
                       v
                  Unit Tests
                       |
                       v
              Package Validation
                       |
                       v
             Package Version Build
                       |
                       v
               Package Install
                       |
                       v
             Integration Tests
                       |
                       v
               UAT Validation
                       |
                       v
               Release Approval
                       |
                       v
                  Production
```

But the key is:

> Not every stage must run on every commit.

---

# 89. Advanced — Fast Feedback vs Full Validation

Two pipeline classes:

### Fast pipeline

```text
Every commit
```

Run:

```text
Source push
Unit tests
Basic validation
```

### Release/package pipeline

```text
PR / package stage
```

Run:

```text
Package version
Package installation
Integration tests
Environment validation
```

This separation improves developer productivity while retaining package-level confidence.

---

# 90. Advanced — Pipeline Cost Model

Every CI action has a cost:

```text
Time
+
Scratch org consumption
+
Package version request consumption
+
Compute
+
Developer waiting time
```

Therefore:

```text
Pipeline design
=
Quality
+
Speed
+
Resource management
```

---

# 91. Advanced — CI/CD Should Be Layered

Recommended conceptual structure:

```text
Layer 1
Developer feedback

Layer 2
Package validation

Layer 3
Environment validation

Layer 4
Production release
```

This mirrors the source's branch strategy.

---

# 92. Advanced — Artifact Promotion vs Rebuild

A critical release-management principle:

```text
Build artifact once
        ↓
Test artifact
        ↓
Promote same artifact
```

instead of:

```text
Build A
   ↓
Test A

Build B
   ↓
Production
```

The source's manual selection of an exact package version naturally supports the first model.

---

# 93. Advanced — Why Exact Package Version Matters

Suppose:

```text
Package Version 1.5.0
```

passed:

```text
UAT
```

while:

```text
Package Version 1.6.0
```

is being developed.

Production should explicitly identify:

```text
1.5.0
```

if that is the approved candidate.

Not:

```text
latest
```

---

# 94. Advanced — Release Candidate Concept

You can think of:

```text
Package Version
```

as:

```text
Release Candidate
```

once it enters the packaging/UAT flow.

Then:

```text
UAT passed
      ↓
Approval
      ↓
Promotion
      ↓
Production
```

---

# 95. Advanced — Git Tag + Package Version

Best traceability model:

```text
Git Commit
    |
    +--> Git Tag 1.0
    |
    +--> Package Version 1.0
    |
    +--> Production Release 1.0
```

This creates a release chain.

---

# 96. Advanced — Release Traceability

You should ideally be able to answer:

```text
Production version?
       ↓
Package version?
       ↓
Git tag?
       ↓
Git commit?
       ↓
Pull Request?
       ↓
Feature branch?
       ↓
Developer changes?
```

This is a major DevOps/architect capability.

---

# 97. Advanced — Hotfix Traceability

Hotfix:

```text
master
   |
   v
hotfix
   |
   +--> fix
   |
   +--> master
   |
   +--> develop
```

This prevents:

```text
Production fix
```

from disappearing from:

```text
future development
```

---

# 98. Advanced — Release Branch Strategy

The source's Git image includes release branches.

A typical pattern:

```text
develop
   |
   +--> release/1.0
            |
            +--> stabilization
            +--> testing
            |
            +--> master
```

While:

```text
develop
```

can continue receiving future features.

---

# 99. Advanced — Feature Freeze

A release branch can be used conceptually for:

```text
Feature Freeze
```

Meaning:

```text
Only release stabilization
```

rather than:

```text
New unrelated features
```

This helps stabilize a Production candidate.

---

# 100. Advanced — Environment State vs Branch State

A branch should not be thought of as only a Git location.

In this model it represents:

```text
Expected application state
+
Deployment stage
+
CI behavior
```

Example:

```text
develop
```

means:

```text
Pre-production source state
+
full-copy sandbox validation
```

---

# 101. Advanced — Branch Policy

Each branch can have explicit policy:

```text
feature/*
  → fast checks

packaging/*
  → package creation

packaging
  → UAT

develop
  → full-copy validation

master
  → production source
```

This reduces ambiguity.

---

# 102. Advanced — Pull Request as a Quality Gate

PRs can enforce:

```text
Code review
+
Automated tests
+
Package validation
+
Environment validation
```

before merge.

---

# 103. Advanced — Package Boundary + Branch Boundary

A package and branch can align:

```text
Package A
   |
   +--> feature/package-A/*
   +--> packaging/package-A/*
```

This makes it easier to understand:

```text
Who changed what?
Which package?
Which release?
```

---

# 104. Advanced — Monorepo Package Strategy

The article's model can work with multiple package directories in one project:

```text
repo
|
+-- package-A
+-- package-B
+-- package-C
+-- sfdx-project.json
```

CI can process packages according to:

```text
package directory order
```

and dependency relationships.

---

# 105. Advanced — Dependency-Aware Packaging

Suppose:

```text
A → B → C
```

Then package CI should understand:

```text
C first
B second
A third
```

or otherwise use the package manager's dependency resolution appropriately.

The source's `sfdx-project.json` ordering is explicitly part of its deployment workflow.

---

# 106. Advanced — Don't Confuse Git Dependency and Package Dependency

Git:

```text
branch relationship
```

Package:

```text
metadata/package relationship
```

They are different.

Example:

```text
feature/A
```

doesn't automatically mean:

```text
Package A depends on Package B
```

Package dependencies must be declared at package level.

---

# 107. Advanced — Don't Confuse Branch With Environment

A branch can represent an environment in a team's process, but:

```text
Git branch
```

and:

```text
Salesforce org
```

are not technically the same thing.

The process maps them:

```text
develop
   ↔
full-copy sandbox
```

but they remain separate systems.

---

# 108. Super Advanced — Release State Machine

The entire workflow can be modeled as a state machine:

```text
DEVELOPMENT
     |
     v
TESTED
     |
     v
PACKAGE_CREATED
     |
     v
PACKAGE_VALIDATED
     |
     v
UAT
     |
     v
APPROVED
     |
     v
PROMOTED
     |
     v
PRODUCTION
```

Transitions are controlled by:

```text
Tests
+
PRs
+
Approvals
+
Package version identity
```

---

# 109. Super Advanced — Package Version as Immutable Artifact

Once a package version is created:

```text
Package Version
```

should be treated as a release artifact.

You should not think:

```text
I'll modify this same version.
```

Instead:

```text
Change source
   ↓
Create new version
```

This aligns well with controlled release management.

---

# 110. Super Advanced — Build Once, Promote Many

Ideal pipeline concept:

```text
Create package version
        ↓
Test
        ↓
UAT
        ↓
Production
```

The same version moves through environments.

This prevents environment-specific rebuild differences.

---

# 111. Super Advanced — Environment Drift

Suppose:

```text
UAT
```

contains package:

```text
1.2
```

but Production contains:

```text
1.1
```

The release process should explicitly know why.

A mature pipeline maintains:

```text
Environment
+
Package Version
+
Git Tag
```

mapping.

---

# 112. Super Advanced — Release Inventory

You can maintain a conceptual inventory:

| Environment | Package | Version | Git Tag | Status |
|---|---|---|---|---|
| Dev | ESBaseCode | X | feature-* | Development |
| UAT | ESBaseCode | X | release-* | Testing |
| Full Copy | ESBaseCode | X | release-* | Pre-prod |
| Production | ESBaseCode | X | 1.0 | Live |

This is an architectural extension of the source workflow.

---

# 113. Super Advanced — CI/CD Governance

A mature enterprise process can define:

```text
Who can:
[ ] Create package versions?
[ ] Promote package versions?
[ ] Install Production packages?
[ ] Merge into master?
[ ] Create release tags?
```

This turns technical automation into release governance.

---

# 114. Super Advanced — Production Approval

Production should ideally have an explicit approval point:

```text
Candidate Version
       ↓
Test Evidence
       ↓
Business Approval
       ↓
Release Approval
       ↓
Promotion
       ↓
Production
```

The source's manual package-version selection is the starting point for this model.

---

# 115. Super Advanced — Rollback Thinking

For package-based releases, define:

```text
What version is currently live?
```

and:

```text
What is the recovery strategy?
```

At minimum, the team should know:

```text
Current Production package version
Previous Production package version
Git tag
Deployment history
```

Exact rollback mechanics depend on the metadata/package type and current Salesforce capabilities.

---

# 116. Super Advanced — Emergency Hotfix

Example:

```text
Production
   |
   X
Critical bug
```

Workflow:

```text
master
   ↓
hotfix
   ↓
Test
   ↓
Package/release process as required
   ↓
Production
   ↓
Merge hotfix back to develop
```

This prevents future releases from reintroducing the old bug.

---

# 117. Super Advanced — Release Train

For multiple packages:

```text
Package A
Package B
Package C
```

you may define:

```text
Release Train
```

where compatible package versions are grouped into a tested release.

Example:

```text
A 2.1
B 3.4
C 1.7
```

Together:

```text
Enterprise Release 2026.09
```

The source doesn't explicitly define a release-train feature; this is an architectural extension.

---

# 118. Super Advanced — Independent Package Releases

Alternatively:

```text
Package A → release independently
Package B → release independently
Package C → release independently
```

This is useful only if:

```text
Dependencies
+
Testing
+
Ownership
+
Release process
```

support independent releases.

---

# 119. Super Advanced — Coupling Is the Real Question

Before creating many packages, ask:

```text
Can this package really evolve independently?
```

If:

```text
A always changes with B
B always changes with C
C always changes with A
```

then packaging may create administrative complexity without providing much independence.

---

# 120. Super Advanced — Package Boundary Design

Good package boundary:

```text
Cohesive
+
Clear ownership
+
Clear API/metadata contract
+
Controlled dependencies
+
Independent release lifecycle
```

Weak package boundary:

```text
Highly coupled
+
Many cross-references
+
Frequent synchronized changes
+
No clear ownership
```

---

# 121. Architect Decision Framework

Before introducing a new package, ask:

```text
1. What business capability does it represent?
2. Who owns it?
3. What metadata does it contain?
4. What does it depend on?
5. Who depends on it?
6. Can it be independently tested?
7. Can it be independently versioned?
8. How often does it change?
9. Which environments need it?
10. What is the CI cost?
11. What is the Production release process?
12. How will emergency changes work?
```

---

# 122. Real-World Salesforce Example

Suppose an enterprise has:

```text
Core Data
Shared Services
Shared UI
Sales
Service
Reservations
Analytics
```

Possible packages:

```text
CoreData
    ↓
SharedServices
    ↓
SharedUI
    ↓
ReservationApp
```

Git:

```text
feature/reservation/*
        ↓
packaging/reservation/*
        ↓
packaging
        ↓
develop
        ↓
master
```

CI:

```text
Feature:
source push

Packaging:
create package version

UAT:
install package

Full Copy:
install package

Production:
promote exact version
```

---

# 123. Real-World Example — Hotfix

Production:

```text
ReservationApp 2.4
```

Critical bug:

```text
Booking confirmation fails
```

Hotfix:

```text
master
   ↓
hotfix/booking-confirmation
   ↓
Fix
   ↓
Test
   ↓
Package/release process
   ↓
Production
   ↓
develop
```

The exact packaging steps should follow the organization's current package/release tooling.

---

# 124. Interview Q&A

## Q1. Why shouldn't we create a package version on every commit?

**Answer:**

Because package-version creation can be slow and has resource/request limits. At feature level, developers need fast feedback. The Easy Spaces strategy therefore uses source push and tests on every feature commit, while package version creation happens later at the packaging branch.

---

## Q2. Why use `source:push` on feature branches?

**Answer:**

To provide fast source-level feedback without creating a package version for every commit.

---

## Q3. Why create a package version on `packaging/package-name/*`?

**Answer:**

Because that branch changes the question from "does the feature work?" to "does the feature work and package correctly?"

---

## Q4. Why can't you source-push ESBaseObjects and then install ESBaseCode?

**Answer:**

Because ESBaseCode has a package dependency on an installed ESBaseObjects package. Source-pushed metadata is not equivalent to an installed package version for satisfying that package dependency.

---

## Q5. Can we simply remove dependencies from `sfdx-project.json`?

**Answer:**

Not for an already-created package version. The dependency information is part of the package version, so changing `sfdx-project.json` afterward does not modify that existing package version.

---

## Q6. Why have a separate packaging branch?

**Answer:**

It acts as an intermediary between active feature development and sandbox testing, allowing the team to validate package creation and installation.

---

## Q7. Is the packaging branch mandatory?

**Answer:**

No. The source explicitly says an organization may skip it and manually create package versions instead.

---

## Q8. Why does the `develop` branch install packages again?

**Answer:**

Because it represents a later pre-production environment, such as a full-copy sandbox. The same package-based installation process is validated in a broader environment.

---

## Q9. Why doesn't master automatically promote the latest package?

**Answer:**

Because development continues in parallel. The latest package version may not be the version approved for Production. The source recommends manually selecting the exact package version.

---

## Q10. Why tag master?

**Answer:**

To keep Git's Production source state aligned with released versions and make historical comparisons easier.

---

## Q11. What is the difference between a Git tag and a package version?

**Answer:**

A Git tag identifies a point in source-control history. A package version identifies a packaged Salesforce artifact. They can be linked to provide release traceability, but they are different artifacts.

---

## Q12. What is the purpose of a hotfix branch?

**Answer:**

To isolate an urgent Production fix and then merge the fix into both the Production line and ongoing development so the fix is not lost.

---

## Q13. What should CI do on feature commits?

**Answer:**

In the Easy Spaces example: source-push all package folders, run tests, do not create package versions, and delete the scratch org.

---

## Q14. What should CI do on packaging package branches?

**Answer:**

Create a package version, deploy/install package directories in the configured order, run tests, and clean up the scratch org.

---

## Q15. What is the purpose of `sfdx-project.json` package directory order?

**Answer:**

The source uses that order to determine the order in which package directories are deployed to the sandbox.

---

# 125. Interview — 60-Second Architect Answer

> "I would separate fast developer feedback from package-level validation. On feature branches, I would use source push and automated tests on every commit, but avoid creating package versions for every commit because package creation takes time and consumes limited resources. Once a feature is ready, I would move it into a package-specific packaging branch where CI creates the package version and validates installation and tests. Then a packaging/UAT branch would install the package versions into a partial or UAT sandbox, followed by a develop branch representing a broader pre-production environment such as a full-copy sandbox. For Production, I would explicitly select the exact package version rather than automatically promoting latest, because newer versions may already be under development. Finally, I would align the Production state with master and tag the release so Git, package versions, and Production remain traceable."

---

# 126. Part 4 — Error/Gotcha Cheat Sheet

```text
Problem:
Package version creation too often
→ Reduce package creation frequency

Problem:
Feature source-pushed but dependent package install fails
→ Validate installed package dependency

Problem:
Dependency removed from JSON but old package still behaves the same
→ Dependency is already captured in created package version

Problem:
"latest" package selected for Production
→ Explicitly select approved version

Problem:
Production fix not present in develop
→ Merge hotfix back to develop

Problem:
Production state hard to identify in Git
→ Tag master releases

Problem:
Too many branches
→ Simplify branching model

Problem:
CI too slow
→ Separate fast feature validation from package validation
```

---

# 127. Part 4 — Limits / Constraints Mentioned by Source

The article highlights:

1. Package-version creation takes time.
2. CI feedback can become slow if package creation happens on every commit.
3. Scratch-org creation has daily limits.
4. Package-version creation requests have daily limits.
5. Excessive package-version creation can clutter the environment.
6. Dependencies make source-push/package-install mixing problematic.
7. Existing package-version dependency information cannot be changed by editing `sfdx-project.json`.
8. The exact Production package version should be selected intentionally.
9. Branching can become complicated when multiple branches run simultaneously.

These are source-derived points. fileciteturn2file0L40-L48

---

# 128. Best Option — Architect's Decision Framework

There is no single Git strategy that is automatically correct for every Salesforce organization.

Use these questions:

```text
Team size?
Release frequency?
Number of packages?
Dependency complexity?
CI execution time?
Scratch-org limits?
Package-version limits?
UAT requirements?
Production approval model?
Hotfix frequency?
```

Then design the branching model.

---

# 129. When Feature-Level Package Creation Makes Sense

The article says an organization may choose package creation at feature level if it wants to detect unpackageable features earlier.

Use this if:

```text
Packaging compatibility
```

is more important than:

```text
Fast feature feedback
```

The Easy Spaces team chose otherwise.

---

# 130. When the Packaging Branch Makes Sense

Useful when:

```text
You want package creation automated
+
You want visible CI output
+
You want a clear boundary before sandbox
```

---

# 131. When You Might Skip the Packaging Branch

The source explicitly allows:

```text
Manual package version creation
```

instead.

This can make sense for:

```text
Small teams
Simple release processes
Low package count
Strong manual release ownership
```

The actual choice should be based on the organization's needs.

---

# 132. Real-World Release Model

A practical enterprise model:

```text
Developer
   ↓
Feature Branch
   ↓
Fast CI
   ↓
PR
   ↓
Package Branch
   ↓
Package Version
   ↓
UAT
   ↓
Full Copy
   ↓
Release Approval
   ↓
Exact Package Version
   ↓
Promote
   ↓
Production
   ↓
Master Tag
```

---

# 133. Full Part 3 + Part 4 Connection

Part 3:

```text
How to create packages?
How to version packages?
How to declare dependencies?
How to promote packages?
```

Part 4:

```text
When should packages be created?
Which branch should create them?
Which branch installs them?
Which environment validates them?
Which version goes to Production?
How do we align Git and Production?
```

Together:

```text
Package Architecture
        +
Git Architecture
        +
CI/CD Architecture
        =
Release Architecture
```

---

# 134. Complete End-to-End Salesforce DX Flow

```text
Developer
   |
   v
feature/package-name/*
   |
   +--> source:push
   +--> tests
   +--> scratch cleanup
   |
   v
PR
   |
   v
packaging/package-name/*
   |
   +--> package version
   +--> package installation
   +--> tests
   |
   v
PR
   |
   v
packaging
   |
   +--> package installation
   +--> tests
   +--> UAT/partial sandbox
   |
   v
PR
   |
   v
develop
   |
   +--> package installation
   +--> tests
   +--> full-copy sandbox
   |
   v
Manual release decision
   |
   v
Promote exact package version
   |
   v
Production
   |
   v
master
   |
   v
Tag release
```

---

# 135. Super-Advanced — Release Artifact Traceability

The ideal relationship is:

```text
Feature Branch
      ↓
Pull Request
      ↓
Commit
      ↓
Package Version
      ↓
UAT
      ↓
Production
      ↓
Git Tag
```

Therefore a Production incident can be traced backwards.

Example:

```text
Production Bug
      ↓
Package Version 2.1
      ↓
Git Tag 2.1
      ↓
Commit abc123
      ↓
PR #456
      ↓
Feature Branch
```

---

# 136. Super-Advanced — Build Once, Promote the Same Artifact

Do not conceptually think:

```text
Build for UAT
Build again for Production
```

Prefer:

```text
Build package version
       ↓
Test same package version
       ↓
UAT same package version
       ↓
Promote same package version
       ↓
Production same package version
```

This reduces uncertainty between environments.

---

# 137. Super-Advanced — Environment Promotion Matrix

| Stage | Artifact | Validation |
|---|---|---|
| Feature | Source | Unit/basic tests |
| Package branch | Package version | Package install + tests |
| Packaging/UAT | Same package version | Integration/UAT |
| Develop | Same package version | Pre-production |
| Production | Approved package version | Release |
| Master | Git source state | Tag/traceability |

---

# 138. Super-Advanced — CI Pipeline Gates

Potential gates:

```text
Gate 1
Source compilation

Gate 2
Unit tests

Gate 3
Package creation

Gate 4
Dependency validation

Gate 5
Package installation

Gate 6
Integration tests

Gate 7
UAT

Gate 8
Production approval

Gate 9
Promotion

Gate 10
Git tagging
```

This is an architectural extension of the source's workflow.

---

# 139. Super-Advanced — Why "Latest" Is Dangerous

Imagine:

```text
Version 1.0
→ passed UAT

Version 1.1
→ currently testing

Version 1.2
→ developer branch
```

If Production simply chooses:

```text
latest
```

then:

```text
Approved version
```

and:

```text
latest version
```

can be different.

Therefore:

```text
Release candidate identity
```

must be explicit.

---

# 140. Super-Advanced — Branching Strategy Is a Business Decision

Git branching is not only a technical decision.

It affects:

```text
Developer productivity
+
Release frequency
+
Testing effort
+
Operational risk
+
Production governance
```

Therefore the architect should ask:

> "What release process does the business need?"

before deciding:

> "Which Git flow should we use?"

---

# 141. Super-Advanced — Avoid Copying Git Flow Blindly

The article itself says the branch names can be customized.

Therefore don't blindly implement:

```text
feature
develop
release
hotfix
master
```

just because it is popular.

Instead define:

```text
Business release cadence
+
Package lifecycle
+
Environment strategy
+
Team size
+
CI constraints
```

then choose the simplest strategy that supports them.

---

# 142. Super-Advanced — Package Granularity and CI Cost

More packages can mean:

```text
More independent lifecycle
```

but also:

```text
More package versions
+
More dependency checks
+
More CI jobs
+
More release coordination
```

Therefore:

```text
Modularity
```

has an operational cost.

Architectural goal:

> Create meaningful boundaries, not the maximum possible number of packages.

---

# 143. Super-Advanced — Dependency Graph + CI

If:

```text
A → B → C
```

then CI can conceptually validate:

```text
C
 ↓
B
 ↓
A
```

A change in C may affect:

```text
B
+
A
```

Therefore package dependency graphs should influence:

```text
Testing scope
+
Release scope
+
Impact analysis
```

---

# 144. Super-Advanced — Change Impact Analysis

Before changing:

```text
ESBaseObjects
```

ask:

```text
Who depends on it?
```

Example:

```text
ESBaseObjects
 ├── ESBaseCode
 ├── ESBaseStyles
 └── ESSpaceMgmt
```

So a foundation change can trigger broader testing.

---

# 145. Super-Advanced — Hotfix Impact Analysis

A hotfix should answer:

```text
What Production version is affected?
What package owns the bug?
Which dependent packages consume it?
Does the fix require a new package version?
Which environments need revalidation?
```

This is where package architecture and release architecture meet.

---

# 146. Super-Advanced — Source of Truth

A healthy model is:

```text
Git
  +
Package Version
  +
Environment
```

all aligned.

Example:

```text
Git Tag 2.0
      ↕
Package Version 2.0
      ↕
Production 2.0
```

If one differs unexpectedly:

```text
Drift
```

needs investigation.

---

# 147. Super-Advanced — Production Drift

Example:

```text
master = version 2.0
Production = version 2.1
```

Now:

```text
Source
≠
Production
```

This should trigger reconciliation.

The exact drift-management implementation is organization-specific.

---

# 148. Part 4 — Practical Checklist

## Git Strategy

```text
[ ] Feature branch convention defined
[ ] Packaging branch convention defined
[ ] Pre-production branch defined
[ ] Production branch defined
[ ] Hotfix process defined
[ ] Release/tag convention defined
```

## CI

```text
[ ] Fast feature CI
[ ] Package validation CI
[ ] UAT CI
[ ] Pre-production CI
[ ] Production approval
[ ] Scratch cleanup
```

## Package

```text
[ ] Dependencies declared
[ ] Package version naming defined
[ ] Package version creation stage defined
[ ] Installation strategy defined
[ ] Promotion strategy defined
```

## Production

```text
[ ] Exact package version selected
[ ] Approval completed
[ ] Package promoted
[ ] Package installed
[ ] Master updated
[ ] Release tag created
```

---

# 149. Part 4 — One-Page Cheat Sheet

```text
FEATURE
-------
Every commit
source:push
run tests
NO package version
delete scratch org


PACKAGING/PACKAGE
-----------------
PR from feature
create package version
install packages
run tests
cleanup


PACKAGING
---------
PR from package branch
install package versions
run tests
partial/UAT sandbox


DEVELOP
-------
PR from packaging
install package versions
run tests
full-copy sandbox


MASTER
------
No automatic package action
Manually choose exact version
Promote package version
Install Production
Merge/update master
Tag release
```

---

# 150. Part 4 — Key Takeaways

1. Git strategy is a critical part of Salesforce CI/CD.
2. Active development happens in `develop` in the source's basic Git model.
3. Features and bug fixes use dedicated branches.
4. Tested work merges into `develop`.
5. Production code moves from `develop` to `master`.
6. Production releases should be tagged.
7. Multiple simultaneous branches create complexity.
8. CI strategy must decide when tests run.
9. CI strategy must decide when package versions are created.
10. CI strategy must decide how package versions are promoted.
11. Package version creation on every commit can be slow.
12. Scratch-org and package-version request limits matter.
13. Easy Spaces uses `feature/package-name/*`.
14. Easy Spaces uses `packaging/package-name/*`.
15. `packaging` represents partial/UAT sandbox work.
16. `develop` represents pre-production/full-copy sandbox.
17. `master` represents Production.
18. Feature branches use source push.
19. Feature branches do not create package versions.
20. Feature branches run tests.
21. Feature branch scratch orgs are deleted.
22. Package branches create package versions.
23. Package branches install packages.
24. Package branches run tests.
25. Packaging branch installs packages into a sandbox.
26. Develop branch installs packages into a full-copy sandbox.
27. Master has no automated action in the source workflow.
28. Production package version selection is manual.
29. "Latest" is not automatically the correct Production candidate.
30. Package dependencies are part of the created package version.
31. Editing `sfdx-project.json` after version creation doesn't rewrite that version.
32. Source-pushed metadata is not equivalent to an installed package dependency.
33. A packaging branch is optional.
34. The packaging branch provides a clear intermediary stage.
35. A reusable CI script can support both CI and local developer execution.
36. Package directory order comes from `sfdx-project.json`.
37. Package installation is emphasized in UAT/pre-production stages.
38. Production and Git should be aligned.
39. Tags provide release traceability.
40. Hotfixes need to flow back into development.
41. Branch names can be customized.
42. The strategy must remain manageable.
43. Package creation frequency should balance confidence and speed.
44. Package artifacts should be explicitly identified.
45. Release management is ultimately about controlling which exact artifact reaches Production.

---

# 151. Part 3 + Part 4 — Master Mental Model

```text
PART 2
------
Modularize metadata
       ↓
Package boundaries


PART 3
------
Create packages
       ↓
Create versions
       ↓
Declare dependencies
       ↓
Validate
       ↓
Promote


PART 4
------
Put packages into Git workflow
       ↓
Feature branches
       ↓
Package validation branches
       ↓
UAT
       ↓
Pre-production
       ↓
Exact Production candidate
       ↓
Promote
       ↓
Production
       ↓
Git tag
```

---

# 152. Architect's Final Mental Model

For a Salesforce Technical/Solution Architect, think in four layers:

```text
1. METADATA ARCHITECTURE
   What belongs together?

2. PACKAGE ARCHITECTURE
   What should have an independent lifecycle?

3. SOURCE CONTROL ARCHITECTURE
   How do developers collaborate and release changes?

4. CI/CD ARCHITECTURE
   When do we validate, package, install, promote, and release?
```

Together:

```text
Business Capability
       ↓
Metadata Boundary
       ↓
Package Boundary
       ↓
Git Branch
       ↓
CI Pipeline
       ↓
Environment
       ↓
Production Release
```

---

# 153. LinkedIn Post — Hinglish

## Salesforce Unlocked Packages + Git + CI/CD 🚀

Part 4 of Salesforce's Modular Development & Unlocked Packages series ne ek important point clear kiya:

**Package banana alone is not enough.**

Agar enterprise Salesforce org ko properly modularize karna hai, to:

```text
Metadata Architecture
        ↓
Package Architecture
        ↓
Git Strategy
        ↓
CI/CD
        ↓
Release Management
```

Easy Spaces example mein branch strategy kuch aisi thi:

```text
feature/package-name/*
        ↓
packaging/package-name/*
        ↓
packaging
        ↓
develop
        ↓
master
```

Aur har stage ka different purpose:

```text
Feature Branch
→ source push + tests
→ no package version

Packaging Branch
→ package version create
→ package install
→ tests

Packaging/UAT
→ package installation
→ tests

Develop
→ full-copy sandbox
→ package installation
→ tests

Production
→ manually select exact package version
→ promote
→ install
→ master + tag
```

Sabse important learning:

**"Latest package" ≠ "Approved Production package".**

Continuous development ke saath multiple package versions simultaneously testing mein ho sakte hain.

Isliye Production ke liye exact package version identify karna important hai.

Aur ek aur critical point:

```text
source push
      ≠
package installation
```

Agar `ESBaseCode` package `ESBaseObjects` package par depend karta hai, sirf `ESBaseObjects` ko source-push kar dena package dependency satisfy nahi karta.

Package dependency package version ka part ban jaati hai.

For Salesforce Architects, this is bigger than Git or packaging.

It is about designing a **controlled release architecture**.

#Salesforce #SalesforceDX #UnlockedPackages #SalesforceArchitect #DevOps #CICD #Git #SalesforceDevelopment #ReleaseManagement

---

# 154. Source Fidelity / Scope Note

The source article's content has been intentionally preserved rather than silently rewritten or replaced.

Preserved areas include:

- Series context
- Git branching introduction
- `develop`, `master`, feature and hotfix concepts
- Release-branch concept shown in the source diagram
- Questions around automated tests
- Package version creation timing
- Package promotion
- Scratch-org/package-version limits
- Pull Request strategy
- Easy Spaces branch naming
- Branch responsibilities
- CircleCI workflow context
- Feature branch commands and behavior
- Package dependency behavior
- `source:push` vs package installation issue
- The failed idea of removing dependencies from `sfdx-project.json`
- Package-version preparation
- `packaging/package-name/*`
- Packaging branch rationale
- Sandbox/UAT installation
- Bash script approach
- `develop` / full-copy sandbox
- `master` / no automated action
- Manual Production package selection
- `sfdx force:package:version:promote`
- Manual selection instead of blindly choosing latest
- Master release tags
- Production/source-of-truth alignment
- Multiple simultaneous branches

The additional Advanced / Super Advanced / Architect / Interview sections are intentionally marked as extensions so they are not confused with direct source statements.

---

# 155. Final Revision

## If interviewer asks:

### "How would you design Salesforce package CI/CD?"

Think:

```text
Feature
 ↓
Fast source-level validation
 ↓
Package-specific validation
 ↓
UAT package installation
 ↓
Pre-production package installation
 ↓
Select exact release candidate
 ↓
Promote
 ↓
Production
 ↓
Git tag
```

### If interviewer asks:

### "Why not package on every commit?"

Answer:

```text
Package creation is slower
+
Resource/request limits exist
+
Developers need fast feedback
+
Every commit does not need to become a release artifact
```

### If interviewer asks:

### "Why not deploy dependency by source push?"

Answer:

```text
Because package dependencies are resolved against
installed package versions. Source-pushed metadata
does not automatically satisfy an installed-package
dependency.
```

### If interviewer asks:

### "Why manual Production version selection?"

Answer:

```text
Because continuous development means multiple
package versions may exist simultaneously. The latest
version is not necessarily the approved Production
candidate.
```

### If interviewer asks:

### "Why tag master?"

Answer:

```text
To align Git history with Production releases and
provide traceability for what source state corresponds
to a released application version.
```

---

# 156. Final Architecture

```text
                         SALESFORCE RELEASE ARCHITECTURE

                                BUSINESS CHANGE
                                      |
                                      v
                            feature/package-name/*
                                      |
                         +------------+------------+
                         |                         |
                    source:push                 Tests
                         |                         |
                         +------------+------------+
                                      |
                                      v
                              PULL REQUEST
                                      |
                                      v
                         packaging/package-name/*
                                      |
                         +------------+------------+
                         |                         |
                  Package Version             Tests
                         |                         |
                         +------------+------------+
                                      |
                                      v
                                packaging
                                      |
                              Package Install
                                      |
                                   Tests
                                      |
                                  UAT/Partial
                                      |
                                      v
                                  develop
                                      |
                              Package Install
                                      |
                                   Tests
                                      |
                              Full-Copy Sandbox
                                      |
                                      v
                         MANUAL RELEASE DECISION
                                      |
                                      v
                         SELECT EXACT VERSION
                                      |
                                      v
                         PROMOTE PACKAGE VERSION
                                      |
                                      v
                                PRODUCTION
                                      |
                                      v
                                  master
                                      |
                                      v
                                RELEASE TAG
```

**Core principle:**

> **Fast feedback during development, package-level validation before environment promotion, explicit Production artifact selection, and Git tags that keep source control aligned with Production.**
