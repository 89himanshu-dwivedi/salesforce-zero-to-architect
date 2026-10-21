# Enterprise Architecture Thinking: Zero to Architect

Hi, I'm **Himanshu Kumar** — Technical Lead & Solution Architect with **8 years** of experience building enterprise systems.

I work at **CRISIL Ltd, an S&P Global company**, on S&P Global projects.

This is my catch-all learning repo for everything that sits *around the application* — messaging, caching, databases, integrations, infrastructure, cloud, AI/GenAI, and the architecture thinking needed to connect them into production-ready enterprise systems.

---

## What You'll Get From Here

# Salesforce Sharing & Visibility --- Complete Learning Roadmap

A structured Hinglish learning index for Salesforce record access,
sharing architecture, user visibility, privacy, security, Apex-managed
sharing, Experience Cloud, and Enterprise Territory Management.

This README follows the organization and GitHub-friendly presentation of
the supplied reference README while keeping the Salesforce course
content separate and intact.

------------------------------------------------------------------------

## What You'll Learn

This course builds a practical understanding of how Salesforce decides
who can access records, fields, and users---and how to design,
troubleshoot, and scale that access model.

``` text
User and License Model
        ↓
Object Permissions and Field-Level Security
        ↓
Org-Wide Defaults (OWD)
        ↓
Role Hierarchy and Implicit Sharing
        ↓
Sharing Rules, Teams, Groups, and Queues
        ↓
Apex-Managed Sharing and Advanced Access Patterns
        ↓
Experience Cloud and Territory Management
        ↓
Privacy, Consent, Security, and Production Readiness
```

**Course at a glance:** 16 topics · 238 listed subtopics · Hinglish

------------------------------------------------------------------------

## What This Course Covers

-   Salesforce record access and the sharing architecture
-   OWD, profiles, permission sets, FLS, roles, and implicit sharing
-   Declarative sharing rules, manual sharing, teams, groups, and queues
-   Apex-managed sharing, share objects, Row Cause, and security
    enforcement
-   Experience Cloud sharing models and external-user license
    considerations
-   Enterprise Territory Management and design trade-offs
-   Salesforce platform security, audit, privacy, and consent management
-   Scenario-based design, revision, troubleshooting, and interview
    preparation

------------------------------------------------------------------------

## Course Index

1.  Sharing & Visibility --- Record Access Kaun Dekhega aur Kaise (OWD,
    Roles, Profiles, Permission Sets, Teams)
2.  Designer Level Deep Dive --- Licenses, OWD ki Asli Definition aur
    "Profile vs OWD mein Boss Kaun?" (Real Project Debates)
3.  Horizontal Sharing ka Engine --- Sharing Rules, Share Tables ka
    Backend, Manual Sharing aur Login Hours / IP Range Security
4.  Data Model ka Sharing Impact --- External Objects, List Views,
    Salesforce Files aur Master-Detail vs Lookup Security
5.  User Setup se Territory tak --- User Record, Licenses, Teams, Role
    par Exclusive Access aur Enterprise Territory Management
6.  Programmatic Sharing --- Apex Managed Sharing, With/Without Sharing
    Keywords aur Visualforce + Trigger ka Security Behaviour
7.  Salesforce Security ka Bada Picture --- Health Check, Shield, Audit
    Trail, MFA/SSO aur Apex/VF ki Vulnerabilities
8.  Hands-On Setup Walkthrough --- Account Teams Configure Karna,
    Master-Detail Inheritance, Share Button ka Behaviour aur Flow se
    Apex Sharing
9.  Revision & Edge Cases --- Junction ka Primary Master, OWD Change ki
    Direction, Row Cause Catalogue, Manager Groups aur Inherited Sharing
    Matrix
10. Scenario Solving Playbook --- 16-Tool Toolkit, Partner Role
    Hierarchy ka Raaz aur CTA-Style Requirement Breakdown
11. Experience Cloud (Community) Sharing --- 5 Licenses, High Volume
    Users, Sharing Sets vs Share Groups aur Super User Access
12. CTA Review Board Practice --- Data Model, Large Data Volume Strategy
    aur Sharing ke Trade-offs
13. Data Privacy & Consent Management --- GDPR/CCPA ka Common Framework,
    Data Classification aur Salesforce ka Layered Consent Model
14. Programmatic Sharing ka Practical Playbook --- Teen Coded Scenarios,
    Production Hardening, Unit Tests aur Defer Sharing Calculations
15. Enterprise Territory Management ka Setup Guide --- Models, Type
    Priority ka Sach, Forecasting Link aur Assignment Rules ke Gotchas
16. User Sharing ("Kaun Kise Dekh Sakta Hai") --- User Object ka OWD,
    Group-Membership Rules, User Visibility Settings aur Role ke Access
    Options

------------------------------------------------------------------------

## Sharing Design Decision Framework

``` text
Business Requirement
        ↓
Identify Users, Licenses, and Data
        ↓
Check Object Permissions and Field-Level Security
        ↓
Define the Baseline with OWD
        ↓
Evaluate Role Hierarchy and Implicit Sharing
        ↓
Evaluate Declarative Sharing, Teams, Groups, and Queues
        ↓
Use Apex-Managed Sharing Only When Needed
        ↓
Validate Access, Exceptions, Security, and Scale
        ↓
Test, Troubleshoot, and Document Trade-offs
```

### Questions to ask for every design

-   What business problem are we solving?
-   Which users need access, and which licenses do they have?
-   Is the requirement about object access, field visibility,
    record-level access, or user visibility?
-   What should the default access be when a user is not the record
    owner?
-   Can hierarchy, implicit sharing, teams, groups, queues, or sharing
    rules meet the requirement?
-   Is programmatic sharing genuinely required, and how will it be
    recalculated and tested?
-   What are the implications for privacy, performance, maintainability,
    and large data volumes?

------------------------------------------------------------------------

## Complete Topic Roadmap

## 01. Sharing & Visibility --- Record Access Kaun Dekhega aur Kaise (OWD, Roles, Profiles, Permission Sets, Teams)

Poora data access model: OWD se lekar sharing rules, role hierarchy,
implicit sharing, FLS, permission sets, teams, Experience Cloud aur Apex
sharing tak --- plus 7 exam-style scenarios.

### Subtopics

-   Sharing Architecture ka Foundation --- Har Record ka ek Owner hota
    hai
-   User Licenses ka Sharing pe Asar --- Full Sharing vs High Volume
-   Org-Wide Defaults (OWD) --- Sabse Neeche ka Base Lock
-   OWD Access Levels --- Private se Public Full Access tak (aur
    Reporting ka twist)
-   Sharing Rules --- Owner-Based, Criteria-Based aur Guest User Rules
    (automatic sharing)
-   Sharing Overrides --- View All Data / Modify All Data ka Master Key
-   Manual Sharing --- Ek-ek Record Haath se Share Karna
-   Role Hierarchy --- Upar wale ko Access, Sibling ko Nahi
-   Implicit Sharing --- Account/Contact/Opportunity/Case ka Automatic
    Behaviour
-   Profiles --- Object Access (CRED), Tabs aur Role vs Profile ka Farq
-   Field Level Security vs Page Layout --- Sabse Bada Confusion Clear
-   Permission Sets --- Profile ke Upar Extra Access (5 Types)
-   Permission Set Groups --- Kai Permission Sets ka ek Bundle
-   Public Groups, Manager Groups aur Queues --- Users ko Bundle Karna
-   Account, Opportunity aur Case Teams --- Cross-Team Collaboration
-   Experience Cloud (Community) Sharing --- Sharing Sets aur High
    Volume Users
-   Programmatic (Apex Managed) Sharing --- Share Objects aur Row Cause
-   Exam-Style Scenarios --- 7 Requirements aur unke Solutions

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## 02. Designer Level Deep Dive --- Licenses, OWD ki Asli Definition aur "Profile vs OWD mein Boss Kaun?" (Real Project Debates)

Sharing & Visibility Designer cert ka blueprint, grant-not-block
mindset, internal vs external users, OWD ke gotchas aur recalculation,
profile anatomy, View All vs View All Data, permission set truth table,
aur project se nikle hue real scenarios/debates.

### Subtopics

-   Sharing & Visibility Designer Exam ka Blueprint --- Kiske Liye,
    Kitne Marks, Kahan Focus
-   Sabse Bada Mindset --- Sharing GRANT karta hai, BLOCK kabhi nahi
-   User Licenses --- Sharing ki Sabse Pehli Deewar (jo license nahi
    deta, wo share bhi nahi hoga)
-   OWD ki Asli Definition --- "Jab aap Owner NAHI ho" tab kaam karta
    hai
-   Internal vs External Users --- Definition, Licenses aur "Roadmap
    Pehle Socho" Rule
-   OWD Values Deep Dive --- Private, Read Only, Read/Write, Transfer,
    Controlled by Parent, Full Access
-   Calendar & Activity Access --- Chaar Levels aur ek Sensitive
    Real-Life Risk
-   Price Book aur User Object --- Do Special OWD Cases
-   Salesforce ki Case Recommendation vs Sales Reality --- ek Genuine
    Conflict
-   Sharing Recalculation --- Change Turant Apply Nahi Hota (Order of
    Operations)
-   OWD ki Limitations --- Private se Public wapas nahi ja sakte (Apex
    Sharing ka lock)
-   Profile ki Anatomy --- Apps, Tabs, Objects, Fields, Layouts aur
    Admin Permissions
-   View All vs View All Data --- Exam ka Favourite Confusion (Object vs
    Org)
-   Field Mandatory Kahan Banayein? --- Object Level vs Page Layout vs
    Validation Rule (UI vs API ka Trap)
-   Profile vs Permission Set Strategy --- "Base Power vs Super Power"
    aur 4-Row Truth Table
-   Permission Set ki Limitations --- Page Layouts, License Binding aur
    Audit Trail
-   Ultimate Boss Kaun? --- OWD vs Profile ki 6 Combinations (sabse
    important slide)
-   Real Project Scenarios & Debates --- Hierarchy Hataana,
    Multi-Country Rollout, Sensitive Fields

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## 03. Horizontal Sharing ka Engine --- Sharing Rules, Share Tables ka Backend, Manual Sharing aur Login Hours / IP Range Security

Vertical vs horizontal access ka problem statement, owner vs criteria
based rules ka decision framework (300 vs 50 limit), share objects ka
backend, manual sharing + Expand List debugging, recalculate button,
share locks, aur login hours / IP ranges / business hours ka poora
security angle.

### Subtopics

-   Problem Statement --- Vertical Access kaafi kyun nahi hai (2 Gaps)
-   Har Sharing Rule ke 3 Sawaal --- Kya, Kisko, Kitna (aur ek
    Prerequisite)
-   Owner-Based vs Criteria-Based --- 300 vs 50 ka Limit hi Aapka
    Decision Hai
-   "Share With" ke Options aur One-Way Sharing ka Bada Gotcha
-   Related Object Cascade --- Ek Sharing Rule se Kaun-Kaun se Related
    Records Khul Jaate Hain
-   Criteria-Based Rules Case-Sensitive Hoti Hain --- Silent Failure ka
    Sabse Bada Reason
-   Behind the Scenes --- Share Objects (AccountShare,
    MyObject\_\_Share) mein Kya Likha Jaata Hai
-   Manual Sharing UI + "Expand List" --- Access Debugging ka Sabse
    Powerful Tool
-   Recalculate Button aur Object-Specific Share Locks --- Bulk Rules ka
    Performance Trick
-   Decision Framework --- Requirement aane par Kis Order mein Socho (4
    Steps)
-   Sharing Rules ke 6 Important Behaviours --- Most Permissive,
    Inactive Users, Expired Packages
-   Login ke Peeche ka Engine --- Username Daalne se Home Page tak ke 4
    Checks
-   Login Hours --- Profile-Level Time Restriction aur "5 PM par Kya
    Hoga?" ka Famous Debate
-   IP Ranges ka Confusion --- Network Access vs Profile Login IP vs
    "Enforce on Every Request"
-   Business Hours & Holidays vs Login Hours --- Naam Milta-Julta, Kaam
    Bilkul Alag
-   Session ke Q&A aur Takeaways --- Record Type Criteria, Group IDs aur
    Quick Recall

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## 04. Data Model ka Sharing Impact --- External Objects, List Views, Salesforce Files aur Master-Detail vs Lookup Security

Salesforce Connect ke external data sources & external objects, External
vs Indirect Lookup relationships, custom list views ki visibility (aur
wo record access kyun nahi deti), Salesforce Files ke 3 sharing states,
aur junction object mein security dono masters se kaise aati hai.

### Subtopics

-   External Data Sources & External Objects --- Data jo Salesforce mein
    Rehta hi Nahi (Salesforce Connect)
-   External Lookup vs Indirect Lookup vs Lookup --- Teen Relationship
    Types ka Farq
-   Custom List Views --- Do Profile Permissions jo Sab Kuch Decide
    Karti Hain
-   List View Visibility Options --- aur Portal Users ka Chhupa hua Risk
-   Sabse Bada Concept --- List View Share karna Record Share karna NAHI
    hai
-   Salesforce Files (ContentDocument) --- Teen Sharing States
-   File Share karne ke Tareeke --- Users, Groups, Records aur Public
    Link
-   Files vs Classic Attachments --- aur "Salesforce koi CMS nahi hai"
-   Master-Detail vs Lookup --- Relationship ka Direct Security Impact
-   Junction Object (Many-to-Many) --- Security DONO Masters se, Look &
    Feel sirf Primary se
-   Session 4 Quick Recall --- Sab kuch ek Table mein

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## 05. User Setup se Territory tak --- User Record, Licenses, Teams, Role par Exclusive Access aur Enterprise Territory Management

Authentication vs authorization, internal/external/integration users,
user record ki anatomy, user vs feature license, Manager & Delegated
Approver, 4 tarah ke teams, role par Contact/Opportunity/Case ka
exclusive access (jo sharing rules bacha deta hai), aur Enterprise
Territory Management 1.0 vs 2.0 + Role vs Territory decision tree.

### Subtopics

-   Authentication vs Authorization --- Sharing kis Half mein Baithti
    Hai
-   Users ke Teen Roop --- Internal, External aur Integration Users
-   User Record ki Anatomy --- Har Field ka Sharing/Automation par Asar
-   User License vs Feature License --- Parent aur Add-on ka Rishta
-   Manager Field aur Delegated Approver --- Chhote Fields, Bada Impact
-   Teams ke Chaar Roop --- Manager-based View, Default Teams aur Record
    Teams
-   Role Hierarchy --- Design ke Do Sunhare Niyam aur OWD ka
    Prerequisite
-   Role ka Chhupa hua Superpower --- Contact / Opportunity / Case
    Access
-   Scenario A --- Role Access se Sharing Rule Bachao (Professional
    Services Example)
-   Scenario B --- Ulta Case: jab "Edit All Cases" bilkul Khatarnaak hai
    (Telecom)
-   Scenario C --- GDPR Privacy Champion (Sharing Rule ki jagah Role
    Access)
-   Contact-without-Account ka Gotcha aur 10,000 Child Records ka Skew
    Problem
-   Enterprise Territory Management --- Pehle Business Samjho, Phir
    Technology
-   Territory Hierarchy Design --- Named vs Region vs Mixed Model
-   Territory Management 1.0 vs Enterprise Territory Management 2.0 ---
    Kya Badla
-   Territory par Exclusive Access --- Role se Do Extra Powers
-   Decision Tree --- Role Hierarchy vs Territory Management (Matrix hi
    Jawab Hai)

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## 06. Programmatic Sharing --- Apex Managed Sharing, With/Without Sharing Keywords aur Visualforce + Trigger ka Security Behaviour

Apex sharing = manual sharing (bas actor alag), share object ke 4
fields, Apex Sharing Reasons ka asli faayda, recalculation batch class,
4 tricky gotchas, with/without/inherited sharing ke saare combinations,
inner class ka trap, Execute Anonymous ka user mode, Visualforce
controller matrix aur "triggers system mode mein chalte hain" wala myth.

### Subtopics

-   Programmatic Sharing kahan Fit hota hai --- Architecture ka Aakhri
    Layer
-   Core Concept --- Apex Sharing aur Manual Sharing Bilkul Ek hi Cheez
    Hain
-   Share Object ke 4 Fields aur Actual Code
-   Apex Sharing Reasons (Row Cause) --- Optional hai, par Ise Hamesha
    Use Karo
-   Expand List se Debugging --- "Isko Access Kyun Mila?" ka Instant
    Jawab
-   Scenario 1 --- Account Plan + Account Team (Real Project)
-   Scenario 2 --- Lead par "Potential Repeat Sale" Account ka Access
-   Scenario 3 & 4 --- Recruiting App aur Rule-Engine Driven Sharing
    (J&J)
-   Apex Sharing Recalculation --- Wo Batch Class jo Log Bhool Jaate
    Hain
-   Chaar Tricky Gotchas --- jo Exam mein aur Production mein Pakadte
    Hain
-   with sharing / without sharing / inherited sharing --- aur "kuch na
    likhne" ka Khatra
-   Cross-Class Calls --- Kis Class ka Sharing Jeetega?
-   Inner Class ka Trap --- Sharing INHERIT nahi Hoti
-   Sharing Keywords vs Access Modifiers --- Do Bilkul Alag Cheezein
-   Profile par Apex Class Access --- Sirf Entry Point ka Access Chahiye
-   Execute Anonymous --- System Mode mein NAHI Chalta
-   Visualforce Controllers ka Matrix --- Standard vs Extension vs
    Custom
-   Trigger Myth Busted --- "Triggers hamesha System Mode mein chalte
    hain" (aur Final Recall)

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## 07. Salesforce Security ka Bada Picture --- Health Check, Shield, Audit Trail, MFA/SSO aur Apex/VF ki Vulnerabilities

Sharing ke bahar ki security: phishing/malware, Security Health Check,
field history vs Field Audit Trail (aur 24-month deletion gotcha),
Shield ke 3 pillars, Transaction Security & concurrent session limit,
password policies, duplicate username ka sach, My Domain + SSO, MFA vs
SSO ka real answer, file upload/download security, aur SOQL Injection /
XSS / CSRF.

### Subtopics

-   Security ka Bada Picture --- 6 Areas aur Sharing Stack ka Final
    Recap
-   Phishing & Malware --- trust.salesforce.com aur Aapki Zimmedari
-   Security Health Check --- Ek Click mein Aapke Org ka Security Report
    Card
-   Basic Audit Hygiene --- System Info Fields, Login History, Setup
    Audit Trail
-   Field History Tracking --- 20 Fields ki Limit aur Chatter Feed ka
    Smart Trick
-   History Data Retention ka Chhupa hua Gotcha --- 24 Mahine ke Baad
    Data Gayab
-   Salesforce Shield #1 --- Platform Encryption (aur Encryption kaam
    kaise karta hai)
-   Salesforce Shield #2 --- Event Monitoring
-   Salesforce Shield #3 --- Field Audit Trail (aur DIY Trigger ka Trap)
-   Transaction Security Policies aur Concurrent Session Limit (Default
    = 5)
-   Password Policies aur Programmatically Password Set Karna
-   Duplicate Usernames --- Documentation vs Practice (Exam mein kya
    Likhein)
-   "Expire All Passwords" --- Kaunsa "All"? (Misleading Naam)
-   Cookies in Visualforce --- Available hai, par Use Karne se Bachо
-   My Domain aur Single Sign-On (SSO) --- Branding se Security tak
-   Two-Factor / MFA --- aur "SSO ho to MFA lagegi ya nahi?" ka Asli
    Jawab
-   File Upload & Download Security --- Browser mein Khule ya Download
    Ho?
-   SOQL Injection --- Search Box se Poora Database Khol Dena
-   XSS aur CSRF --- Script Injection aur Session Hijacking
-   HTTP vs HTTPS, SSL vs TLS --- Interview ka Favourite Sawaal

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## 08. Hands-On Setup Walkthrough --- Account Teams Configure Karna, Master-Detail Inheritance, Share Button ka Behaviour aur Flow se Apex Sharing

Click-by-click implementation guide: Account Teams enable + team roles +
default teams, AccountTeamMember object, master-detail 'Controlled by
Parent' demo (aur uska implicit sharing se farq), share object naming
rules, Sharing button kab gayab hota hai, recruiting app scenario Apex +
Invocable Method + record-triggered Flow, aur territory model ka
hands-on setup.

### Subtopics

-   Account Teams --- Definition, Rules aur Kab Use Karein
-   Step-by-Step --- Account Teams Enable Karna aur Team Roles Setup
-   Account Team Member Add Karna --- Live Demo ke Observations
-   Default Account Teams --- User par Set Hoti Hai, Org par Nahi
-   AccountTeamMember Object --- Ek Hidden Extension Point
-   Master-Detail "Controlled by Parent" --- Hands-On Demo (aur ek
    Terminology Correction)
-   Sabse Bada Demo Sabak --- Record Access Milne par bhi Object Access
    Chahiye
-   Child Object par OwnerId Query Nahi Hota --- Classic Interview
    Question
-   Share Object ka Naam Kaise Banta Hai --- aur Developer Console se
    Kaise Dekhein
-   "Sharing Button Kahan Gaya?" --- Live Demo aur 3-Point Checklist
-   Apex Managed Sharing ka Real Scenario --- Recruiting App (Candidate
    → Interviewer)
-   Invocable Method Pattern --- Apex jo Flow se Call ho Sake
-   Record-Triggered Flow se Apex Call Karna --- Step by Step
-   Enterprise Territory Management ka Hands-On --- Model, Rules aur
    "Run Rules"
-   Hands-On Gotchas ki Checklist --- Sab Kuch Ek Jagah

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## 09. Revision & Edge Cases --- Junction ka Primary Master, OWD Change ki Direction, Row Cause Catalogue, Manager Groups aur Inherited Sharing Matrix

Group revision session ke wo nuggets jo baaki jagah nahi milte:
Workbench se junction object ka primary master pata karna, OWD badhane
vs ghatane ka alag behaviour, Campaign sharing rule mein Full Access,
Manager Groups ka asli use, poora Row Cause catalogue, user mode vs
system mode ki cheat sheet, aur inherited sharing ka complete matrix ---
plus kuch live-tested corrections.

### Subtopics

-   External Access Column --- Ab Default ON hai, aur Enable karke Wapas
    Nahi Ja Sakte
-   Calendar Access ke 4 Levels --- Precise Matlab (Live Demo se)
-   Junction Object ka PRIMARY Master kaise Pata Karein --- Workbench ka
    RelationshipOrder Trick
-   OWD Change ki DIRECTION Matter Karti Hai --- Badhao to Turant,
    Ghatao to Intezaar
-   "Ye Objects Hamesha Private Hain" --- Live Test se Nikla Sach
-   View All Data / Modify All Data --- License Type par Depend Karta
    Hai
-   "Modify All chahiye par Delete nahi" --- Ye Possible Nahi Hai
-   Core License + Feature License --- Add-on Model (Identity Connect ka
    Example)
-   Sharing Rules ke Access Levels Object ke Hisaab se Badalte Hain
    (Campaign = Full Access!)
-   Sharing Rules ke 3 Types aur unki Limits (50 / 300)
-   Manager Groups vs Manager Subordinate Groups --- aur Hierarchy OFF
    karne ki Zarurat
-   Managed Package Fields par Sharing Rules --- aur License Expire hone
    par Kya Hota Hai
-   Row Cause ka Poora Catalogue --- Teen Categories mein Sharing
-   Standard Objects par Apex Sharing --- Ek nahi, Kai Access Level
    Fields
-   External Object Relationships --- Ek Line ka Decision Rule
-   User Mode vs System Mode --- Cheat Sheet aur Yaad Rakhne ki Trick
-   inherited sharing ka Poora Matrix --- aur Uski Asli Power

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## 10. Scenario Solving Playbook --- 16-Tool Toolkit, Partner Role Hierarchy ka Raaz aur CTA-Style Requirement Breakdown

Ek bade CTA-style scenario (Sunflower Computing) ko line-by-line todkar
solve karna: 4 layers ka sharing toolkit, implicit sharing ki direction,
role ke radio buttons ka hidden trap, partner user role hierarchy (3
auto-created roles), queues + case assignment rules, CTA artifact tips,
aur design decision framework (Apex vs Manual vs ETM).

### Subtopics

-   Sharing Toolkit --- 4 Layers, 16 Declarative Tools (Ek Mental Model)
-   Scenario Padhne ka Method --- "Home Alone wala Kevin" Feeling se
    Bahar Kaise Aayein
-   Case Study Setup --- Sunflower Computing ka Poora Landscape
-   Scenario 1a --- Opportunity Private, aur Implicit Sharing ki
    DIRECTION
-   Scenario 1b & 1c --- Account Private (+ ek Popup) aur Contact
    Controlled by Parent
-   🔥 Sabse Bada Trap --- Role ke Radio Buttons aapki OWD Design Tod
    Sakte Hain
-   Scenario 2 --- Role Hierarchy Banana (Draw Top-Down, Solve
    Bottom-Up)
-   ⭐ Partner User Role Hierarchy --- 3 Roles jo Salesforce APNE AAP
    Banata Hai
-   Scenario 2c --- Service Hierarchy, aur "No Requirement" ko bhi
    Respect Karo
-   Scenario 3 --- Case OWD Private + Queues (jab Record ko "Kai Log"
    Own Karein)
-   Case Assignment Rules --- Type/Subtype se Routing (aur Catch-All
    Queue)
-   CTA Artifact Tips --- 127 Countries ko Whiteboard par Kaise
    Dikhayein
-   Design Decision Framework --- Apex vs Manual Sharing vs Territory
    Management
-   Q&A --- Multi-Level Account Hierarchy ka Sharing Problem (aur uska
    Declarative Hal)
-   Sharing Issue ko Troubleshoot karne ki Checklist (+ Topic 10 Recall)

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## 11. Experience Cloud (Community) Sharing --- 5 Licenses, High Volume Users, Sharing Sets vs Share Groups aur Super User Access

External users ka poora sharing model: 5 community license types ka
comparison, high volume community users kya-kya NAHI kar sakte, Sharing
Sets (external ko access dena) vs Share Groups (external ka data
internal ko dena) ka ulta direction, aur do tarah ka Super User Access
--- Partner (contact record se) aur Community Plus (permission set se).

### Subtopics

-   Communities ka Overview --- Teen Duniyaon ka Rishta
-   5 Community License Types --- Poora Comparison
-   Login-Based vs Usage-Based Licensing --- Pricing Model ka Design par
    Asar
-   Community mein Sharing ke 4 Tareeke --- Ek Funnel ki tarah Sochо
-   High Volume Community User --- Wo Poori List jo ye NAHI kar sakte
-   External Sharing Model Enable Karna --- Do Alag Columns
-   Sharing Sets --- External User ko uske Account se Jude Records ka
    Access
-   Share Groups --- ULTI Direction: External ka Data Internal Team ko
    Dikhana
-   Setup Walkthrough --- Sharing Set aur Share Group Banana (aur
    30-Minute Wait)
-   Super User Access #1 --- Partner Super User (Contact Record se)
-   Super User Access #2 --- Community Plus Super User (Permission Set
    se)
-   Design Patterns, Practice Plan aur Topic 11 Recall

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## 12. CTA Review Board Practice --- Data Model, Large Data Volume Strategy aur Sharing ke Trade-offs

Ek hi scenario (Clean Bikes) ke teen alag solutions se seekhna: review
board ka format, license aur role hierarchy ke trade-offs, 12,000
dealerships par sharing rules vs sharing sets, Large Data Volume ka
poora decision framework (Big Objects, Salesforce Connect, Heroku
Connect, skinny tables, Async SOQL), aur ek clever formula-field trick
jab FLS kaafi na ho.

### Subtopics

-   CTA Review Board ka Format --- Aur "Sahi Jawab" ka Myth
-   "90% Log Data Model ki wajah se Fail Hote Hain" --- Kyun
-   Clean Bikes Scenario --- Actors, Licenses aur Teen Alag Faisle
-   Role Hierarchy ke Teen Designs --- aur Partner Roles ka
    Auto-Creation
-   Data Model ke Faisle --- Person Accounts, Opportunity vs Contract,
    aur Custom Objects
-   Postal Code se Lead Routing --- Territory Management ka Bada Gap
-   Sharing at Scale --- 30 Sharing Rules Theek, par 12,000 Nahi
-   Large Data Volume --- Poora Decision Framework
-   Big Objects Deep Dive --- Index Design, Async SOQL aur Archive
    Pattern
-   ⭐ Jab FLS Kaafi Na Ho --- Formula Field ka Clever Trick
-   Trade-off Thinking aur Topic 12 Recall

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## 13. Data Privacy & Consent Management --- GDPR/CCPA ka Common Framework, Data Classification aur Salesforce ka Layered Consent Model

Privacy regulations ka landscape aur unke common threads, 3-pillar
framework (data dhoondho, manage karo, control do), Data Classification
(owner/usage/sensitivity/compliance), consent ka 4-level layered object
model, Authorization Forms, Legal Basis, Party Consent (CCPA), aur
Consent API jo saare sources ko reconcile karta hai.

### Subtopics

-   Privacy Regulations ka Landscape --- GDPR ke Baad Jo Hua
-   Common Threads --- Isi liye Ek Framework Kaafi Hai
-   3-Pillar Framework --- Dhoondho, Manage Karo, Control Do
-   Pillar 1 --- Data Dhoondho, Access Samjho, Risk Aanko
-   Data Minimization aur Retention --- "Jo Data Hai Hi Nahi, Wo Leak
    Nahi Ho Sakta"
-   Pillar 2 --- Data Subject Access Requests (DSAR) aur Access Review
-   Pillar 3 --- Transparency, Data Rights aur Consent Preferences
-   Data Classification --- Salesforce ke Governance Fields
-   Consent Model ke 4 Layers --- Broad se Bahut Specific tak
-   Legal Basis aur Authorization Forms --- Consent hi Ekmatra Raasta
    Nahi
-   Consent API --- "Objects Badh Gaye, Ab Jawab Kaun Dega?"
-   Practical Notes, Sharing se Rishta aur Topic 13 Recall

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## 14. Programmatic Sharing ka Practical Playbook --- Teen Coded Scenarios, Production Hardening, Unit Tests aur Defer Sharing Calculations

Apex sharing ka hands-on playbook: kab use karein (4 signals), share
objects ka behaviour, record types visibility control nahi karte
(production gotcha), teen poore coded scenarios (lookup-based, standard
object + public group, time-based schedulable), production hardening
(update/null/exception), Apex security checks, System.runAs ke rules,
aur Defer Sharing Calculations + Guest User security changes.

### Subtopics

-   Teen Tarah ki Sharing --- aur Exam mein Programmatic ka Weightage
-   Programmatic Sharing Kab Use Karein --- 4 Signals
-   Share Objects --- Kab Bante Hain aur Unke Fields
-   🚨 Production Gotcha --- Record Types Record VISIBILITY Control Nahi
    Karte
-   User Managed vs Apex Managed --- Owner Change ka Nirnayak Farq
-   Apex Sharing Reasons --- Sirf Custom Objects (aur Standard Objects
    ka Dard)
-   Scenario 1 --- Lookup Fields se Sharing (Poora Code Walkthrough)
-   ⭐ Scenario 1 ka Production Hardening --- Teen Cheezein jo Demo Code
    mein Nahi Thin
-   Scenario 2 --- Standard Object (Account) ko Public Groups se Share
    Karna
-   Scenario 3 --- Dynamic / Time-Based Sharing (Schedulable Apex)
-   Apex mein Security Enforce Karna --- Sharing, CRUD aur FLS
-   Unit Tests se Security Verify Karna --- System.runAs ke Rules
-   Do Operational Topics --- Defer Sharing Calculations aur Guest User
    Security
-   "Apex Sharing Aakhri Choice Ho" --- aur Topic 14 Recall

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## 15. Enterprise Territory Management ka Setup Guide --- Models, Type Priority ka Sach, Forecasting Link aur Assignment Rules ke Gotchas

ETM ka step-by-step setup: TM sirf Accounts par chalta hai, territory
hierarchy role hierarchy se independent hai, Territory Type Priority
asal mein kya hai, 4 models aur 3 states (archive irreversible),
forecast hierarchy ban jaana, Confine Opportunity Assignment checkbox,
'activate before run rules' gotcha, inherited assignment, aur
opportunity reassignment ke liye Apex class.

### Subtopics

-   Territory Management ka Scope --- Sirf Accounts (aur Opportunities
    ka Alag Rasta)
-   Business Case --- TM Kyun, Spreadsheet ya Custom Object Kyun Nahi
-   Territory Hierarchy vs Role Hierarchy --- Bilkul Independent
-   ⭐ Territory Type Priority ka Sach --- Ye Assignment Priority NAHI
    Hai
-   Territory Models --- 4 Models, 3 States, aur Archive ka One-Way Door
-   Territory ke Fields, Access Levels aur Permissions
-   Forecasting ka Rishta --- Territory Hierarchy hi Forecast Hierarchy
    Ban Jaati Hai
-   6-Step Build Process --- aur "Activate Before Run Rules" wala Gotcha
-   Assignment ke Teen Tareeke --- Rule-Based, Inherited aur Manual
-   Opportunity Territory Assignment --- Aur Wo Apex Class jo Salesforce
    Deta Hai
-   Decision Guide, Study Plan aur Topic 15 Recall

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## 16. User Sharing ("Kaun Kise Dekh Sakta Hai") --- User Object ka OWD, Group-Membership Rules, User Visibility Settings aur Role ke Access Options

Records nahi, USERS ka sharing: User object ka OWD, user sharing rules
ka unique 'based on group membership' type, user record ka manual
sharing, portal/community ke liye User Visibility Settings, View All
Users permission, @mention aur People tab par asar --- plus role
hierarchy ke access options ka poora matrix (OWD decide karta hai kaunse
options dikhenge, aur profile hamesha ceiling hai).

### Subtopics

-   User Sharing Kya Hai --- aur Ye Zaroori Kyun Ho Gaya
-   User Object ka OWD --- Sirf Do Options
-   ⭐ User Sharing Rules --- "Based on Group Membership" (Ek Unique
    Rule Type)
-   User Record ka Manual Sharing --- Sabse Granular Control
-   User Visibility Settings --- Portal/Community Users ke liye Alag
    Override
-   "View All Users" Permission --- User Object ka Master Key
-   Poora Scenario --- AW Computing ki Teen Requirements
-   Practical Impact --- People Tab, @mentions aur Chatter Groups
-   Role Hierarchy ke Access Options --- Ek Confusion Clear Karte Hain
-   ⭐ OWD Decide Karta Hai ki Role par Kaunse Options DIKHENGE
-   Profile Hamesha Ceiling Hai --- 6 Combinations ka Proof
-   Queues --- Kuch Practical Notes
-   Design Guide aur Topic 16 Recall

### Revision checklist

-   [ ] Understand the core concepts
-   [ ] Review the scenarios and practical gotchas listed in the topic
-   [ ] Explain the design choices and trade-offs in your own words
-   [ ] Practice troubleshooting or interview-style questions where
    applicable

------------------------------------------------------------------------

## Revision and Interview Preparation

Use the course index to revise concepts in layers:

``` text
Concept
   ↓
Simple Explanation
   ↓
Salesforce Behaviour and Configuration
   ↓
Scenario-Based Application
   ↓
Edge Cases and Gotchas
   ↓
Architecture Trade-offs
   ↓
Interview / Exam Recall
```

When solving a scenario, explicitly distinguish:

-   **Object permissions** from **record-level sharing**.
-   **Field-Level Security (FLS)** from page layout behaviour.
-   **OWD baseline access** from access granted by hierarchy, sharing
    rules, teams, or Apex.
-   **Internal users** from **external / high-volume users** and their
    licensing constraints.
-   **User visibility** from access to business records.
-   **Sharing a list view or file** from granting access to the
    underlying record.
-   **Declarative solutions** from **programmatic solutions**, including
    operational and scale trade-offs.

------------------------------------------------------------------------

## Production and Design Review Checklist

-   [ ] Business requirement and intended access outcome are explicit
-   [ ] User licenses and external-user constraints are checked
-   [ ] Object permissions and Field-Level Security are validated
-   [ ] OWD is appropriate for the required baseline
-   [ ] Role hierarchy and implicit sharing effects are understood
-   [ ] Sharing rules, teams, groups, queues, and manual sharing are
    evaluated
-   [ ] Apex-managed sharing is used only where appropriate
-   [ ] Sharing recalculation, ownership changes, and exceptions are
    considered
-   [ ] Large data volume and sharing-lock risks are considered
-   [ ] Privacy, consent, audit, and security implications are reviewed
-   [ ] Access has been tested with representative users and scenarios
-   [ ] Design decisions and trade-offs are documented

------------------------------------------------------------------------

## Definition of Done

A topic is ready for revision when you can:

-   Explain the concept in simple language.
-   Describe the relevant Salesforce behaviour and configuration.
-   Apply it to a realistic requirement or scenario.
-   Identify limitations, edge cases, and common mistakes.
-   Compare the available approaches and justify a design choice.
-   Explain how you would validate and troubleshoot the result.

------------------------------------------------------------------------

## Repository Principle

> Don't just memorize sharing features. Understand the access
> requirement, identify the right security layer, choose the simplest
> maintainable solution, and validate its behaviour in realistic
> scenarios.

## Source and Scope

-   Topic titles, descriptions, and subtopic lists are based on the
    supplied Salesforce course index.
-   This README is an index and learning roadmap; it does not contain
    the full lesson content for every listed subtopic.
-   The checklists and framework sections organize the supplied index
    for learning and revision.
## About Me

- Technical Lead & Solution Architect at **CRISIL Ltd, an S&P Global company**
- 8 years across Salesforce architecture, integrations and enterprise GenAI
- Domains: Public Sector, Financial Services, Healthcare, Supply Chain
- B.Tech in Computer Science, AKTU Lucknow
- [GitHub](https://github.com/89himanshu-dwivedi)
- [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/)
- [X](https://x.com/kum60094)
- [Trailblazer](https://www.salesforce.com/trailblazer/hdwivedi2)
- Email: [himanshu.jee.1996@gmail.com](mailto:himanshu.jee.1996@gmail.com)

---

# 🔒 Attribution & Ownership

Copyright (c) 2026 Himanshu Kumar. All rights reserved.

This material is my original learning and documentation work.

Reading it here and linking to it is welcome.

Downloading, copying, mirroring, forking, redistributing, republishing, creating derivative works, or using this material to train an AI/ML model requires **prior written permission**.

Request permission through [GitHub](https://github.com/89himanshu-dwivedi).

---

## License

See [LICENSE](https://github.com/89himanshu-dwivedi/enterprise-stack-zero-to-architect/blob/main/LICENSE) — proprietary, all rights reserved, permission required.

---


## ⭐ Repository Principle

> **Don't just learn technologies. Learn the problem they solve, understand the trade-offs, design for production, and know why an architect would choose one approach over another.**