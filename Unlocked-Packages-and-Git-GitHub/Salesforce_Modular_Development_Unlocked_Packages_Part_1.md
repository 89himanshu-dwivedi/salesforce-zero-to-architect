# Working with Modular Development and Unlocked Packages — Part 1
## Salesforce DX • Modular Development • Source Control • Unlocked Packages

> **Source:** Salesforce Developer Blog — *Working with Modular Development and Unlocked Packages: Part 1*  
> **Original author:** Zayne Turner  
> **Original publication:** June 05, 2018  
>
> **Important:** Yeh article 2018 ka hai, isliye kuch commands/features historical Salesforce DX context mein hain. Concepts—**modularization, dependencies, source control, package boundaries, source of truth**—aaj bhi architect/developer thinking ke liye important hain. Jahan useful hai, maine modern development perspective bhi add kiya hai.

---

# 1. Big Picture — Salesforce DX Ne Kya Change Kiya?

Salesforce DX ne Salesforce applications ko build aur deliver karne ke liye developers ko zyada options diye.

Traditional Salesforce development mein hum aksar **org ko center of development** maante the.

Salesforce DX ke saath hum soch sakte hain:

```text
Traditional / Org-Based
        |
        v
Production Org = Main Source of Truth
        |
        +--> Sandboxes
        +--> Other Environments
```

Aur modular/package-based approach mein:

```text
Source Control
      |
      +--> Module A
      +--> Module B
      +--> Module C
      |
      v
Package Versions
      |
      +--> Dev
      +--> QA
      +--> UAT
      +--> Staging
      +--> Production
```

### Core idea

> **Org ko ek giant monolith ki tarah treat karne ke bajay, functionality ko logical modules mein divide karo.**

---

# 2. Article Series — Part 1 to Part 4

Original series mein 4 major parts the:

| Part | Focus |
|---|---|
| Part 1 | Package kya hai? Org ko modular units mein kaise break karein? |
| Part 2 | App/org metadata ko packages aur source control mein kaise organize karein? |
| Part 3 | App Builder workflows aur production mein unlocked package install karne ka impact |
| Part 4 | Git branching strategy aur CI/CD mein packaging ko kab/kaise introduce karein |

### Series ka progression

```text
Part 1
Understand Packaging
        |
        v
Identify Dependencies
        |
        v
Modularize Org
        |
        v
Part 2
Organize Metadata + Source Control
        |
        v
Part 3
Understand Package-Based Deployment
        |
        v
Part 4
Git + Branching + CI/CD
```

---

# 3. Salesforce DX Ke 3 Important Concepts

Is topic mein teen concepts ko mix nahi karna hai:

1. **Source-driven development**
2. **Modularization**
3. **Package-based development**

Ye related hain, but **same nahi hain**.

---

## 3.1 Source-Driven Development

Source-driven development ka basic idea:

> **Source control ko development ka important source of truth banana.**

Example:

```text
Developer
   |
   v
Local Salesforce DX Project
   |
   v
Git
   |
   +--> Feature Branch
   +--> Release Branch
   +--> Main
```

Is approach mein metadata ko files ke form mein manage kiya jaata hai.

Benefits:

- Version history
- Code review
- Branching
- Pull requests
- Rollback/reference
- Collaboration
- CI/CD
- Auditability

---

# 4. Modularization Kya Hai?

Modularization ka matlab:

> Large Salesforce org ko logical business/technical modules mein divide karna.

Example:

```text
Large Salesforce Org
|
+-- Sales Module
|    +-- Objects
|    +-- Apex
|    +-- Flows
|    +-- LWC
|
+-- Service Module
|    +-- Objects
|    +-- Apex
|    +-- Flows
|
+-- Customer Portal Module
|    +-- Experience Cloud
|    +-- LWC
|    +-- Apex
|
+-- Integration Module
     +-- Named Credentials
     +-- Apex
     +-- Platform Events
```

### Important

Modularization ka matlab automatically packages banana nahi hai.

Aap:

- source control use kar sakte ho,
- modules define kar sakte ho,
- Git strategy bana sakte ho,

aur packaging baad mein adopt kar sakte ho.

---

# 5. Package Development

Package-based development mein org ke customizations ko packages mein segment kiya jaata hai.

Traditional model:

```text
Production Org
      |
      +--> Everything
```

Package model:

```text
Package A
Package B
Package C
      |
      v
Different Environments
```

Package ke andar jo metadata hai, us package ko us metadata ka source of truth maana ja sakta hai.

Example:

```text
Sales Package v1.0
Sales Package v1.1
Sales Package v1.2
```

Agar v1.2 mein change hai:

```text
Create Package Version
        |
        v
Install Version
        |
        +--> Scratch Org
        +--> Sandbox
        +--> Production
```

---

# 6. Org-Based Development vs Package-Based Development

## 6.1 Org-Based Development

Article ke context mein traditional development ko **org-based development** kaha gaya hai.

Production org complete customization ka ultimate structure/source of truth hota hai.

```text
                 Production
                     |
       +-------------+-------------+
       |             |             |
      Dev           QA          Staging
```

Even if source control exist karta ho, historically production org ke paas complete customization hoti thi.

---

## 6.2 Package-Based Development

Package development mein org ko modules mein segment kiya jaata hai.

```text
Package A
Package B
Package C
    |
    v
Environments
```

Example:

```text
Customer Portal Package
       |
       +--> Dev
       +--> QA
       +--> UAT
       +--> Production
```

Agar package metadata update hota hai:

```text
Package v1
   |
   | Change
   v
Package v2
   |
   +--> QA
   +--> UAT
   +--> Production
```

---

# 7. Image 1 — Non-Packaged vs Packaged Development

Provided architecture diagram mein do approaches compare ki gayi hain.

## Top: Non-Packaged Deployments / Org-Based Development

```text
Dev1 ----\
Dev2 -----+--> QA --> Integration Testing --> UAT --> Staging --> Production
Dev3 ----/                                     
                                                   |
                                                   +--> Hotfix
                                                   +--> Prod Support
                                                   +--> Training
```

Yahan environments ek doosre ko metadata/deployment ke through update kar rahe hain.

Production environment mein multiple metadata groups combined hain.

### Main characteristic

**Org/environment itself is the center of deployment.**

---

## Bottom: Packaged Deployments / Source-Based Development

```text
 Dev1
 Dev2
 Dev3
   |
   v
Source Control
   |
   +--> QA
   +--> Integration Testing
   +--> UAT
   +--> Staging
   +--> Production
   +--> Hotfix
   +--> Prod Support
   +--> Training
```

Yahan source control center mein hai.

### Main characteristic

**Source/package artifacts become the controlled delivery mechanism.**

---

# 8. Golden Rule — Source, Modularization, Packaging Ko Mix Mat Karo

Ye architect interview ka very important point hai.

```text
Source Control
      |
      |  stores/manages
      v
Source Representation
      |
      | can be organized into
      v
Modules
      |
      | may be delivered as
      v
Packages
```

### Meaning

Aap:

- source control use kar sakte ho without packages,
- modularization kar sakte ho without immediately packaging everything,
- packaging adopt kar sakte ho after source-control discipline establish karne ke baad.

---

# 9. Kya Har Cheez Package Mein Dalni Zaroori Hai?

**NO.**

Article ka important point:

> Aapko org ka har single piece package mein nahi daalna hai.

Aap gradually start kar sakte ho.

Example:

```text
Org
|
+-- Sales Core
|      -> Package
|
+-- Service
|      -> Package
|
+-- Legacy Area
|      -> Existing Deployment Strategy
|
+-- Temporary Configuration
|      -> Existing Process
```

Aap kuch areas ko packages se manage kar sakte ho aur kuch ko existing deployment mechanisms se.

---

# 10. Start Small

Packaging adopt karte waqt poore org ko ek saath package karne ki koshish mat karo.

Original article specifically warn karta hai:

> **Do not try to put everything in your org into a single package.**

Aur is point ko strongly repeat kiya gaya hai.

### Better approach

Ek small, understandable application/module choose karo.

Example:

```text
Huge Org
  |
  +-- Sales
  +-- Service
  +-- Finance
  +-- HR
  +-- Integration
  +-- Legacy
  |
  v
Start with one small application
```

### Good candidate

- Standalone Lightning App
- Small business application
- Small feature set
- Limited number of objects
- Manageable Apex/LWC/Flow dependencies

### Bad first candidate

```text
100 tabs
500 Visualforce pages
Thousands of dependencies
Many legacy integrations
```

Aise component se start karoge to dependency analysis unnecessarily difficult ho jayega.

---

# 11. Managed Package vs Unlocked Package vs Unpackaged Metadata

Architect ke liye ye distinction important hai.

| Type | Basic idea |
|---|---|
| Managed Package | Strong packaging boundaries; package developer controls many aspects |
| Unlocked Package | Modular/package-based development with more flexibility |
| Unpackaged Metadata | Direct org/source-based deployment outside package boundary |

---

# 12. Managed Package

Managed package mein package developer ke metadata par comparatively strong control hota hai.

Installed metadata ko customer/admin har situation mein freely modify nahi kar sakta.

Exact behavior metadata type aur package design par depend karta hai.

### Typical thinking

```text
ISV / Package Developer
        |
        v
Managed Package
        |
        v
Subscriber Org
```

---

# 13. Unlocked Package

Unlocked package ka purpose modular development ko support karna hai.

Article ke historical context mein important point:

> Unlocked package metadata ko managed package ki tarah tightly locked nahi treat kiya jaata.

Original example:

Agar object unlocked package mein hai aur admin field ka:

- Help Text
- Label

update karna chahta hai, to such changes may be possible.

### But very important

Production mein directly kiya gaya change automatically package version mein merge nahi hota.

```text
Package v1
    |
    v
Production
    |
    +--> Admin manually changes metadata
    |
    X
    |
    +--> Automatically becomes Package v2? NO
```

Aapko production change ko source/package representation mein consciously capture karna padega.

---

# 14. Production Drift

Ye modern development mein bahut important concept hai.

Agar source/package mein:

```text
Field Label = Customer Name
```

Production mein manually change:

```text
Field Label = Client Name
```

To:

```text
Source
  |
  | Customer Name
  |
Production
  |
  | Client Name
```

Ab dono diverge ho gaye.

Isko **configuration drift / source drift / org drift** ke context mein discuss kiya ja sakta hai.

### Architect lesson

> Production should not become an uncontrolled second source of truth.

Agar emergency production change karna pade:

```text
Emergency Change
      |
      v
Production
      |
      v
Capture Change
      |
      v
Source Control
      |
      v
Package / Deployment Artifact
```

---

# 15. Why Modularization Matters

Modularization ka biggest benefit:

> **Change impact ko samajhna easier hota hai.**

Example:

```text
Customer Object
      |
      +--> Apex Class
      +--> Flow
      +--> LWC
      +--> Permission Set
      +--> Layout
      +--> Integration
      +--> Reports
```

Agar Customer object mein change kiya:

```text
Customer Object Change
        |
        +--> What breaks?
        +--> What depends on it?
        +--> What must be deployed?
        +--> What tests are required?
```

Ye architecture thinking ka core hai.

---

# 16. Dependencies Kya Hain?

Simple definition:

> Jab ek metadata component doosre metadata component ko reference karta hai, dependency exist karti hai.

Example:

```text
Page Layout
    |
    +--> Object
    +--> Fields
    +--> Related Lists
    +--> Actions
```

Page Layout depends on these components.

---

# 17. Dependency Chain

Simple dependency:

```text
Page Layout
     |
     +--> Account Object
     +--> Account.Name
     +--> Account.Owner
```

Complex dependency:

```text
Lightning Page
      |
      +--> Lightning Component
                |
                +--> Apex Controller
                         |
                         +--> Custom Object
                         |
                         +--> Custom Field
                         |
                         +--> Permission
```

Aur chain further continue ho sakti hai.

---

# 18. Example — Visualforce Dependency

```text
Page Layout
     |
     v
Visualforce Page
     |
     v
Apex Controller
     |
     +--> Custom Object
     +--> Custom Fields
     +--> Other Apex Classes
     +--> Metadata
```

Isliye ek small UI change bhi multiple metadata components ko involve kar sakta hai.

---

# 19. Dependency Graph Socho

Architect level par dependencies ko graph ki tarah sochna useful hai.

```text
              Account
             /   |   \
            /    |    \
       Layout   Flow   Apex
         |       |      |
         |       |      +--> Integration
         |       |
         +-------+-------> Permission
```

### Why important?

Deployment se pehle:

```text
What am I changing?
        |
        v
What depends on it?
        |
        v
What does it depend on?
        |
        v
What should move together?
```

---

# 20. Dependency API — Historical Context

Original article mein Summer '18 ke liye **Dependency API** pilot ka mention tha.

Purpose:

> Metadata dependencies ko identify karne mein help karna.

Historical article ke context mein ye API pilot stage mein thi.

### Learning point

Salesforce platform continuously dependency discovery capabilities improve karta raha hai.

Architect ko sirf UI dependency viewer par blindly depend nahi karna chahiye.

---

# 21. Unmanaged Package Ko Dependency Discovery Tool Ki Tarah Use Karna

Article ka practical technique:

> Ek small piece of metadata select karo aur unmanaged package mein add karo.

Salesforce kuch dependencies automatically package mein pull kar sakta hai.

```text
Select Metadata
      |
      v
Unmanaged Package
      |
      v
Salesforce identifies some dependencies
      |
      v
Review Package Components
```

Ye dependency discovery ke liye useful experiment hai.

---

# 22. Important Warning — Automatic Dependency Discovery Is Not Complete

Bahut important:

> Package mein jo kuch automatically pull hua hai, iska matlab ye nahi ki org ki every dependency discover ho gayi.

Dependency screen bhi complete picture nahi dikhata.

Original article specifically warns:

> **Blank dependency screen ka matlab zero dependencies nahi hai.**

---

# 23. Image 2 — Package Manager

Provided screenshot mein:

```text
Setup
  |
  v
Package Manager
  |
  v
Package: SFDX Extract
```

Package details mein:

- Package Name
- Language
- Type = Unmanaged
- Components
- Versions
- Add
- View Dependencies

dikh rahe hain.

### Components table

Metadata components ke saath:

- Name
- Parent Object
- Type
- Included By
- Owned By

jaise columns dikhte hain.

### "Included By" ka meaning

Ye help karta hai samajhne mein ki component package mein kis dependency/reference ki wajah se include hua.

---

# 24. Image 3 — Dependency Information

Provided dependency screenshot mein different dependency categories hain.

Examples:

### Organization-level Feature Dependencies

```text
Flow
  |
  v
OfficeDesigner
```

### Object Operational Scope

Objects ke liye operations jaise:

- Insert
- Update
- Delete
- Upsert
- Undelete
- Merge

dikhaye ja sakte hain.

Example:

```text
Office
Office Space
Account
Contact
User
```

Aur "Referenced By" section batata hai ki kaunse components/code in objects ko reference kar rahe hain.

---

# 25. Apex Class, Trigger and Page References

Dependency screen mein Apex references bhi dikh sakte hain.

Example:

```text
ChangePasswordController
      |
      +--> ChangePassword
      +--> ChangePasswordControllerTest
```

Another example:

```text
OfficeDataTableController
      |
      +--> OfficeDataTableControllerTest
```

Isse developer ko impact analysis mein help mil sakti hai.

---

# 26. But Dependency Screen Has Limitations

Ye point exam/interview mein bolna useful hai:

> Dependency UI is a **helping tool**, not a complete dependency graph.

Reasons:

- All metadata types necessarily appear nahi karte.
- Indirect dependencies miss ho sakti hain.
- Dynamic references harder ho sakte hain.
- Apex string-based references detect karna difficult ho sakta hai.
- External system dependencies package metadata se fully visible nahi hote.
- Business/process dependencies technical metadata dependency se different hote hain.

---

# 27. "Cruft" Kya Hai?

Cruft = unnecessary metadata jo dependency chain ke through package mein aa gaya ho, but actual application ke liye relevant na ho.

Example:

```text
Lightning App
      |
      +--> Required metadata
      |
      +--> Unrelated Account metadata
      +--> Unrelated Contact metadata
      +--> Unrelated User metadata
```

Unrelated components = potential cruft.

### Problem

Agar package mein unnecessary metadata aa gaya:

- package boundary dirty ho sakti hai,
- ownership unclear ho sakti hai,
- deployment complexity badh sakti hai,
- future maintenance difficult ho sakti hai.

---

# 28. Example From Article — Lightning App

Author ne demo org mein ek simple Lightning app ko first package component banaya.

Salesforce ne automatically kaafi metadata package mein pull kiya.

But gaps bhi mile.

### Missing examples

- Permission Set
- Custom Objects
- Apex Test Classes

In components ko manually add karna pada.

### Lesson

> Automated dependency collection ke baad **human architectural review** still required hai.

---

# 29. Dependency Discovery = Automation + Human Knowledge

Best mental model:

```text
Salesforce Dependency Discovery
             |
             v
      Automated Discovery
             +
      Developer Knowledge
             +
      Trial & Error
             |
             v
      Clean Module Boundary
```

Sirf tool par depend mat karo.

---

# 30. Trial-and-Error Dependency Discovery

Agar org unfamiliar hai:

```text
Create Package
      |
      v
Deploy to another Org
      |
      v
Deployment Failure?
      |
      +-- Yes --> Analyze Error
      |             |
      |             v
      |       Add/Remove Metadata
      |             |
      |             v
      |          Retry
      |
      +-- No --> Candidate Module
```

Ye process dependency discovery mein practical hai.

---

# 31. Why Start Small?

Suppose:

### Option A

```text
Entire Org
1000s of components
```

Dependency analysis:

```text
Very difficult
```

### Option B

```text
Small App
50–100 related components
```

Dependency analysis:

```text
Manageable
```

Isliye:

> **Small module first → learn → refine → expand.**

---

# 32. Source Control Ki Taraf Move

Unmanaged package assemble karne ke baad article Salesforce CLI use karta hai.

Historical command:

```bash
sfdx force:mdapi:retrieve -p 'SFDX Extract' -s -r ./demoOrgExtract -u demoOrg
```

### Parameters

```text
-p
```

Package name.

Example:

```text
SFDX Extract
```

Because package name mein space tha, single quotes use kiye gaye.

---

```text
-s
```

Single package / specified package retrieval context.

---

```text
-r ./demoOrgExtract
```

Retrieved ZIP/content ko specified directory mein place karne ke liye.

---

```text
-u demoOrg
```

Target Salesforce org alias/username.

---

# 33. Historical Command vs Modern CLI

Original article 2018 ka hai, isliye `sfdx force:*` commands historical Salesforce CLI syntax hain.

Modern Salesforce CLI mein commands generally `sf` style mein available hote hain.

Concept same hai:

```text
Salesforce Org
     |
     v
Retrieve Metadata
     |
     v
Local Source
     |
     v
Git
```

### Development lesson

Command syntax evolve ho sakta hai.

**Architecture concept ko command syntax se alag samjho.**

---

# 34. Metadata API Format vs Salesforce DX Source Format

Original process:

```text
Org
 |
 v
Metadata API Retrieve
 |
 v
ZIP / Metadata API Format
 |
 v
Unzip
 |
 v
Convert
 |
 v
Salesforce DX Source Format
```

Why?

Salesforce DX source format ko source control ke liye better organize kiya ja sakta hai.

---

# 35. Metadata API Format

Traditional Metadata API representation deployment/retrieval ke liye useful hai.

Example concept:

```text
unpackaged/
   package.xml
   classes/
   objects/
   layouts/
```

But source-driven development ke liye Salesforce DX source format more modular structure provide karta hai.

---

# 36. Salesforce DX Source Format

Conceptually:

```text
force-app/
   main/
      default/
         classes/
         objects/
         lwc/
         flows/
         permissionsets/
         layouts/
```

Modern project structure commonly isi type ki source representation use karti hai.

---

# 37. Original Article Ka Conversion Process

Article ke according:

### Step 1

Temporary Salesforce DX project create karo.

Historical command concept:

```bash
sfdx force:project:create
```

### Step 2

Metadata retrieve karo.

### Step 3

Metadata API format ko Salesforce DX source format mein convert karo.

Historical command concept:

```bash
sfdx force:mdapi:convert
```

`-r` ko `unpackaged` folder ke saath use kiya gaya.

---

# 38. End-to-End Flow From Article

```text
Salesforce Org
      |
      v
Select Small Application
      |
      v
Create Unmanaged Package
      |
      v
Review Automatically Included Dependencies
      |
      v
Identify Missing Metadata
      |
      v
Add Missing Components
      |
      v
Identify Cruft
      |
      v
Retrieve Package Using CLI
      |
      v
Metadata API Format
      |
      v
Convert to Salesforce DX Source Format
      |
      v
Clean Up Metadata
      |
      v
Organize Source Control
```

---

# 39. What Was Still Left To Do?

Article ke end par source local machine par aa gaya tha.

But work complete nahi tha.

Author ke paas:

```text
Local Source
```

tha, lekin:

- extra metadata clean karna tha,
- cruft remove/understand karna tha,
- source organization improve karna tha,
- modules ko better define karna tha,
- source control mein properly organize karna tha.

Ye next article ka focus tha.

---

# 40. Development Perspective — How I Would Apply This Today

Aaj real project mein main process ko roughly is tarah approach karunga:

```text
1. Business Capability Identify
          |
          v
2. Candidate Module Define
          |
          v
3. Dependency Map
          |
          v
4. Source Control Structure
          |
          v
5. Package Boundary Decide
          |
          v
6. CI Validation
          |
          v
7. Deploy to Lower Environment
          |
          v
8. Integration Testing
          |
          v
9. UAT
          |
          v
10. Production
```

---

# 41. Business Capability First, Metadata Second

Architecting mein ek important improvement:

**Package ko sirf metadata type ke basis par define mat karo.**

Bad thinking:

```text
All Apex = Package A
All Objects = Package B
All Flows = Package C
```

Better thinking:

```text
Business Capability
      |
      +--> Objects
      +--> Apex
      +--> Flow
      +--> LWC
      +--> Permission Set
      +--> Custom Metadata
      +--> Tests
```

Example:

```text
Customer Onboarding Module
```

could contain:

```text
Customer Object
Onboarding Flow
OnboardingService.cls
CustomerOnboarding LWC
Permission Set
Custom Metadata
Tests
```

---

# 42. Package Boundary Should Follow Cohesion

Good module:

```text
High Cohesion
```

Meaning components strongly belong together.

Bad module:

```text
Random Components
```

### Architect question

Ask:

> "Agar mujhe ye business capability independently develop, test, deploy aur maintain karni ho, to kya ye components logically saath move karne chahiye?"

Agar answer yes hai, package/module boundary strong candidate ho sakti hai.

---

# 43. Coupling Also Matters

Cohesion ke saath coupling dekho.

```text
Module A <----> Module B
```

Agar A aur B extremely tightly coupled hain, unhe separate packages karna unnecessarily difficult ho sakta hai.

Ideal:

```text
Module A
   |
   | Stable Contract
   v
Module B
```

Avoid:

```text
A --> B --> C --> A
```

Circular dependency package architecture ko painful bana sakti hai.

---

# 44. Dependency Types

Salesforce dependency ko multiple levels par dekho.

## 44.1 Metadata Dependency

```text
Layout --> Field
```

## 44.2 Code Dependency

```text
LWC --> Apex
```

## 44.3 Configuration Dependency

```text
Flow --> Custom Metadata
```

## 44.4 Security Dependency

```text
Feature --> Permission Set
```

## 44.5 Data Dependency

```text
Order --> Account
```

## 44.6 Integration Dependency

```text
Salesforce --> MuleSoft --> ERP
```

## 44.7 Operational Dependency

```text
Feature
  |
  +--> Monitoring
  +--> Support
  +--> Deployment
```

---

# 45. Static vs Dynamic Dependencies

### Static

Code/metadata directly reference karta hai.

```apex
Account a = [SELECT Id, Name FROM Account LIMIT 1];
```

Dependency relatively obvious hai.

### Dynamic

Example:

```apex
String objectName = 'Account';
```

or dynamic metadata/API configuration.

Dependency discover karna harder ho sakta hai.

### Architect Lesson

> Automated dependency discovery ke limitations ko always consider karo.

---

# 46. Production Change Control

Unlocked package ka flexibility point useful hai, but direct production changes risky ho sakte hain.

Recommended workflow:

```text
Developer Change
      |
      v
Source Control
      |
      v
Review
      |
      v
CI Validation
      |
      v
Package / Deployment Artifact
      |
      v
QA
      |
      v
UAT
      |
      v
Production
```

Emergency:

```text
Production Hotfix
      |
      v
Immediately document
      |
      v
Back-port to Source
      |
      v
Update Package/Source
      |
      v
Validate
```

---

# 47. CI/CD Perspective

Packaging ka maximum benefit tab milta hai jab CI/CD ke saath integrate karo.

Example:

```text
Developer
   |
   v
Feature Branch
   |
   v
Pull Request
   |
   v
Automated Validation
   |
   +--> Apex Tests
   +--> Static Analysis
   +--> Metadata Validation
   +--> Dependency Checks
   |
   v
Merge
   |
   v
Package Version
   |
   v
Deploy
```

---

# 48. Source Control Strategy

Basic Git model:

```text
main
 |
 +-- release/*
 |
 +-- feature/*
 |
 +-- hotfix/*
```

Example:

```text
main
 |
 +-- release/2026.10
 |       |
 |       +-- feature/customer-onboarding
 |       +-- feature/new-approval
 |
 +-- hotfix/login-failure
```

### Principle

Production-ready code should remain controlled and traceable.

---

# 49. What Should Go Into Source Control?

Generally, source-controlled Salesforce development should include relevant deployable metadata/source artifacts.

Examples:

- Apex
- LWC
- Objects
- Fields
- Flows
- Permission Sets
- Custom Metadata Types
- Custom Metadata Records where appropriate
- Layouts
- Applications
- Relevant configuration

But remember:

> Metadata that is environment-specific or generated/runtime data may need a different strategy.

---

# 50. Source Control Is Not Data Backup

Important distinction:

```text
Source Control
     |
     +--> Metadata / Source
```

versus:

```text
Data
     |
     +--> Accounts
     +--> Contacts
     +--> Business Transactions
```

Git/source control is not automatically a complete Salesforce data backup strategy.

---

# 51. Package Versioning

Package development introduces version thinking.

Example:

```text
Customer Module
|
+-- v1.0
+-- v1.1
+-- v1.2
+-- v2.0
```

A version should represent a controlled set of changes.

Good versioning gives:

- traceability,
- reproducibility,
- controlled deployment,
- easier rollback strategy,
- release visibility.

---

# 52. Versioning ≠ Rollback Everything Automatically

Important architect point:

A package version does not mean every production problem can always be solved by blindly installing an older version.

Why?

Because production may have:

- data changes,
- configuration changes,
- dependent packages,
- database migrations,
- external integrations,
- security changes.

Rollback strategy must be designed.

---

# 53. Data Migration Consideration

Suppose package v1:

```text
Customer__c
```

v2 introduces:

```text
Customer_Type__c
```

But existing data needs values.

Deployment becomes:

```text
Deploy Metadata
      |
      v
Data Migration
      |
      v
Validation
      |
      v
Activate Feature
```

So package architecture and data migration architecture must be considered together.

---

# 54. Integration Dependencies

A package can contain Salesforce-side integration metadata/code, but the full integration landscape may extend outside Salesforce.

Example:

```text
Salesforce Package
      |
      +--> Named Credential
      +--> Apex
      +--> Platform Event
      |
      v
MuleSoft
      |
      v
ERP
```

The package cannot magically capture every external-system dependency.

Architect should maintain an external dependency map.

---

# 55. Security Dependencies

Suppose feature requires:

```text
Object
Field
Apex
Flow
LWC
Permission Set
```

A deployment that moves only functional metadata but misses security configuration can result in:

```text
Feature deployed
       |
       v
User cannot access it
```

Therefore:

> **Security metadata is part of dependency analysis.**

---

# 56. Testing Dependencies

Do not forget tests.

Article example itself mentions Apex test classes being missing from the automatically assembled package.

Good module:

```text
Feature
 |
 +--> Production Code
 +--> Configuration
 +--> Security
 +--> Tests
```

### Architect rule

> Testability should be part of module design, not an afterthought.

---

# 57. Deployment Dependency vs Runtime Dependency

These are different.

### Deployment dependency

Component must exist before another component can deploy.

```text
Field
  |
  v
Flow
```

### Runtime dependency

Feature depends on another system during execution.

```text
Salesforce Flow
     |
     v
Integration
     |
     v
ERP
```

Both should be documented.

---

# 58. Package Dependency vs Business Dependency

Example:

```text
Technical:
Package A --> Package B
```

But business-wise:

```text
Sales
   |
   +--> Customer Service
```

Business dependency may exist even when no direct metadata reference exists.

Architect should consider both.

---

# 59. Anti-Pattern — One Giant Package

```text
One Org
   |
   v
One Giant Package
   |
   +--> Everything
```

Problems:

- Large blast radius
- Harder releases
- Harder ownership
- More coupling
- More dependency complexity
- Harder testing
- Harder troubleshooting

---

# 60. Anti-Pattern — One Package Per Metadata Type

```text
Apex Package
Object Package
Flow Package
LWC Package
Permission Package
```

This can create artificial dependencies.

Example:

```text
Flow Package
   |
   +--> Object Package
   |
   +--> Apex Package
   |
   +--> Permission Package
```

Abstraction technically possible hai, but business capability cohesion weak ho sakti hai.

---

# 61. Better Mental Model

Think:

```text
Business Capability
       |
       v
Logical Module
       |
       +--> Metadata
       +--> Code
       +--> Security
       +--> Tests
       +--> Configuration
       |
       v
Package Boundary
       |
       v
Versioned Artifact
```

---

# 62. Practical Example — Customer Onboarding

Requirement:

> Customer onboarding ko independently develop and release karna hai.

Possible module:

```text
Customer Onboarding
|
+-- Customer__c
+-- Onboarding_Status__c
+-- Onboarding Flow
+-- CustomerOnboardingController.cls
+-- CustomerOnboarding.js
+-- Permission Set
+-- Custom Metadata
+-- Apex Tests
```

Deployment flow:

```text
Feature Branch
      |
      v
Source Control
      |
      v
Validation
      |
      v
Package Version
      |
      v
QA
      |
      v
UAT
      |
      v
Production
```

---

# 63. Practical Example — Salesforce Architect Interview

Interviewer:

> "How would you modularize a large Salesforce org?"

Strong answer structure:

### Step 1 — Understand business capabilities

```text
Sales
Service
Customer Onboarding
Billing
Integration
```

### Step 2 — Map metadata

```text
Objects
Flows
Apex
LWC
Security
Tests
```

### Step 3 — Map dependencies

```text
Direct
Indirect
Security
Integration
Data
Runtime
```

### Step 4 — Define boundaries

Focus on:

- high cohesion,
- manageable coupling,
- clear ownership,
- independent lifecycle where possible.

### Step 5 — Source control

Use Git-based workflow.

### Step 6 — Package selectively

Don't package everything just for the sake of packaging.

### Step 7 — CI/CD

Automated validation and controlled promotion.

---

# 64. Interview Q&A

## Q1. Is source control the same as packaging?

**Answer:** No.

Source control manages source/history/collaboration.

Packaging defines a deployable/modular boundary and versioned artifact.

---

## Q2. Does modularization require unlocked packages?

**Answer:** No.

You can modularize your source and development process without immediately converting every module into a package.

---

## Q3. Should we package the entire org?

**Answer:** Not necessarily.

Start with a well-understood, cohesive module and evolve the strategy.

---

## Q4. Does Salesforce automatically identify every dependency?

**Answer:** No.

Dependency discovery tools can help, but not every dependency is necessarily visible. Human analysis and testing remain important.

---

## Q5. What happens if production metadata is changed after an unlocked package is installed?

The production change does not automatically become a new package version. The team needs a controlled process to capture that change back into source/package development.

---

## Q6. Why are Apex tests important in modular development?

Because the module must be independently validated. Tests are part of the delivery lifecycle even when they aren't automatically included by a particular dependency-discovery mechanism.

---

## Q7. What is cruft?

Unnecessary metadata that gets included in a package/module but does not meaningfully belong to the intended capability.

---

## Q8. Why start with a small module?

Because dependency analysis becomes manageable, mistakes are easier to identify, and the team can learn the packaging workflow before scaling it.

---

# 65. Errors & Gotchas

## Gotcha 1 — Blank Dependency Screen

```text
No dependency displayed
       !=
No dependency exists
```

---

## Gotcha 2 — Automatic Inclusion Is Not Complete

Some required metadata may still need manual addition.

---

## Gotcha 3 — Production Changes Don't Automatically Flow Back

```text
Production Change
       X
       |
       X
Package Version
```

You need a controlled synchronization process.

---

## Gotcha 4 — Don't Trust Package Contents Blindly

Review:

```text
Required
vs
Cruft
vs
Missing
```

---

## Gotcha 5 — Business Dependency May Not Be Metadata Dependency

A business process can depend on another system/team/process even when no direct Salesforce metadata reference exists.

---

## Gotcha 6 — Security Can Be Missed

Functional metadata without Permission Sets/Profile/FLS/access design can create deployment or runtime issues.

---

## Gotcha 7 — Data Migration Is Separate

Metadata deployment does not automatically solve all data migration requirements.

---

# 66. Limits / Constraints

Modular packaging is powerful, but it doesn't remove Salesforce platform constraints.

Consider:

- Metadata dependencies
- Package dependency rules
- API version compatibility
- Apex governor limits
- Deployment limits
- Test requirements
- Data migration
- Security
- Integration dependencies
- Environment-specific configuration
- External systems
- Legacy metadata
- Circular dependencies

---

# 67. Best Option — Decision Framework

Instead of asking:

> "Should we use packages?"

Ask:

### 1. What is the business capability?

### 2. Can we define a clean boundary?

### 3. What are the dependencies?

### 4. Is ownership clear?

### 5. Does it need an independent release lifecycle?

### 6. Can it be tested independently?

### 7. Can it be deployed independently?

### 8. What happens to data during deployment?

### 9. What external systems are involved?

### 10. How will production drift be controlled?

---

# 68. When Packaging Is Useful

Packaging becomes particularly useful when:

- org is large,
- multiple teams work independently,
- components have clear ownership,
- releases need better boundaries,
- source control is mature,
- CI/CD exists,
- reusable modules are valuable,
- deployment traceability matters.

---

# 69. When to Be Careful

Be cautious when:

- org is highly coupled,
- legacy metadata is everywhere,
- dependencies are poorly understood,
- teams make frequent direct production changes,
- source control is immature,
- deployment process is mostly manual,
- package boundaries are artificial.

---

# 70. Development Maturity Model

A useful maturity path:

```text
Level 1
Manual / Org-Based
      |
      v
Level 2
Source Control
      |
      v
Level 3
Modularization
      |
      v
Level 4
Automated Validation
      |
      v
Level 5
Package-Based Delivery
      |
      v
Level 6
CI/CD + Governance
      |
      v
Level 7
Independent Business Capability Delivery
```

Important:

> Package adoption should follow engineering maturity—not replace it.

---

# 71. Architect's Dependency Checklist

Before splitting a module, ask:

```text
[ ] Which objects does it use?
[ ] Which fields does it use?
[ ] Which Apex classes?
[ ] Which LWC/Aura components?
[ ] Which Flows?
[ ] Which Validation Rules?
[ ] Which Permission Sets?
[ ] Which Layouts?
[ ] Which Record Types?
[ ] Which Custom Metadata?
[ ] Which Custom Settings?
[ ] Which integrations?
[ ] Which external systems?
[ ] Which tests?
[ ] Which data?
[ ] Which deployment sequence?
[ ] Which teams own it?
[ ] Which production changes are allowed?
```

---

# 72. Source Control Checklist

```text
[ ] Git repository
[ ] Branching strategy
[ ] Pull request process
[ ] Code review
[ ] Metadata validation
[ ] Apex tests
[ ] Static analysis
[ ] Deployment validation
[ ] Package/version strategy
[ ] Release tagging
[ ] Audit trail
[ ] Production hotfix process
```

---

# 73. Production Drift Prevention

Recommended governance:

```text
Developer
    |
    v
Source Control
    |
    v
PR / Review
    |
    v
CI
    |
    v
Package / Deployment
    |
    v
Environment
```

Avoid:

```text
Developer
    |
    v
Production
    |
    X
Source Control
```

If direct production change is unavoidable:

```text
Production Hotfix
      |
      v
Document
      |
      v
Reproduce in Source
      |
      v
Commit
      |
      v
Future Release
```

---

# 74. Real-World Scenario

## Situation

Company has:

```text
Salesforce Org
|
+-- Sales
+-- Service
+-- Partner Portal
+-- Billing
+-- Integration
+-- Legacy
```

100+ developers are working across teams.

Current problem:

```text
One change
   |
   v
Multiple teams affected
   |
   v
Deployment conflicts
   |
   v
Release delays
```

### Architecture response

First:

```text
Source Control
```

Then:

```text
Dependency Mapping
```

Then:

```text
Business Capability Modules
```

Then:

```text
Package Boundaries
```

Then:

```text
CI/CD
```

---

# 75. What Not To Do

Don't do this:

```text
"Salesforce has unlocked packages,
so let's package everything tomorrow."
```

Instead:

```text
Understand Org
      |
      v
Understand Dependencies
      |
      v
Improve Source Control
      |
      v
Define Modules
      |
      v
Pilot One Module
      |
      v
Measure
      |
      v
Scale
```

---

# 76. Super-Advanced — Package Boundary as Architecture Boundary

Package boundary sirf deployment concern nahi hai.

It can become an architectural boundary.

Example:

```text
+-----------------------------+
| Customer Onboarding Module  |
|                             |
| Object                      |
| Apex                        |
| Flow                        |
| LWC                         |
| Security                    |
| Tests                       |
+-----------------------------+
              |
              | Contract
              v
+-----------------------------+
| Customer Service Module     |
+-----------------------------+
```

This encourages:

- ownership,
- clear interfaces,
- controlled coupling,
- independent lifecycle.

---

# 77. Super-Advanced — Think in Contracts

Instead of allowing every module to directly access everything:

```text
Module A
  |
  +--> Internal details of B
  +--> Internal details of C
```

Prefer:

```text
Module A
   |
   v
Stable Contract
   |
   v
Module B
```

Contract can conceptually be:

- Apex service interface
- Platform Event
- API
- well-defined metadata/configuration boundary
- integration contract

This is a more scalable architecture mindset.

---

# 78. Super-Advanced — Dependency Direction

Try to make dependency direction intentional.

Bad:

```text
A --> B
B --> C
C --> A
```

Good:

```text
Common/Platform Layer
       ^
       |
Business Modules
       |
       v
Integration Layer
```

Exact architecture depends on the system, but **dependency direction should be designed**, not accidental.

---

# 79. Super-Advanced — Blast Radius

Before deployment ask:

> "Agar main ye component change karun, kitna system impact hoga?"

```text
Small Module
   |
   v
Small Blast Radius
```

versus:

```text
Shared Core Object
   |
   +--> 30 Flows
   +--> 50 Apex Classes
   +--> 20 LWCs
   +--> 10 Integrations
   |
   v
Large Blast Radius
```

This helps prioritize testing and deployment planning.

---

# 80. Super-Advanced — Ownership

A package/module should ideally have clear ownership.

Example:

```text
Customer Onboarding
       |
       v
Customer Platform Team
```

Ownership includes:

- source code,
- releases,
- defects,
- dependencies,
- security,
- documentation,
- operational support.

---

# 81. Super-Advanced — Observability

Production support is also part of module architecture.

For a critical module:

```text
Module
 |
 +--> Logs
 +--> Errors
 +--> Monitoring
 +--> Alerts
 +--> Support Runbook
```

A deployable module should ideally be an **operationally understandable unit**, not just a metadata folder.

---

# 82. Simple Comparison

| Area | Org-Based | Source/Package-Oriented |
|---|---|---|
| Source of truth | Historically org-centric | Source/package-centric |
| Modularity | Usually weaker | Explicit |
| Versioning | Deployment-centric | Source/package versioning |
| Dependency thinking | Often reactive | More deliberate |
| Git integration | Optional/partial | Core practice |
| CI/CD | Can be added | Strong fit |
| Independent releases | Harder | Easier when boundaries are good |
| Production drift | Common risk | Must be governed |
| Architecture focus | Org | Capability/module |

---

# 83. One-Line Definitions For Interview

### Org-Based Development

> Org itself is the primary representation/source of the complete customization.

### Source-Driven Development

> Source control becomes a first-class representation of Salesforce metadata and development history.

### Modularization

> Large solution ko logically cohesive modules mein divide karna.

### Package-Based Development

> Modules ko versioned package artifacts ke through deliver/manage karna.

### Dependency

> Ek metadata/code component ka doosre component par reference ya reliance.

### Cruft

> Module/package mein accidentally included unnecessary metadata.

### Production Drift

> Production state ka source-controlled/package state se diverge ho jaana.

---

# 84. Quick Revision

```text
Salesforce DX
     |
     +--> Source
     +--> Modules
     +--> Packages
```

Remember:

```text
Source != Module != Package
```

They are related but different.

---

# 85. Complete Mental Model

```text
                BUSINESS CAPABILITY
                         |
                         v
                    MODULE
                         |
          +--------------+--------------+
          |              |              |
        Code         Metadata       Security
          |              |              |
          +--------------+--------------+
                         |
                         v
                  DEPENDENCY MAP
                         |
                         v
                   SOURCE CONTROL
                         |
                         v
                  PACKAGE VERSION
                         |
                         v
              AUTOMATED VALIDATION
                         |
                         v
        +----------------+----------------+
        |                |                |
       QA               UAT          Integration
        |                |                |
        +----------------+----------------+
                         |
                         v
                    PRODUCTION
```

---

# 86. Final Summary

Salesforce DX ka major shift sirf CLI commands ka shift nahi tha.

Real shift tha:

> **How we think about Salesforce application structure and delivery.**

Traditional approach:

```text
Org
 |
 +--> Everything
```

Modern engineering mindset:

```text
Business Capabilities
        |
        v
Logical Modules
        |
        v
Source Control
        |
        v
Versioned Artifacts
        |
        v
Controlled Deployment
```

### Most important lessons

1. **Har cheez package mein daalna mandatory nahi hai.**
2. **Start small.**
3. **Source control, modularization aur packaging ko same concept mat samjho.**
4. **Dependency analysis is critical.**
5. **Automatic dependency discovery complete nahi hoti.**
6. **Human architectural knowledge still matters.**
7. **Cruft identify karo.**
8. **Missing metadata identify karo.**
9. **Production changes automatically package mein वापस nahi aate.**
10. **Production drift ko control karo.**
11. **Business capability ke basis par modules define karna useful hai.**
12. **High cohesion + manageable coupling target karo.**
13. **Security aur tests ko module ka part samjho.**
14. **External integrations ko dependency map mein include karo.**
15. **Package boundary ko architecture boundary ki tarah bhi treat kiya ja sakta hai.**
16. **Packaging ko CI/CD aur source control discipline ke saath adopt karo.**
17. **Package adoption engineering maturity ka replacement nahi hai.**

---

# 87. Developer's 30-Second Mental Model

Agar interview ya project mein koi pooche:

> **"Unlocked package / modular development ka basic idea kya hai?"**

Bolo:

> "Main Salesforce org ko blindly ek single deployment unit treat nahi karunga. Pehle business capabilities identify karunga, phir unke metadata, code, security, tests aur integration dependencies map karunga. Uske baad cohesive modules define karke unhe source control mein manage karunga. Jahan independent lifecycle useful ho, wahan package boundary introduce karunga. Package versions ko CI/CD ke through QA, UAT aur production tak promote karunga, aur direct production changes ko source/package mein reconcile karke drift prevent karunga."

---

# 88. Final Architect Takeaway

```text
Don't start with:
"Which metadata should I package?"

Start with:
"What business capability am I trying to isolate?"

Then ask:
"What does it depend on?"

Then:
"What depends on it?"

Then:
"Who owns it?"

Then:
"Can I test and deploy it independently?"

Then:
"Should this become a package?"
```

**This is the real modular-development mindset.**

---

# LinkedIn Post — Hinglish Version

## Salesforce DX: Package Banana Main Goal Nahi Hai — Modular Thinking Main Goal Hai 🚀

Salesforce DX ke baare mein learn karte time ek important realization hua:

**Source Control ≠ Modularization ≠ Packaging**

Teeno related hain, but same nahi hain.

Traditional Salesforce development mein hum aksar:

```text
Production Org = Source of Truth
```

ke around sochte the.

Modular/package-oriented development mein thinking change hoti hai:

```text
Business Capability
      ↓
Logical Module
      ↓
Dependencies
      ↓
Source Control
      ↓
Package Version
      ↓
CI/CD
      ↓
Production
```

Sabse important lesson:

**Don't package everything just because you can.**

Pehle:

- business capability identify karo
- dependencies map karo
- security + tests include karo
- unnecessary metadata/cruft remove karo
- source control discipline establish karo
- then package boundaries define karo

Aur ek aur critical point:

**Production mein direct change karna = automatic package update nahi hai.**

Isliye production drift ko control karna architecture ka important part hai.

For me, modular development ka real question hai:

> "Can this business capability be understood, owned, tested, versioned and deployed independently?"

Agar answer yes hai, tab package boundary meaningful ho sakti hai.

#Salesforce #SalesforceDX #UnlockedPackages #Architecture #DevOps #CI_CD #Git #SalesforceArchitect #Metadata #ModularDevelopment

---

# Notes on the Original 2018 Article

The original article used historical Salesforce DX commands and terminology. For learning purposes:

- Preserve the **architecture concepts**.
- Treat exact CLI command syntax as **version-specific**.
- For a current implementation, verify the command syntax against the current Salesforce CLI documentation.
- Do not blindly copy a 2018 deployment workflow into a 2026 production pipeline.

The most durable knowledge from this article is:

```text
Source Control
      +
Modularization
      +
Dependency Management
      +
Package Boundaries
      +
CI/CD
      +
Production Governance
```

These together form the foundation of a scalable Salesforce development lifecycle.
