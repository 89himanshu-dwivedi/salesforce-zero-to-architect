# Working with Modular Development and Unlocked Packages — Part 3
## Salesforce DX • Unlocked Packages • Package Versions • Dependencies • App Development Lifecycle

> **Source:** Salesforce Developer Blog — *Working with Modular Development and Unlocked Packages: Part 3*  
> **Author:** Zayne Turner  
> **Publication date:** June 19, 2018
>
> **Scope:** This document preserves the source article's concepts, examples, commands, JSON, errors, warnings, package dependency model, and app-development lifecycle discussion. Additional architect/interview guidance is clearly marked as an extension.
>
> **Important historical note:** The source article is from 2018 and describes Summer '18 beta-era unlocked-package behavior and historical `sfdx force:*` commands. For a current Salesforce project, verify command syntax, package behavior, limits, and supported features against current Salesforce documentation.

---

# 1. Part 3 — What Are We Learning?

This is the third installment in the modular development and unlocked packages series.

The series covers:

```text
Part 1
What is a package?
How can we start segmenting an org?

        ↓

Part 2
How do we organize metadata into packages?
How do we organize metadata/projects in source control?

        ↓

Part 3
How do app-builder workflows change?
What happens when an unlocked package is installed into Production?

        ↓

Part 4
Git branching strategy
Continuous deployment
Where/how packaging fits into CI/CD
```

Part 3 specifically focuses on:

- Turning modules into unlocked packages
- Creating package versions
- Version naming and numbering
- Package aliases
- Package dependencies
- Dependency installation order
- Debugging package-version creation failures
- Flow/FlowDefinition issues
- JSON output for troubleshooting
- Querying packaging-related objects from the Dev Hub
- Package deprecation limitations in the historical Summer '18 beta
- Package version limits
- Package version aliases
- Validating packages in clean orgs
- Promoting package versions
- Installing packages into production
- App development implications
- Lightning App Builder limitations
- Staged adoption of unlocked packages
- Source control + modularization + package lifecycle

The source article explicitly says that Part 3 looks at **building unlocked packages, working with versioning, and setting up dependencies between packages.** fileciteturn1file0L11-L17

---

# 2. Part 3 Ka Core Idea

Part 2 mein humne modules banaye the.

Part 3 mein:

```text
Modules
   ↓
Packages
   ↓
Package Versions
   ↓
Dependencies
   ↓
Validation
   ↓
Promotion
   ↓
Installation
```

Simple architecture:

```text
Module
   |
   v
Unlocked Package
   |
   v
Package Version
   |
   +--> Dependencies
   |
   v
Install in Test Org
   |
   v
Promote
   |
   v
Production
```

But important:

> **Package create ho jana ≠ package version ready hona.**

Actual validation package version create karte waqt hoti hai.

---

# 3. Part 2 Se Part 3 Tak Transition

Part 2 mein author ne application ko modules mein divide kiya.

Example:

```text
es-base-objects
es-base-code
es-base-styles
es-space-mgmt
```

Part 3 ka next question:

> Ab in modules ko actual packages kaise banayenge?

Article ke workflow mein:

```text
Module
   ↓
Package
   ↓
Package Version
   ↓
Dependency Resolution
   ↓
Install/Promote
```

---

# 4. Turning Modules Into Packages

Author ke paas modules the.

` sfdx-project.json ` ko modify karke author:

- control kar sakta tha ki kaunsa module deploy ho,
- modules ko kis order mein deploy kiya jaye,
- package installation order simulate kar sakta tha.

Source article ke according, agar kisi module ko deploy nahi karna tha, to uski entry `sfdx-project.json` se remove ki ja sakti thi.

Agar deployment order test karna tha:

```text
Module 1
   ↓
source:push
   ↓
Module 2
   ↓
source:push
   ↓
Module 3
   ↓
source:push
```

This helped simulate:

```text
Package Installation Order
```

and test:

```text
Dependency Management
```

Source explicitly describes adding modules one-by-one to `sfdx-project.json` and running `sfdx force:source:push` between additions. fileciteturn1file0L21-L24

---

# 5. When to Create Packages?

Once the author was reasonably confident about:

```text
Module boundaries
+
Deployment order
+
Dependencies
```

the next step was:

```text
Turn modules into packages
```

---

# 6. Historical Warning — Package Deletion

Summer '18 context mein:

> An unlocked package could be updated, but an unlocked package could not be deleted.

This is a major historical warning.

Example:

```text
Create Package A
      |
      v
Experiment
      |
      v
Don't need it
      |
      X
Cannot simply delete unlocked package
```

Because the author was using a Dev Hub specifically for package experiments, this wasn't a major team-environment concern.

But even with only three people doing a handful of experiments, commands such as:

```bash
sfdx force:package:version:list
```

could accumulate a lot of unwanted package/version information.

---

# 7. Why Use a Trial/Separate Dev Hub for Experiments?

The article recommends that if you want to experiment with packaging without polluting your production Dev Hub, consider a trial Dev Hub environment.

Mental model:

```text
Production Dev Hub
        |
        +--> Important package lifecycle
        |
        X--> Random experiments

Experiment Dev Hub
        |
        +--> Prototype packages
        +--> Version experiments
        +--> Dependency experiments
```

This is especially useful when learning package creation.

> **Historical behavior:** Always check current package/deletion/deprecation behavior before applying this 2018 guidance literally.

---

# 8. Package Creation Command

The author uses:

```bash
sfdx force:package:create
```

for package creation.

The important thing is to understand:

```text
Package
=
Named package definition
+
Package type
+
Package source path
```

---

# 9. Project Structure Before Package Creation

The author's project looked roughly like:

```text
Easy-Spaces
|
+-- .sfdx
|
+-- config
|
+-- data
|
+-- es-base-code
|
+-- es-base-objects
|
+-- es-base-styles
|
+-- es-images
|
+-- es-space-mgmt
```

Only four modules became packages:

```text
es-base-objects
es-base-code
es-base-styles
es-space-mgmt
```

The following remained outside the package set:

```text
data
es-images
```

This is important:

> **Not every project directory has to become a package.**

---

# 10. First Package — ESBaseObjects

The author created the first package for:

```text
es-base-objects
```

Historical command:

```bash
sfdx force:package:create \
  -n ESBaseObjects \
  -d 'Easy Spaces base objects' \
  -t Unlocked \
  -r ./es-base-objects
```

---

# 11. Understanding the Package Create Command

Let's break it down.

```bash
sfdx force:package:create
```

This invokes package creation.

### `-n`

```text
-n ESBaseObjects
```

Package name:

```text
ESBaseObjects
```

---

### `-d`

```text
-d 'Easy Spaces base objects'
```

Package description.

The author explicitly recommends using a meaningful description.

Don't leave important packages with vague descriptions.

---

### `-t`

```text
-t Unlocked
```

Specifies the package type:

```text
Unlocked
```

So this creates an:

> **Unlocked Package**

---

### `-r`

```text
-r ./es-base-objects
```

Relative path to the package contents.

This means:

```text
Package
   |
   v
./es-base-objects
```

---

# 12. Why Use the `-r` Relative Path?

The relative path gives flexibility.

Instead of:

```text
cd es-base-objects
sfdx force:package:create ...
cd ../
cd es-base-code
sfdx force:package:create ...
```

you can issue package creation commands from the project root.

Example:

```bash
sfdx force:package:create ... -r ./es-base-objects

sfdx force:package:create ... -r ./es-base-code

sfdx force:package:create ... -r ./es-base-styles
```

It also makes the resulting `sfdx-project.json` easier to read.

---

# 13. Create One Package Per Package Module

The author creates a package for each module that is intended to become a package:

```text
es-base-objects
       ↓
ESBaseObjects

es-base-code
       ↓
ESBaseCode

es-base-styles
       ↓
ESBaseStyles

es-space-mgmt
       ↓
ESSpaceMgmt
```

---

# 14. Package Creation vs Package Version Creation

This distinction is critical.

### Package creation

```text
force:package:create
```

creates the package definition.

### Package version creation

```text
force:package:version:create
```

creates an actual version/snapshot of package contents.

Think:

```text
Package
   |
   +--> Definition / Container
   |
   v
Package Version
   |
   +--> Actual versioned contents
```

---

# 15. What Is a Package Version?

A package version is a snapshot of package contents at a point in time.

Example:

```text
ESBaseObjects
      |
      +--> v0.1
      |
      +--> v0.2
      |
      +--> v1.0
```

Each version represents a particular state of the package.

---

# 16. CLI Updates `sfdx-project.json`

After the package-create commands, the CLI updated:

```text
sfdx-project.json
```

The article describes this as automatic.

The file now contained package information in addition to the original project directories.

---

# 17. Package-Aware `sfdx-project.json`

The article's example contained entries like:

```json
{
  "packageDirectories": [
    {
      "path": "es-base-objects",
      "default": false
    },
    {
      "path": "es-base-code",
      "default": false
    },
    {
      "path": "es-base-styles",
      "default": false
    },
    {
      "path": "es-space-mgmt",
      "default": true
    },

    {
      "path": "./es-base-objects",
      "package": "ESBaseObjects",
      "id": "0HoB00000004CWSKA2",
      "versionName": "ver 0.1",
      "versionNumber": "0.1.0.NEXT",
      "default": false
    },

    {
      "path": "./es-base-code",
      "package": "ESBaseCode",
      "id": "0HoB00000004CWXKA2",
      "versionName": "ver 0.1",
      "versionNumber": "0.1.0.NEXT",
      "default": false
    },

    {
      "path": "./es-base-styles",
      "package": "ESBaseStyles",
      "id": "0HoB00000004CWcKAM",
      "versionName": "ver 0.1",
      "versionNumber": "0.1.0.NEXT",
      "default": false
    },

    {
      "path": "./es-space-mgmt",
      "package": "ESSpaceMgmt",
      "id": "0HoB00000004CWhKAM",
      "versionName": "ver 0.1",
      "versionNumber": "0.1.0.NEXT",
      "default": false
    }
  ],

  "namespace": "",
  "sfdcLoginUrl": "https://login.salesforce.com",
  "sourceApiVersion": "43.0",

  "packageAliases": {
    "ESBaseObjects": "0HoB00000004CWSKA2",
    "ESBaseCode": "0HoB00000004CWXKA2",
    "ESBaseStyles": "0HoB00000004CWcKAM",
    "ESSpaceMgmt": "0HoB00000004CWhKAM"
  }
}
```

---

# 18. Important `sfdx-project.json` Properties

Package-aware project configuration introduces:

```text
package
id
versionName
versionNumber
dependencies
packageAliases
```

These become important for package lifecycle management.

---

# 19. `packageAliases`

The article highlights:

```json
"packageAliases": {
  "ESBaseObjects": "0HoB00000004CWSKA2",
  "ESBaseCode": "0HoB00000004CWXKA2",
  "ESBaseStyles": "0HoB00000004CWcKAM",
  "ESSpaceMgmt": "0HoB00000004CWhKAM"
}
```

A package alias gives you a readable name for a package ID.

Instead of repeatedly using:

```text
0HoB00000004CWSKA2
```

you can refer to:

```text
ESBaseObjects
```

---

# 20. Why Package Aliases Matter

Without alias:

```text
0HoB00000004CWSKA2
```

Hard to remember.

With alias:

```text
ESBaseObjects
```

Much easier.

This is especially useful for:

- versioning,
- installation,
- scripts,
- automation,
- CI/CD,
- dependency declarations.

---

# 21. Package Alias = Automation-Friendly Naming

Conceptually:

```text
Package ID
     |
     v
Alias
     |
     v
Human-readable reference
```

This can make package automation easier to understand and maintain.

---

# 22. `versionName`

Example:

```json
"versionName": "ver 0.1"
```

This is a human-readable package-version name.

The article recommends eventually replacing default/generic names with meaningful names.

Example:

```text
Customer Onboarding Baseline
```

or:

```text
Reservation Analytics Refactor
```

depending on the release.

---

# 23. `versionNumber`

Example:

```json
"versionNumber": "0.1.0.NEXT"
```

The article explains the expected format as:

```text
Major.Minor.Patch.Build
```

Example:

```text
1.4.2.7
```

The historical CLI supported:

```text
NEXT
```

as a relative value in the final build position.

Example:

```text
0.1.0.NEXT
```

---

# 24. Version Number Structure

Think:

```text
1 . 2 . 3 . 4
|   |   |   |
|   |   |   +--> Build
|   |   +------> Patch
|   +----------> Minor
+--------------> Major
```

The exact versioning strategy should be defined by the team.

---

# 25. Redundant Package Directory Entries

After package creation, the `sfdx-project.json` can contain original module entries plus package-aware entries.

The article says the CLI updates are additive.

So you may temporarily have:

```text
Original:
es-base-objects

Package-aware:
./es-base-objects + package metadata
```

The author notes that keeping redundant entries may not necessarily cause problems, but the project should eventually be cleaned up.

---

# 26. Only One `default: true`

A key rule:

```text
Only one package directory
can have:
"default": true
```

So while cleaning up `sfdx-project.json`, verify that only one package directory is default.

---

# 27. Package Creation Does Not Mean Structure Is Correct

This is one of the most important concepts in Part 3.

You can successfully execute:

```bash
sfdx force:package:create
```

and still have a broken package architecture.

The real test:

```text
Package Version Creation
```

---

# 28. Why Package Version Creation Is the Real Test

Package creation may only establish:

```text
Package identity
+
source path
```

But package version creation forces Salesforce to process actual package contents.

Therefore:

```text
force:package:create
        |
        v
Package exists
        |
        v
force:package:version:create
        |
        v
Dependencies / metadata validity tested
```

---

# 29. Creating a Version of a Dependency-Free Package

For:

```text
ESBaseObjects
```

there were no dependencies.

The author could use:

```bash
sfdx force:package:version:create -p "ESBaseObjects" -x
```

---

# 30. What Does `-p` Mean?

It identifies the package:

```text
ESBaseObjects
```

Because aliases are available, a readable package name can be used instead of a package ID.

---

# 31. What Does `-x` Mean?

In the historical command used by the article, `-x` indicates package version creation should proceed in the intended unlocked-package flow.

For current CLI usage, verify the exact current meaning/syntax because Salesforce CLI commands have evolved since 2018.

---

# 32. ESBaseCode Fails During Version Creation

When the author tried to create a version of:

```text
ESBaseCode
```

the operation returned many errors.

Example:

```text
=== Package Version Create Request

NAME                           VALUE
─────────────────────────────  ──────────────────
ID                             08cB00000004CsYIAU
Status                         Error
Package Id                     0HoB00000004CWXKA2
Package Version Id
Subscriber Package Version Id
Tag
Branch
Created Date                   2018-06-07 09:35
Installation URL

=== Errors

(1) testDataFactory: Variable does not exist: m
(2) testDataFactory: Variable does not exist: markets
(3) testDataFactory: Invalid type: Space__c
(4) testDataFactory: Invalid type: Space__c
(5) testDataFactory: Variable does not exist: s
(6) testDataFactory: Variable does not exist: s
(7) testDataFactory: Variable does not exist: s
(8) testDataFactory: Variable does not exist: s
(9) testDataFactory: Variable does not exist: s
(10) testDataFactory: Variable does not exist: s
(11) testDataFactory: Variable does not exist: s
(12) marketServicesTest: Variable does not exist: allSpaces
...
```

---

# 33. More Detailed Errors

The article then identifies common key errors:

```text
marketServicesTest:
DML requires SObject or SObject list type:
List<Space__c>

marketServices:
Invalid type: Market__c

marketServices:
Invalid type: Space__c

testDataFactory:
Invalid type: Space__c

testDataFactory:
Invalid type: Reservation__c

testDataFactory:
Variable does not exist: Reservation_Status__c

marketServices:
Invalid type: Schema.Space__c

marketServices:
Invalid type: Schema.Market__c

marketServicesTest:
Invalid type: Market__c

marketServicesTest:
Invalid type: Space__c
```

There was also:

```text
Customer_Fields.Contact_Customer_Fields:
In field: Customer_Status__c
- no CustomField named Contact.Reservation_Status__c found
```

---

# 34. What Was Actually Wrong?

The code package contained explicit references to the:

```text
Easy Spaces schema
```

For example:

```text
Space__c
Market__c
Reservation__c
Reservation_Status__c
```

But those objects/fields lived in:

```text
ESBaseObjects
```

So:

```text
ESBaseCode
     |
     +--> Apex
     |
     +--> references Space__c
     +--> references Market__c
     +--> references Reservation__c
     |
     v
ESBaseObjects
```

The dependency existed technically.

But the CLI did not know about it.

---

# 35. Critical Lesson — Hidden Dependency vs Declared Dependency

There are two kinds of dependency:

### Technical dependency

Code references another package's metadata.

```text
Apex
  |
  v
Space__c
```

### Declared package dependency

`sfdx-project.json` explicitly says:

```json
"dependencies": [
  {
    "package": "ESBaseObjects",
    "versionNumber": "0.1.0.LATEST"
  }
]
```

The package system needs the second one.

---

# 36. Dependency Must Be Declared

The author needed to update the `ESBaseCode` package entry.

Before:

```json
{
  "path": "./es-base-code",
  "package": "ESBaseCode",
  "id": "0HoB00000004CWXKA2",
  "versionName": "ver 0.1",
  "versionNumber": "0.1.0.NEXT",
  "default": false
}
```

After:

```json
{
  "path": "./es-base-code",
  "package": "ESBaseCode",
  "id": "0HoB00000004CWXKA2",
  "versionName": "ver 0.1",
  "versionNumber": "0.1.0.NEXT",
  "default": false,
  "dependencies": [
    {
      "package": "ESBaseObjects",
      "versionNumber": "0.1.0.LATEST"
    }
  ]
}
```

---

# 37. Dependency Meaning

Now the package system understands:

```text
ESBaseCode
     |
     depends on
     v
ESBaseObjects
```

So package installation becomes:

```text
ESBaseObjects
       |
       v
ESBaseCode
```

---

# 38. `LATEST`

The article explains that, similar to:

```text
NEXT
```

in `versionNumber`, you can use:

```text
LATEST
```

for a package dependency.

Example:

```json
{
  "package": "ESBaseObjects",
  "versionNumber": "0.1.0.LATEST"
}
```

Meaning conceptually:

> Use the newest available build matching the specified package version line.

Always verify current version/dependency syntax in modern Salesforce CLI documentation.

---

# 39. Why Dependencies Matter

Without dependencies:

```text
Package B
   |
   X
Unknown dependency
```

With dependency:

```text
Package B
   |
   v
Package A version
```

Now the package manager knows the required installation relationship.

---

# 40. Full Dependency Chain

The final architecture in the article becomes:

```text
ESBaseObjects
      |
      v
ESBaseCode
      |
      v
ESBaseStyles
      |
      v
ESSpaceMgmt
```

More explicitly:

```text
ESSpaceMgmt
   |
   +--> ESBaseObjects
   |
   +--> ESBaseCode
   |
   +--> ESBaseStyles
            |
            +--> ESBaseObjects
            +--> ESBaseCode

ESBaseCode
   |
   +--> ESBaseObjects
```

---

# 41. Full `sfdx-project.json` From the Article

The source article's cleaned-up package configuration looked like:

```json
{
  "packageDirectories": [

    {
      "path": "./es-base-objects",
      "package": "ESBaseObjects",
      "id": "0HoB00000004CWSKA2",
      "versionName": "ver 0.1",
      "versionNumber": "0.1.0.NEXT",
      "default": false
    },

    {
      "path": "./es-base-code",
      "package": "ESBaseCode",
      "id": "0HoB00000004CWXKA2",
      "versionName": "ver 0.1",
      "versionNumber": "0.1.0.NEXT",
      "default": false,

      "dependencies": [
        {
          "package": "ESBaseObjects",
          "versionNumber": "0.1.0.LATEST"
        }
      ]
    },

    {
      "path": "./es-base-styles",
      "package": "ESBaseStyles",
      "id": "0HoB00000004CWcKAM",
      "versionName": "ver 0.1",
      "versionNumber": "0.1.0.NEXT",
      "default": false,

      "dependencies": [
        {
          "package": "ESBaseObjects",
          "versionNumber": "0.1.0.LATEST"
        },
        {
          "package": "ESBaseCode",
          "versionNumber": "0.1.0.LATEST"
        }
      ]
    },

    {
      "path": "./es-space-mgmt",
      "package": "ESSpaceMgmt",
      "id": "0HoB00000004CWhKAM",
      "versionName": "ver 0.1",
      "versionNumber": "0.1.0.NEXT",
      "default": true,

      "dependencies": [
        {
          "package": "ESBaseObjects",
          "versionNumber": "0.1.0.LATEST"
        },
        {
          "package": "ESBaseCode",
          "versionNumber": "0.1.0.LATEST"
        },
        {
          "package": "ESBaseStyles",
          "versionNumber": "0.1.0.LATEST"
        }
      ]
    }
  ],

  "namespace": "",
  "sfdcLoginUrl": "https://login.salesforce.com",
  "sourceApiVersion": "43.0",

  "packageAliases": {
    "ESBaseObjects": "0HoB00000004CWSKA2",
    "ESBaseCode": "0HoB00000004CWXKA2",
    "ESBaseStyles": "0HoB00000004CWcKAM",
    "ESSpaceMgmt": "0HoB00000004CWhKAM"
  }
}
```

---

# 42. Dependency Graph From This JSON

```text
ESBaseObjects
      |
      +----------------------+
      |                      |
      v                      v
ESBaseCode              ESBaseStyles
      |                      |
      +----------+-----------+
                 |
                 v
            ESSpaceMgmt
```

More exact:

```text
ESBaseCode
   └── ESBaseObjects

ESBaseStyles
   ├── ESBaseObjects
   └── ESBaseCode

ESSpaceMgmt
   ├── ESBaseObjects
   ├── ESBaseCode
   └── ESBaseStyles
```

---

# 43. Dependency Installation Order

The article says:

> The order in which dependencies are listed should follow the intended installation order.

Conceptually:

```text
1. ESBaseObjects
        ↓
2. ESBaseCode
        ↓
3. ESBaseStyles
        ↓
4. ESSpaceMgmt
```

Why?

Because:

```text
Higher layer
depends on
lower layer
```

---

# 44. Dependency Graph = Installation Plan

Think:

```text
Foundation
   ↓
Shared Services
   ↓
Shared UI
   ↓
Application
```

This is one of the strongest architect-level takeaways from Part 3.

---

# 45. Package Version Creation — Real Validation

After dependencies are declared:

```text
Create ESBaseObjects version
        ↓
Create ESBaseCode version
        ↓
Create ESBaseStyles version
        ↓
Create ESSpaceMgmt version
```

Each package can now understand its required dependencies.

---

# 46. Package Version Creation Is a Dependency Test

When:

```bash
sfdx force:package:version:create
```

fails, don't immediately assume the CLI is broken.

Ask:

```text
Which metadata does this package reference?
        |
        v
Where does that metadata live?
        |
        v
Which package owns it?
        |
        v
Is that dependency declared?
```

---

# 47. Flow / FlowDefinition Gotcha

The article highlights a special issue involving:

```text
Flow
FlowDefinition
```

The Salesforce CLI historically attempted to ignore inactive flow metadata in the project by default.

This can create a problem.

---

# 48. Flow Failure Scenario

Suppose:

```text
Flow
   |
   +--> Multiple versions
   |
   +--> All inactive
```

If there is no active version:

```text
Package Version Creation
        |
        X
        |
      Failure
```

The error message may not be very useful.

---

# 49. Recommended Flow Check

Before creating a package version:

```text
Check every Flow
      |
      v
Does it have an active version?
      |
   +--+--+
   |     |
  YES    NO
   |     |
   v     v
Keep   Remove inactive
       version metadata
```

The article strongly recommends:

1. Double-check that flows are active.
2. Proactively remove inactive flow-version metadata from the package directory.
3. Then attempt package-version creation.

---

# 50. Why Flow Metadata Is Tricky

Flow metadata can contain:

```text
Flow definition
+
Versions
+
Active/inactive states
```

Packaging has to deal with those states.

Therefore:

> Don't blindly package every Flow metadata file that happens to exist in source.

---

# 51. JSON Output for Troubleshooting

The article recommends using:

```text
-json
```

if:

```bash
sfdx force:package:version:create
```

fails.

Example historical pattern:

```bash
sfdx force:package:version:create ... -json
```

---

# 52. Why `-json` Helps

A normal CLI error might say:

```text
ERROR:
Cannot read property '0' of undefined.
```

Not very useful.

Using JSON output can provide:

```text
Path to the CLI file
```

that threw the error.

This can help trace the problem to a more specific project area.

---

# 53. Empty Files Can Also Cause Problems

The article mentions that:

> Empty files in project directories can also produce errors such as the one described above.

Therefore, when package version creation fails unexpectedly:

```text
Check:
[ ] Flow metadata
[ ] Inactive versions
[ ] Empty files
[ ] Package contents
[ ] Dependencies
```

---

# 54. Querying Packaging-Related sObjects

When package version creation fails, the CLI may suggest querying:

```text
Package2CreationRequestError
```

The CLI may even provide the query command.

But there is a catch.

---

# 55. Default Query Target Problem

The Salesforce CLI's default behavior for:

```bash
force:data:query
```

was to query the default scratch org.

But packaging-related objects belong to the:

```text
Dev Hub
```

context.

So if you blindly copy/paste the generated query, you may get:

```text
Invalid type
```

---

# 56. Use `-u` for the Dev Hub

The article says to add:

```text
-u
```

to target the project Dev Hub.

Conceptually:

```bash
sfdx force:data:query \
  -q "SELECT ..." \
  -u <DevHubAlias>
```

The exact modern CLI syntax should be verified for current Salesforce CLI.

---

# 57. Rule for Package2 Queries

When querying:

```text
Package2
Package2Version
Package2CreationRequestError
```

and related packaging objects, make sure the command targets the appropriate Dev Hub.

Mental model:

```text
Scratch Org
   |
   X
Package2 query

Dev Hub
   |
   ✓
Package2 query
```

---

# 58. Package Deprecation — Historical Summer '18 Limitation

The article states that, as of Summer '18:

> You could not deprecate an unlocked package version.

This matters when experimenting because package versions could accumulate.

The article therefore again emphasizes:

```text
Use meaningful descriptions
```

when creating packages.

---

# 59. Package Description

Example:

```bash
-d 'Easy Spaces base objects'
```

Don't use vague names like:

```text
Test
Package1
New Package
Temp
```

Better:

```text
Easy Spaces base objects
Shared customer services
Reservation management
```

---

# 60. Package Version Name

Similarly, package versions should have meaningful names.

Instead of:

```text
ver 0.1
```

you might use a meaningful release-oriented description.

The article says meaningful version names help manage packages over time.

---

# 61. Package Version Description

The article also notes that descriptions can be added to package versions.

Conceptually:

```text
Package
  |
  +--> Package Description
  |
  +--> Version Name
  |
  +--> Version Description
```

This improves traceability.

---

# 62. Package Version Limits

The article warns that there are limits on how many package-version requests you can make within a 24-hour period.

Historical command:

```bash
sfdx force:limits:api:display
```

Use the:

```text
-u
```

parameter to point to the Dev Hub.

---

# 63. Why Version Limits Matter

Bad workflow:

```text
Small change
   ↓
Create package version
   ↓
Small change
   ↓
Create package version
   ↓
Repeat 100 times
```

You can burn through request limits.

Better:

```text
Validate locally
   ↓
Validate dependencies
   ↓
Create package version intentionally
```

---

# 64. Package Version Aliases

The article has another important alias concept.

There are:

### Package aliases

```text
ESBaseObjects
```

### Package version aliases

Something like:

```text
ESBaseObjects@0.1.0.1
```

These are different.

---

# 65. How Package Version Alias Gets Added Automatically

According to the article, the CLI automatically updates the `packageAliases` section for a package version only when:

```text
-w
```

is used during package-version creation and a sufficiently large wait time is provided.

Conceptually:

```bash
sfdx force:package:version:create ... -w <wait>
```

The exact current syntax should be checked in modern CLI documentation.

---

# 66. Why `-w` Matters

Package version creation may be asynchronous.

Without waiting:

```text
Create request
      |
      v
Command exits before version ready
```

With an adequate wait:

```text
Create request
      |
      v
Wait
      |
      v
Version completes
      |
      v
Alias can be updated
```

---

# 67. If Wait Times Out

Suppose:

```text
-w
```

times out before package creation completes.

You may still need a version alias.

The article says you can manually add one to:

```text
packageAliases
```

---

# 68. Package Version Alias Format

The article gives the format:

```text
packageName@versionNumber
```

Example:

```text
ESBaseObjects@0.1.0.1
```

and map it to the package version ID beginning with:

```text
04t
```

The `04t` value corresponds to the:

```text
Subscriber Package Version ID
```

shown in package-version output.

---

# 69. Package ID vs Subscriber Package Version ID

Important distinction:

```text
Package ID
starts:
0Ho...
```

Historical package definition ID.

Package version subscriber ID:

```text
04t...
```

This distinction is important when creating aliases and installing versions.

---

# 70. Validate Package Versions Before Production

Once package versions are generated:

```text
Do NOT immediately install in Production.
```

First:

```text
Clean Scratch Org
       |
       v
Install Package
       |
       v
Validate
```

or:

```text
Sandbox
       |
       v
Install Package
       |
       v
Validate
```

---

# 71. Clean Scratch Org Validation

Why clean org?

Because your development org may already contain:

```text
Objects
Code
Settings
Dependencies
```

and can hide package dependency problems.

A clean org answers:

> Can this package actually install from scratch?

---

# 72. Package Installation Test

Conceptual process:

```text
Package Version
      |
      v
Clean Scratch Org
      |
      v
Install dependency
      |
      v
Install dependent package
      |
      v
Run tests
      |
      v
Validate behavior
```

---

# 73. Promotion

The article says:

> You cannot install the package into Production until you promote the package version.

Historical command:

```bash
sfdx force:package:version:promote
```

The promotion step marks the package version as ready for production use under the historical unlocked-package workflow.

---

# 74. Promotion Is a Gate

Think:

```text
Package Version Created
        |
        v
Test
        |
        v
Validate
        |
        v
Promote
        |
        v
Production Installation
```

---

# 75. Don't Confuse Create With Promote

```text
Create
=
Generate version

Promote
=
Move version toward production-ready status
```

So:

```text
Created
≠
Production-ready
```

---

# 76. App Development Impact

Now the article shifts from package mechanics to:

> What does this change for application development?

Traditional Salesforce development may use:

```text
Metadata Deployment
Change Sets
Other deployment mechanisms
```

Package-based development introduces:

```text
Modules
Packages
Versions
Dependencies
Installation
Promotion
```

---

# 77. Unlocked Packages Were Beta in Summer '18

The article's historical context:

```text
Unlocked Packages
=
Beta
```

Therefore there were limitations.

This is important because the source's specific limitations should **not** automatically be treated as current 2026 Salesforce behavior.

---

# 78. Lightning App Builder Limitation

One explicit historical limitation:

> You could not use Lightning App Builder to customize or edit Lightning Apps that had been installed as part of an unlocked package.

So:

```text
Unlocked Package
      |
      v
Installed Lightning App
      |
      X
Lightning App Builder customization
```

under the Summer '18 behavior described in the article.

---

# 79. Why This Matters Architecturally

If your app's UI is packaged:

```text
Package
   |
   +--> Lightning App
```

you need to understand whether administrators can:

```text
Customize
Extend
Override
Edit
```

that installed metadata in the target org.

If not, package ownership becomes more important.

---

# 80. Deprecation Limitation Again

The article notes that package-version deprecation was on the roadmap but not available in Summer '18.

This meant:

```text
Package Lifecycle
```

was still evolving.

---

# 81. Should Everyone Wait for GA?

The article asks:

> Should teams wait until unlocked packages reach GA?

The answer is not presented as a universal yes/no.

Instead:

> It depends on what is right for the organization.

---

# 82. Start Before Full Production Adoption

A team can begin preparing for packaging without immediately installing packages in Production.

Start with:

```text
Source Control
+
Metadata Modularization
```

Then:

```text
Small Modules
```

Then:

```text
Package Experiments
```

Then:

```text
Package Versioning
```

Then:

```text
Staging Installation
```

Then eventually:

```text
Production
```

---

# 83. Staged Adoption Model

Recommended conceptual path from the article:

```text
Stage 1
Source Control

        ↓

Stage 2
Modularize Metadata

        ↓

Stage 3
Build Small Candidate Packages

        ↓

Stage 4
Experiment With Versioning

        ↓

Stage 5
Install in Scratch Org

        ↓

Stage 6
Install in Sandbox / Staging

        ↓

Stage 7
Promote When Confident

        ↓

Stage 8
Production
```

---

# 84. Why Start Small?

Small modules allow the team to learn:

```text
What packages do
What dependencies look like
What tooling supports
What doesn't work
How source sync behaves
How installation behaves
How admins/devs are affected
```

without immediately impacting production users.

---

# 85. Team Feedback Loop

A major benefit of staged adoption:

```text
Developers
   |
   v
Package experiment
   |
   v
Team feedback
   |
   v
Architecture adjustment
   |
   v
Better package
```

Everyone supporting application development gets exposure to the new workflow.

---

# 86. Identify Side Effects Early

Before Production:

```text
Package
   |
   v
Staging Sandbox
   |
   v
Unexpected side effect
   |
   v
Fix
```

Instead of:

```text
Package
   |
   v
Production
   |
   v
Unexpected side effect
```

The first model is safer operationally.

---

# 87. Production Changes and Source Control

The article also highlights a difficult organizational problem:

> How do you capture and synchronize changes made directly in Production with source control?

This is critical.

Suppose:

```text
Git
   |
   v
Package
   |
   v
Production
```

But an admin changes something manually:

```text
Production
   |
   +--> Manual change
```

Now:

```text
Git state
   ≠
Production state
```

You need a process to detect and reconcile this drift.

---

# 88. Source of Truth Problem

A mature packaging workflow should aim for:

```text
Source Control
      |
      v
Controlled Change
      |
      v
Package Version
      |
      v
Environment
```

Rather than:

```text
Production
      |
      +--> Random manual change
      |
      v
Unknown source state
```

---

# 89. Incremental Adoption

As package functionality evolves:

```text
Salesforce Packaging Capability
          |
          v
Team learns
          |
          v
Package architecture evolves
          |
          v
Workflow improves
```

The article recommends incremental adjustments instead of trying to solve everything in one step.

---

# 90. Packaging Is a Capability, Not Just a Command

A team doesn't become "package-ready" simply by knowing:

```bash
force:package:create
```

Real package maturity requires:

```text
Source Control
+
Modularization
+
Dependency Management
+
Versioning
+
Testing
+
Promotion
+
Installation
+
Change Governance
+
Production Drift Management
```

---

# 91. Complete Part 3 Architecture

The full lifecycle can be represented as:

```text
                 SOURCE
                   |
                   v
        +----------------------+
        | Modular Metadata     |
        +----------------------+
                   |
                   v
        +----------------------+
        | Package Definition   |
        +----------------------+
                   |
                   v
        +----------------------+
        | Package Version      |
        +----------------------+
                   |
                   v
        +----------------------+
        | Dependency Resolution|
        +----------------------+
                   |
                   v
        +----------------------+
        | Clean Org Validation |
        +----------------------+
                   |
                   v
        +----------------------+
        | Promote              |
        +----------------------+
                   |
                   v
        +----------------------+
        | Production Install   |
        +----------------------+
```

---

# 92. Dependency Architecture

For the Easy Spaces example:

```text
                 ESSpaceMgmt
                 /    |    \
                /     |     \
               v      v      v
   ESBaseObjects  ESBaseCode  ESBaseStyles
                    |             |
                    v             v
              ESBaseObjects  ESBaseObjects
```

So installation needs to respect the dependency graph.

---

# 93. Package Dependency vs Metadata Dependency

This distinction is extremely important.

### Metadata dependency

```text
Apex
  |
  v
Space__c
```

### Package dependency

```text
ESBaseCode
  |
  v
ESBaseObjects
```

Metadata dependency is about technical references.

Package dependency tells the package manager:

> "Install package X before package Y."

Both need to align.

---

# 94. What Happens If You Don't Declare Dependency?

Example:

```text
ESBaseCode
references
Space__c
```

But:

```text
ESBaseCode
dependencies = []
```

Package version creation may fail with:

```text
Invalid type: Space__c
```

because the package build environment doesn't know where the required metadata comes from.

---

# 95. Dependency Declaration Pattern

```json
"dependencies": [
  {
    "package": "ESBaseObjects",
    "versionNumber": "0.1.0.LATEST"
  }
]
```

This is effectively saying:

```text
Before building/installing this package,
make the required version of ESBaseObjects available.
```

---

# 96. Dependency Tree — Interview View

Imagine:

```text
ESSpaceMgmt
│
├── ESBaseStyles
│   ├── ESBaseCode
│   │   └── ESBaseObjects
│   └── ESBaseObjects
│
├── ESBaseCode
│   └── ESBaseObjects
│
└── ESBaseObjects
```

This shows why dependency declarations matter.

---

# 97. Advanced — Dependency Closure

For a package to install successfully, all of its transitive dependencies must ultimately be available.

Example:

```text
A
 |
 v
B
 |
 v
C
```

A depends directly on B.

B depends directly on C.

Therefore A indirectly requires:

```text
C
```

This is called a transitive dependency relationship.

The package manager must resolve the dependency graph appropriately.

---

# 98. Advanced — Direct vs Transitive Dependency

```text
A → B
```

Direct dependency.

```text
A → B → C
```

A's:

```text
Direct dependency = B
Transitive dependency = C
```

Architects should understand both.

---

# 99. Advanced — Dependency Direction

Good:

```text
Application
    ↓
Shared Layer
    ↓
Foundation
```

Risky:

```text
Foundation
    ↓
Application
```

if it creates unwanted upward coupling.

Even more dangerous:

```text
A → B
B → C
C → A
```

Circular dependency.

---

# 100. Advanced — Circular Dependency

Suppose:

```text
Package A
   |
   v
Package B
   |
   v
Package A
```

Now installation/versioning becomes problematic.

Therefore:

> Keep dependency direction intentional and avoid circular package relationships.

---

# 101. Advanced — Foundation Package Has High Blast Radius

Example:

```text
ESBaseObjects
```

is used by:

```text
ESBaseCode
ESBaseStyles
ESSpaceMgmt
```

Therefore:

```text
Change ESBaseObjects
      |
      +--> affects Code
      +--> affects Styles
      +--> affects Application
```

This is a high blast-radius package.

---

# 102. Advanced — Versioning Foundation Packages

Because many packages depend on:

```text
ESBaseObjects
```

you should treat its versions carefully.

Conceptually:

```text
ESBaseObjects v1
       |
       +--> Code v1
       +--> Styles v1
       +--> App v1
```

Then:

```text
ESBaseObjects v2
```

requires compatibility consideration.

---

# 103. Advanced — Semantic Versioning Mindset

The source article uses:

```text
Major.Minor.Patch.Build
```

An architect can additionally apply a semantic-versioning mindset:

```text
Major
= potentially breaking

Minor
= backward-compatible feature

Patch
= backward-compatible fix

Build
= generated/package build identity
```

This interpretation is an architectural extension; the article itself primarily explains the version-number format and `NEXT` behavior.

---

# 104. Advanced — Package Contract

A package should be treated as a contract.

For example:

```text
ESBaseCode
```

provides:

```text
Shared Apex Services
Shared Lightning Components
```

Consumers depend on that contract.

Therefore:

```text
Change
   |
   v
Check consumers
   |
   v
Version appropriately
```

---

# 105. Advanced — Clean Org as Integration Test

Installing a package into a clean org tests:

```text
Package completeness
+
Dependency completeness
+
Deployment assumptions
```

A package that only works in an already-customized developer org has a hidden dependency problem.

---

# 106. Advanced — Package Creation vs Installation Test

Two different tests:

### Build test

```text
Can I create a package version?
```

### Installation test

```text
Can I install that package version into a clean org?
```

A package can pass the first and still expose issues during the second.

---

# 107. Advanced — Promotion as Governance

Promotion can be treated as a release gate:

```text
Build
  ↓
Validate
  ↓
Review
  ↓
Promote
  ↓
Install
```

This creates an explicit transition between:

```text
Development artifact
```

and:

```text
Production candidate
```

---

# 108. Advanced — Package Lifecycle Governance

A mature team can define:

```text
Package Created
      ↓
Version Created
      ↓
Automated Tests
      ↓
Dependency Validation
      ↓
Sandbox Installation
      ↓
Business Validation
      ↓
Promotion
      ↓
Production
```

---

# 109. Advanced — Package Metadata vs Data

The project contains:

```text
data
```

as a separate area.

Remember:

```text
Package
   |
   +--> Metadata

Data
   |
   +--> Records
```

Packaging metadata doesn't automatically mean business data is handled the same way.

A separate data strategy may be required.

---

# 110. Advanced — Package vs Non-Deployable Assets

The project also contains:

```text
es-images
```

which did not become a package.

This demonstrates:

> Package boundaries should follow actual lifecycle/deployment requirements, not simply folder names.

---

# 111. Advanced — App Builder and Package Ownership

Historical limitation:

```text
Installed Lightning App
      |
      X
Lightning App Builder editing
```

Architectural implication:

If administrators need frequent post-install customization, evaluate whether the affected metadata should be:

```text
packaged
```

or:

```text
configured separately
```

The exact answer depends on current Salesforce capabilities and business requirements.

---

# 112. Advanced — Staged Packaging Strategy

For a large org:

```text
Wave 1
Shared low-risk module

Wave 2
Another foundational module

Wave 3
Feature module

Wave 4
Application module

Wave 5
Production adoption
```

This reduces the amount of unknown behavior introduced at once.

---

# 113. Advanced — Production Drift

If someone changes Production manually:

```text
Production
   |
   +--> Manual change
```

then:

```text
Package Source
      ≠
Production
```

This is configuration drift.

A mature team needs:

```text
Detection
+
Review
+
Reconciliation
+
Source update
```

---

# 114. Advanced — CI/CD Extension

Once package versioning is stable:

```text
Git Commit
    |
    v
CI
    |
    +--> Unit Tests
    +--> Static Analysis
    +--> Dependency Validation
    +--> Package Version Creation
    |
    v
Artifact
    |
    v
Staging
    |
    v
Approval
    |
    v
Production
```

This connects Part 3 naturally to Part 4.

---

# 115. Part 3 → Part 4 Connection

Part 3 gives:

```text
Packages
+
Versions
+
Dependencies
+
Promotion
```

Part 4 will extend that into:

```text
Git Branching
+
CI/CD
+
Packaging
+
Release Strategy
```

So:

```text
Part 2
Modularization
      ↓
Part 3
Packaging + Versioning
      ↓
Part 4
Git + CI/CD
```

---

# 116. Practical Workflow — Full

For a real project:

```text
1. Identify module
        ↓
2. Create package
        ↓
3. Update project config
        ↓
4. Identify package dependencies
        ↓
5. Declare dependencies
        ↓
6. Create package version
        ↓
7. Investigate failures
        ↓
8. Validate flows
        ↓
9. Use JSON diagnostics if required
        ↓
10. Query Dev Hub package errors
        ↓
11. Install in clean scratch org
        ↓
12. Install in sandbox
        ↓
13. Validate
        ↓
14. Promote
        ↓
15. Install in Production
```

---

# 117. Practical Package Checklist

Before creating a package:

```text
[ ] Module boundary is clear
[ ] Package purpose is documented
[ ] Description is meaningful
[ ] Source path is correct
[ ] Package type is correct
[ ] Metadata dependencies identified
[ ] Package dependencies identified
[ ] Flow metadata reviewed
[ ] Inactive flow versions removed where required
[ ] Empty files removed
[ ] Version naming convention defined
```

---

# 118. Package Version Checklist

Before:

```bash
force:package:version:create
```

check:

```text
[ ] Package alias correct
[ ] Dependencies declared
[ ] Dependency versions valid
[ ] Flows active
[ ] Inactive flow metadata cleaned
[ ] Empty files removed
[ ] Package directory clean
[ ] Version number valid
[ ] Version name meaningful
```

---

# 119. Validation Checklist

After package version creation:

```text
[ ] Install in clean scratch org
[ ] Test dependency installation
[ ] Run Apex tests
[ ] Test Lightning components
[ ] Test Flows
[ ] Test permissions
[ ] Test application behavior
[ ] Test integrations
[ ] Test user journeys
[ ] Check admin experience
```

---

# 120. Production Readiness Checklist

Before promotion/production:

```text
[ ] Version successfully created
[ ] Dependencies verified
[ ] Clean-org install tested
[ ] Sandbox tested
[ ] Business validation complete
[ ] Known limitations documented
[ ] Production change process ready
[ ] Source-control synchronization process ready
[ ] Rollout/rollback strategy understood
[ ] Package version identified
```

---

# 121. Errors & Gotchas

## 121.1 Package Created But Version Fails

```text
Package creation success
       ≠
Package version success
```

Check dependencies and metadata.

---

## 121.2 `Invalid type: Space__c`

Likely cause in this article's scenario:

```text
ESBaseCode references Space__c
```

but:

```text
ESBaseObjects dependency
```

was not declared.

---

## 121.3 `Variable does not exist`

May be a downstream consequence of missing schema types.

Example:

```text
Space__c missing
       ↓
Apex compilation breaks
       ↓
Variables/types appear invalid
```

Don't debug each variable error independently before checking package dependencies.

---

## 121.4 Flow Version Error

Check:

```text
Active Flow Version
```

and remove inactive flow-version metadata from package contents as appropriate.

---

## 121.5 `Cannot read property '0' of undefined`

Historical article example.

Use:

```text
-json
```

for more diagnostic detail.

Also inspect:

```text
Empty files
Flow metadata
Package structure
```

---

## 121.6 `Package2CreationRequestError` Query Fails

Likely issue:

```text
Query executed against default scratch org
```

instead of:

```text
Dev Hub
```

Use the appropriate:

```text
-u
```

target.

---

## 121.7 Too Many Version Requests

Check Dev Hub limits.

Historical command:

```bash
sfdx force:limits:api:display -u <DevHub>
```

---

## 121.8 No Package Version Alias

If the package version completes but alias isn't automatically added:

```text
Check whether -w was used
```

If necessary:

```text
Manually add package version alias
```

---

## 121.9 Production Install Blocked

Historical workflow:

```text
Package Version
      |
      v
Promote
      |
      v
Production Install
```

---

# 122. Limits / Constraints From the Source

The source explicitly discusses these historical constraints:

1. Unlocked packages were beta in Summer '18.
2. Unlocked packages could be updated but not deleted.
3. Unlocked package versions could not be deprecated at that time.
4. Lightning App Builder could not customize/edit installed Lightning Apps from unlocked packages at that time.
5. Package version creation had request limits.
6. Flow metadata could cause package-version creation failures.
7. Packaging-related queries needed to target the Dev Hub.
8. Package version aliases had special creation behavior.
9. Source/package structure still needed to be mature before production use.

**Historical caution:** These constraints describe the 2018 article and must not be assumed to remain unchanged today.

---

# 123. Best Option — Architect Decision Framework

When deciding whether to package a module, ask:

```text
1. Is the module cohesive?
2. Does it have a clear business purpose?
3. Does it have clear dependencies?
4. Can those dependencies be declared?
5. Can it be independently tested?
6. Can it be independently versioned?
7. Does it have a different release lifecycle?
8. Does it have clear ownership?
9. Will package boundaries reduce coupling?
10. Will packaging create unnecessary operational complexity?
```

---

# 124. When Should You Package?

Good candidate:

```text
Reusable
+
Cohesive
+
Stable boundary
+
Clear dependency
+
Independent lifecycle
```

Potentially poor candidate:

```text
Highly coupled
+
Rapidly changing
+
No independent lifecycle
+
Many hidden dependencies
+
Tooling limitations
```

---

# 125. Real-World Example — Salesforce Enterprise

Imagine:

```text
Enterprise Salesforce
|
+-- Customer Data
+-- Shared Services
+-- Shared UI
+-- Sales
+-- Service
+-- Reservations
+-- Analytics
+-- Bots
```

Possible package architecture:

```text
CustomerDataPackage
       |
       v
SharedServicesPackage
       |
       v
SharedUIPackage
       |
       +----------------+
       |                |
       v                v
SalesPackage      ReservationPackage
       |
       +--> Analytics
       +--> Bots
```

The actual dependency graph must be derived from the real org.

---

# 126. Real-World Example — Shared Object Package

Suppose:

```text
Customer__c
Reservation__c
Space__c
Market__c
```

are foundational.

Package:

```text
CoreData
```

Then:

```text
SharedServices
```

depends on:

```text
CoreData
```

Then:

```text
ReservationApp
```

depends on:

```text
CoreData
+
SharedServices
+
SharedUI
```

This mirrors the article's Easy Spaces architecture.

---

# 127. Real-World Example — Package Version

Suppose:

```text
CoreData
```

version:

```text
1.2.0
```

Shared Services requires:

```text
CoreData 1.2.x
```

Then:

```text
SharedServices
```

declares the appropriate package dependency.

This prevents an installation from accidentally using an incompatible foundation.

---

# 128. Interview Q&A

## Q1. What is the difference between package creation and package version creation?

**Answer:**

Package creation establishes the package definition and identifies its source path/type. Package version creation produces a versioned snapshot of the package contents and is where actual package-content and dependency issues become visible.

---

## Q2. Why did ESBaseCode package version creation fail?

**Answer:**

Because ESBaseCode referenced metadata such as `Space__c`, `Market__c`, and `Reservation__c` that belonged to ESBaseObjects, but the package dependency on ESBaseObjects had not been declared.

---

## Q3. How do you declare a package dependency?

**Answer:**

Add a `dependencies` entry to the package directory in `sfdx-project.json`, specifying the dependent package and required version.

Example:

```json
"dependencies": [
  {
    "package": "ESBaseObjects",
    "versionNumber": "0.1.0.LATEST"
  }
]
```

---

## Q4. What is `packageAliases`?

**Answer:**

It provides readable aliases for package IDs, making package commands and automation easier to understand.

---

## Q5. What is `LATEST`?

**Answer:**

In the article's historical package dependency syntax, `LATEST` could be used to reference the newest build matching the specified version line.

---

## Q6. What is `NEXT`?

**Answer:**

In the article's historical version-number format, `NEXT` could be used in the build position to represent a relative next build.

---

## Q7. Why should you use meaningful package descriptions?

**Answer:**

Because package experiments and versions can accumulate, and meaningful descriptions make it easier to identify what a package represents.

---

## Q8. Why validate in a clean scratch org?

**Answer:**

To expose hidden dependencies and confirm that the package contains everything necessary to install and function without relying on pre-existing org metadata.

---

## Q9. What should you check when package version creation fails with Flow-related problems?

**Answer:**

Check that the relevant flows have active versions and remove inactive flow-version metadata from the package directory where appropriate.

---

## Q10. Why use `-json` during troubleshooting?

**Answer:**

It can provide more detailed structured diagnostic information, including a path to the CLI file involved in an error.

---

## Q11. Why does a `Package2CreationRequestError` query need the Dev Hub?

**Answer:**

Because the packaging-related object is associated with the Dev Hub, while the CLI may default data queries to the current scratch org.

---

## Q12. What is the purpose of `-u`?

**Answer:**

It identifies the target org for the command. In this article's packaging-query scenario, it is used to target the Dev Hub rather than the default scratch org.

---

## Q13. Why is package promotion required?

**Answer:**

In the historical workflow described by the article, a package version had to be promoted before it could be installed in Production.

---

## Q14. What is the difference between package version creation and promotion?

**Answer:**

Creation produces the package version. Promotion moves that version into the state required for production installation under the historical unlocked-package workflow.

---

## Q15. What is the benefit of staged package adoption?

**Answer:**

It lets the team learn the package workflow, gather feedback, detect side effects, and improve source/package processes before affecting Production.

---

## Q16. Why is Production drift a concern?

**Answer:**

Because manual Production changes can make the deployed state differ from source control, creating inconsistency that must be detected and reconciled.

---

## Q17. Why shouldn't every module become a package?

**Answer:**

Not every module needs an independent versioning/deployment lifecycle. Some project artifacts, data, or non-deployable assets may need different handling.

---

# 129. Architect Interview — 60-Second Answer

If asked:

> **"How would you introduce unlocked packages into an existing Salesforce org?"**

A strong architecture answer:

> "I would first establish source control and modularize the metadata based on business capabilities and deployment dependencies. Then I would identify which modules actually deserve independent package lifecycles. For each package, I would create a package definition, declare explicit dependencies in `sfdx-project.json`, and create package versions only after validating metadata and dependency completeness. I would test the versions in clean scratch orgs and staging environments before promotion. I would also establish version naming, package aliases, CI validation, and a process for handling Production drift. I would adopt packaging incrementally rather than moving the entire org to packages at once, because the key risk is not package creation itself but the team's ability to operate, test, version, and maintain those packages reliably."

---

# 130. Super Advanced — Package Dependency Graph

For the article's architecture:

```text
                 ESSpaceMgmt
                /     |      \
               /      |       \
              v       v        v
     ESBaseObjects ESBaseCode ESBaseStyles
                    |          |
                    |          |
                    v          v
              ESBaseObjects ESBaseObjects
```

Flattened dependency closure:

```text
ESSpaceMgmt
 |
 +--> ESBaseStyles
 |       |
 |       +--> ESBaseCode
 |       |      |
 |       |      +--> ESBaseObjects
 |       |
 |       +--> ESBaseObjects
 |
 +--> ESBaseCode
 |       |
 |       +--> ESBaseObjects
 |
 +--> ESBaseObjects
```

---

# 131. Super Advanced — Topological Installation Order

If:

```text
A depends on B
B depends on C
```

then:

```text
C
↓
B
↓
A
```

is the logical installation sequence.

For this article:

```text
ESBaseObjects
       ↓
ESBaseCode
       ↓
ESBaseStyles
       ↓
ESSpaceMgmt
```

This is essentially a dependency graph/topological-order problem.

---

# 132. Super Advanced — Dependency Validation Before Packaging

A mature CI system can validate:

```text
Package source
      |
      v
Static dependency analysis
      |
      v
Declared dependency comparison
      |
      v
Missing dependency?
      |
   +--+--+
   |     |
  YES    NO
   |     |
   v     v
Fail    Build version
```

This can catch problems before package-version creation.

This is an architectural extension, not a direct feature described in the 2018 article.

---

# 133. Super Advanced — Hidden Dependency Detection

The article's `Space__c` failure is a classic example.

You can conceptually detect:

```text
Apex/LWC/Flow
      |
      v
Referenced metadata
      |
      v
Owning module
      |
      v
Owning package
```

Then compare:

```text
Actual dependency
        vs
Declared package dependency
```

If mismatch:

```text
CI Failure
```

---

# 134. Super Advanced — Package Boundary as a Contract

Once:

```text
ESBaseCode
```

is packaged and consumed by:

```text
ESSpaceMgmt
```

then:

```text
ESBaseCode
```

is no longer just a folder.

It becomes a versioned architectural contract.

```text
ESBaseCode
    |
    +--> version
    +--> dependency
    +--> consumers
    +--> release lifecycle
```

---

# 135. Super Advanced — Version Compatibility

Imagine:

```text
ESBaseObjects v1
ESBaseCode v1
ESSpaceMgmt v1
```

Then foundation changes:

```text
ESBaseObjects v2
```

You need to ask:

```text
Does ESBaseCode v1 still work?
Does ESBaseStyles v1 still work?
Does ESSpaceMgmt v1 still work?
```

This is why foundational packages need careful version management.

---

# 136. Super Advanced — Blast Radius Matrix

Example:

| Package | Consumers | Change Risk |
|---|---|---|
| ESBaseObjects | Code, Styles, App | High |
| ESBaseCode | Styles, App | Medium/High |
| ESBaseStyles | App | Medium |
| ESSpaceMgmt | Mainly app-specific | Lower cross-package impact |

This table is an **architectural extension**, not a scoring/ranking from the source.

---

# 137. Super Advanced — Release Train

A package ecosystem can use a release train:

```text
Foundation Release
       ↓
Shared Code Release
       ↓
Shared UI Release
       ↓
Application Release
```

This aligns package versions with dependency order.

---

# 138. Super Advanced — Independent Release

The major benefit of packages is not merely:

```text
"Separate folders"
```

but potentially:

```text
Independent lifecycle
+
Versioning
+
Dependency declaration
+
Controlled installation
```

That is what turns modularization into a release architecture.

---

# 139. Super Advanced — Package Ownership

Possible model:

```text
Core Data Team
    |
    +--> ESBaseObjects

Platform Services Team
    |
    +--> ESBaseCode

UX Platform Team
    |
    +--> ESBaseStyles

Business App Team
    |
    +--> ESSpaceMgmt
```

Then package ownership aligns with:

```text
Change responsibility
+
Review responsibility
+
Release responsibility
```

---

# 140. Super Advanced — Production Adoption Maturity

A team can move through maturity levels:

```text
Level 1
Source Control

Level 2
Metadata Modularization

Level 3
Package Creation

Level 4
Package Versioning

Level 5
Dependency Management

Level 6
Clean-Org Validation

Level 7
Staging Installation

Level 8
Promotion

Level 9
Production Package Lifecycle

Level 10
Automated CI/CD
```

This maturity ladder is an extension based on the article's staged-adoption ideas.

---

# 141. Super Advanced — Production Drift Governance

A mature package organization needs:

```text
Change Policy
      |
      +--> Who can modify Production?
      |
      +--> How are emergency changes captured?
      |
      +--> How is source updated?
      |
      +--> How is the package version reconciled?
```

Without this:

```text
Package lifecycle
      |
      X
Production drift
```

---

# 142. Super Advanced — Emergency Fix Scenario

Suppose Production has an emergency change.

```text
Production
   |
   +--> Emergency fix
```

Don't allow:

```text
Production-only permanent state
```

Instead:

```text
Emergency Production Change
        |
        v
Capture in source
        |
        v
Update package/module
        |
        v
Create corrected version
        |
        v
Future deployments remain consistent
```

The exact emergency process is organization-specific.

---

# 143. Super Advanced — Package Version as Immutable Release Artifact

Architecturally, a package version can be treated as:

```text
Source
   ↓
Build
   ↓
Version
   ↓
Test
   ↓
Promote
   ↓
Deploy
```

This is conceptually similar to a software release artifact.

---

# 144. Why Part 3 Is Important for Salesforce Architects

Part 2 asks:

> **How do I modularize?**

Part 3 asks:

> **How do I turn those modules into independently versioned/deployable units?**

That transition is critical.

```text
Metadata Architecture
        ↓
Package Architecture
        ↓
Release Architecture
```

---

# 145. Part 3 — 30-Second Revision

```text
Modules
   ↓
Package Create
   ↓
Package Version
   ↓
Dependencies
   ↓
Fix hidden dependencies
   ↓
Validate Flow metadata
   ↓
Use JSON diagnostics
   ↓
Query Dev Hub errors
   ↓
Install in clean org
   ↓
Promote
   ↓
Production
```

---

# 146. Part 3 — One-Page Cheat Sheet

## Package Creation

```bash
sfdx force:package:create \
  -n ESBaseObjects \
  -d 'Easy Spaces base objects' \
  -t Unlocked \
  -r ./es-base-objects
```

## Package Version

```bash
sfdx force:package:version:create -p "ESBaseObjects" -x
```

## Promotion

```bash
sfdx force:package:version:promote
```

## Limits

```bash
sfdx force:limits:api:display -u <DevHub>
```

## Dependency

```json
"dependencies": [
  {
    "package": "ESBaseObjects",
    "versionNumber": "0.1.0.LATEST"
  }
]
```

## Version Format

```text
Major.Minor.Patch.Build
```

Historical relative build:

```text
0.1.0.NEXT
```

## Version Alias

```text
PackageName@VersionNumber
```

mapped to the:

```text
04t...
```

subscriber package version ID.

---

# 147. Part 3 — Important Commands Map

| Goal | Historical command from article |
|---|---|
| Create package | `sfdx force:package:create` |
| Create package version | `sfdx force:package:version:create` |
| Promote version | `sfdx force:package:version:promote` |
| List package versions | `sfdx force:package:version:list` |
| Query data | `sfdx force:data:query` |
| Display limits | `sfdx force:limits:api:display` |
| Source push | `sfdx force:source:push` |

> These commands are preserved because the source article uses them. For current Salesforce CLI, verify the modern `sf` equivalents.

---

# 148. Part 3 — Final Summary

The biggest lesson:

> **Creating a package is easy compared with designing and operating a correct package ecosystem.**

The lifecycle is:

```text
MODULE
  ↓
PACKAGE
  ↓
PACKAGE VERSION
  ↓
DEPENDENCY RESOLUTION
  ↓
CLEAN-ORG TEST
  ↓
STAGING
  ↓
PROMOTION
  ↓
PRODUCTION
```

And the package itself needs:

```text
Clear Boundary
+
Clear Dependencies
+
Meaningful Versioning
+
Validation
+
Ownership
+
Source Control
```

---

# 149. Key Takeaways

1. Modules can be converted into unlocked packages.
2. `sfdx-project.json` controls package definitions and dependencies.
3. `force:package:create` creates the package definition.
4. `force:package:version:create` creates a versioned snapshot.
5. Package creation succeeding does not prove the package is structurally correct.
6. Package-version creation exposes dependency problems.
7. ESBaseCode failed because it referenced ESBaseObjects metadata without declaring the package dependency.
8. Package dependencies belong in `dependencies`.
9. `packageAliases` make package commands easier to read and automate.
10. `versionName` should be meaningful.
11. `versionNumber` follows a Major.Minor.Patch.Build structure in the source article.
12. `NEXT` was used historically in the build position.
13. `LATEST` was used historically for dependency version resolution.
14. Dependency order should reflect installation order.
15. Flows can cause package-version creation issues.
16. Verify active Flow versions.
17. Remove inactive Flow-version metadata where appropriate.
18. `-json` can provide better diagnostics.
19. Packaging-related queries may need to target the Dev Hub.
20. Use `-u` to specify the appropriate org.
21. Package version requests have limits.
22. Package version aliases can be automatically or manually managed.
23. Validate packages in clean scratch orgs or sandboxes.
24. Historical production installation required package-version promotion.
25. Unlocked packages were beta in Summer '18.
26. The article describes historical Lightning App Builder limitations for installed unlocked-package Lightning Apps.
27. Staged adoption reduces risk.
28. Source control and package architecture should evolve together.
29. Production drift needs a process.
30. Packaging is a release-management capability, not just a deployment command.

---

# 150. LinkedIn Post — Hinglish

## Salesforce Unlocked Packages — Package Create Karna Easy Hai, Dependency Design Difficult Hai 🚀

Salesforce DX modular development ke Part 3 se ek important learning:

**Package create hona ≠ package architecture correct hona.**

Flow kuch aisa hai:

```text
Module
   ↓
Package
   ↓
Package Version
   ↓
Dependencies
   ↓
Clean Org Validation
   ↓
Promotion
   ↓
Production
```

Sabse interesting scenario:

`ESBaseCode` package mein Apex classes `Space__c`, `Market__c`, `Reservation__c` jaise objects ko reference kar rahi thi.

But ye objects doosre package:

```text
ESBaseObjects
```

mein the.

Result?

```text
Package Version Creation
        ↓
Invalid type: Space__c
Invalid type: Market__c
Invalid type: Reservation__c
```

Actual problem code nahi tha.

**Problem was an undeclared package dependency.**

Solution:

```json
"dependencies": [
  {
    "package": "ESBaseObjects",
    "versionNumber": "0.1.0.LATEST"
  }
]
```

Then architecture becomes:

```text
ESBaseObjects
      ↓
ESBaseCode
      ↓
ESBaseStyles
      ↓
ESSpaceMgmt
```

Another important lesson:

**Always validate packages in a clean org.**

A package that works only because your developer org already has the required metadata may have hidden dependencies.

And package adoption should be gradual:

```text
Source Control
      ↓
Modularization
      ↓
Small Packages
      ↓
Versioning
      ↓
Dependency Management
      ↓
Scratch/Sandbox Validation
      ↓
Production
```

For Salesforce Architects, this is where metadata architecture turns into **release architecture**.

#Salesforce #SalesforceDX #UnlockedPackages #SalesforceArchitect #DevOps #CI_CD #Git #SalesforceDevelopment #Metadata #Architecture

---

# 151. Source Fidelity Note

This document intentionally preserves the source article's:

- historical commands,
- JSON examples,
- package names,
- package IDs shown in the article,
- error messages,
- Flow warnings,
- package dependency examples,
- historical limitations,
- staged adoption discussion,
- and app-development lifecycle framing.

Additional sections labelled as architect/advanced/super-advanced guidance are extensions intended to help with practical Salesforce Architect learning and interviews.

The source article is from June 19, 2018, so current Salesforce behavior must be verified separately before using any historical command or limitation in production.
