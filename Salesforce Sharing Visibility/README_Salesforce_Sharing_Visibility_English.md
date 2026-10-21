# Enterprise Architecture Thinking: Zero to Architect

Hi, I'm **Himanshu Kumar** — Technical Lead & Solution Architect with **8 years** of experience building enterprise systems.

I work at **CRISIL Ltd, an S&P Global company**, on S&P Global projects.

This is my catch-all learning repo for everything that sits *around the application* — messaging, caching, databases, integrations, infrastructure, cloud, AI/GenAI, and the architecture thinking needed to connect them into production-ready enterprise systems.

---

## What You'll Get From Here

# Salesforce Sharing & Visibility --- Complete Learning Roadmap

An English-language learning roadmap for Salesforce record access,
sharing architecture, user visibility, privacy, security, Apex-managed
sharing, Experience Cloud, and Enterprise Territory Management.

This version translates and rewrites the supplied Hinglish course index
in clear English while preserving its 16-topic structure and 238
subtopic concepts. It follows the supplied reference README's
learning-roadmap style without copying unrelated Enterprise Architecture
content.

------------------------------------------------------------------------

## What You'll Learn

The goal is to understand how Salesforce determines access to records,
fields, and users---and how to design, troubleshoot, and scale that
access model.

``` text
Users and Licenses
        ↓
Object Permissions and Field-Level Security
        ↓
Org-Wide Defaults (OWD)
        ↓
Role Hierarchy and Implicit Sharing
        ↓
Sharing Rules, Teams, Groups, and Queues
        ↓
Apex-Managed Sharing
        ↓
Experience Cloud and Territory Management
        ↓
Privacy, Consent, Security, and Production Readiness
```

**Course at a glance:** 16 topics · 238 subtopics · English

------------------------------------------------------------------------

## Course Index

1.  Sharing & Visibility --- Who Can See Which Records, and How
2.  Designer-Level Deep Dive --- Licenses, the Real Meaning of OWD, and
    Profile-versus-OWD Decisions
3.  Horizontal Sharing --- Sharing Rules, Share Tables, Manual Sharing,
    and Login/IP Restrictions
4.  How the Data Model Affects Sharing --- External Objects, List Views,
    Files, and Relationships
5.  From User Setup to Territories --- User Records, Licenses, Teams,
    Roles, and Enterprise Territory Management
6.  Programmatic Sharing --- Apex Sharing Keywords, Visualforce, and
    Trigger Security
7.  The Bigger Salesforce Security Picture --- Health Check, Shield,
    Audit, MFA/SSO, and Code Vulnerabilities
8.  Hands-On Setup --- Account Teams, Master-Detail Inheritance, Sharing
    Buttons, and Flow-to-Apex Sharing
9.  Revision and Edge Cases --- Primary Master, OWD Change Direction,
    Row Causes, and Inherited Sharing
10. Scenario-Solving Playbook --- Sharing Toolkit, Partner Roles, and
    Requirement Breakdown
11. Experience Cloud Sharing --- Licenses, High-Volume Users, Sharing
    Sets, Share Groups, and Super User Access
12. CTA Review Board Practice --- Data Modeling, Large Data Volumes, and
    Sharing Trade-Offs
13. Data Privacy & Consent Management --- GDPR/CCPA, Data
    Classification, and Layered Consent
14. Practical Apex Sharing Playbook --- Coded Scenarios, Production
    Hardening, Tests, and Deferred Calculations
15. Enterprise Territory Management Setup --- Models, Type Priority,
    Forecasting, and Assignment Gotchas
16. User Sharing --- Who Can See Which Users? User OWD, Group
    Membership, and Visibility Settings

------------------------------------------------------------------------

## Architecture Decision Framework

Use this sequence when solving an access or sharing requirement:

``` text
Business Requirement
        ↓
Users, Licenses, and Data
        ↓
Object Permissions and Field-Level Security
        ↓
Baseline Access with OWD
        ↓
Role Hierarchy and Implicit Sharing
        ↓
Declarative Sharing, Teams, Groups, and Queues
        ↓
Apex-Managed Sharing, if required
        ↓
Security, Scale, Testing, and Operations
        ↓
Documented Decision and Trade-offs
```

### Questions to ask for every design

-   What business problem are we solving?
-   Which users need access, and which licenses do they have?
-   Is the requirement about object access, field visibility, record
    access, or user visibility?
-   What should the baseline access be for a user who does not own a
    record?
-   Can declarative sharing, role hierarchy, teams, groups, or queues
    satisfy the requirement?
-   If Apex sharing is needed, how will changes, ownership transfers,
    recalculation, and testing be handled?
-   What are the privacy, performance, maintainability, and
    large-data-volume implications?

------------------------------------------------------------------------

## Complete Topic Roadmap

## 01. Sharing & Visibility --- Who Can See Which Records, and How

The complete Salesforce data-access model, from OWD and sharing rules to
role hierarchy, implicit sharing, field-level security, permission sets,
teams, Experience Cloud, and Apex sharing, with exam-style scenarios.

### Subtopics

-   Sharing architecture foundations --- every record has an owner
-   How user licenses affect sharing --- full sharing versus high-volume
    users
-   Org-Wide Defaults (OWD) --- the baseline access restriction
-   OWD access levels --- from Private to Public Full Access, including
    reporting implications
-   Sharing rules --- owner-based, criteria-based, and guest-user rules
-   Sharing overrides --- View All Data and Modify All Data
-   Manual sharing --- sharing an individual record by hand
-   Role hierarchy --- access for managers, not automatically for peers
-   Implicit sharing --- automatic behaviour for Accounts, Contacts,
    Opportunities, and Cases
-   Profiles --- object permissions, tabs, and the difference between
    role and profile
-   Field-Level Security versus page layouts
-   Permission sets --- adding access beyond a profile, including five
    types
-   Permission Set Groups --- bundling multiple permission sets
-   Public groups, manager groups, and queues --- grouping users
-   Account, Opportunity, and Case teams --- cross-team collaboration
-   Experience Cloud sharing --- sharing sets and high-volume users
-   Programmatic (Apex-managed) sharing --- share objects and row causes
-   Exam-style scenarios --- seven requirements and their solutions

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## 02. Designer-Level Deep Dive --- Licenses, the Real Meaning of OWD, and Profile-versus-OWD Decisions

The Sharing & Visibility Designer certification blueprint, the
grant-not-block mindset, internal versus external users, OWD edge cases
and recalculation, profile anatomy, View All versus View All Data,
permission-set truth tables, and real project debates.

### Subtopics

-   Sharing & Visibility Designer exam blueprint --- audience,
    weighting, and focus areas
-   The key mindset --- sharing grants access; it does not block access
-   User licenses --- the first boundary of sharing
-   The real definition of OWD --- what applies when you are not the
    owner
-   Internal versus external users --- licenses and the rule to plan the
    roadmap first
-   OWD values in depth --- Private, Read Only, Read/Write, Transfer,
    Controlled by Parent, and Full Access
-   Calendar and activity access --- four levels and a sensitive
    real-world risk
-   Price Book and User objects --- two special OWD cases
-   Salesforce recommendations versus sales reality --- a genuine
    conflict
-   Sharing recalculation --- changes may not apply instantly
-   OWD limitations --- restrictions when changing from Private to
    Public and Apex-sharing implications
-   Profile anatomy --- apps, tabs, objects, fields, layouts, and admin
    permissions
-   View All versus View All Data --- object-level versus org-wide
    access
-   Where should a field be mandatory? Object, page layout, or
    validation rule
-   Profile versus permission-set strategy --- base access versus
    additional access, with a truth table
-   Permission-set limitations --- page layouts, license binding, and
    audit trail
-   Which one takes precedence? Six combinations of OWD and profile
    permissions
-   Real project scenarios and debates --- hierarchy changes,
    multi-country rollout, and sensitive fields

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## 03. Horizontal Sharing --- Sharing Rules, Share Tables, Manual Sharing, and Login/IP Restrictions

Understand vertical versus horizontal access, choose owner-based or
criteria-based rules, inspect share-object backends, debug manual
sharing, use recalculation and share locks effectively, and distinguish
login hours, IP ranges, and business hours.

### Subtopics

-   Why vertical access is not enough --- two common gaps
-   Three questions for every sharing rule: what, who, and how much
    access
-   Owner-based versus criteria-based rules --- the 300-versus-50 limit
-   Sharing targets and the one-way-sharing gotcha
-   Related-record cascades --- which records may become accessible
-   Criteria-based rules are case-sensitive --- a common silent failure
-   Behind the scenes --- AccountShare and custom-object share tables
-   Manual sharing UI and Expand List --- powerful access-debugging
    tools
-   Recalculate controls and object-specific share locks
-   A four-step framework for choosing a sharing approach
-   Six important sharing-rule behaviours --- permissive access,
    inactive users, and expired packages
-   Behind login --- four checks between entering credentials and
    reaching the home page
-   Login hours --- profile-level time restrictions and what happens at
    the cutoff
-   IP-range confusion --- Network Access, profile login IPs, and
    per-request enforcement
-   Business hours and holidays versus login hours
-   Session Q&A and takeaways --- record-type criteria, group IDs, and
    quick recall

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## 04. How the Data Model Affects Sharing --- External Objects, List Views, Files, and Relationships

Covers Salesforce Connect and external objects, external versus indirect
lookups, list-view visibility, the three Salesforce Files sharing
states, and how junction-object security is inherited from master
relationships.

### Subtopics

-   External data sources and external objects --- data that stays
    outside Salesforce
-   External Lookup versus Indirect Lookup versus Lookup
-   Custom list views --- two profile permissions that affect visibility
-   List-view visibility options and a hidden portal-user risk
-   Sharing a list view is not the same as sharing its records
-   Salesforce Files (ContentDocument) --- three sharing states
-   Ways to share files --- users, groups, records, and public links
-   Files versus Classic Attachments --- Salesforce is not a
    content-management system
-   Master-detail versus lookup --- direct security implications
-   Junction objects and many-to-many relationships --- security from
    both masters, presentation from the primary master
-   Session 4 quick review --- everything in one table

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## 05. From User Setup to Territories --- User Records, Licenses, Teams, Roles, and Enterprise Territory Management

Covers authentication versus authorization,
internal/external/integration users, user-record fields, user versus
feature licenses, manager and delegated approver fields, team types,
role-based exclusive access, and Territory Management 1.0 versus 2.0.

### Subtopics

-   Authentication versus authorization --- where sharing fits
-   Three user types --- internal, external, and integration users
-   User-record anatomy --- how fields affect sharing and automation
-   User license versus feature license --- parent and add-on
    relationship
-   Manager field and delegated approver --- small fields with large
    effects
-   Four types of teams --- manager-based views, default teams, and
    record teams
-   Role hierarchy --- two design principles and the OWD prerequisite
-   The hidden power of roles --- Contact, Opportunity, and Case access
-   Scenario A --- use role access instead of a sharing rule in
    professional services
-   Scenario B --- why Edit All Cases can be dangerous in telecom
-   Scenario C --- using role access for a GDPR privacy champion
-   Contacts without Accounts and the 10,000-child-record skew problem
-   Enterprise Territory Management --- understand the business before
    the technology
-   Territory hierarchy design --- named, regional, and mixed models
-   Territory Management 1.0 versus Enterprise Territory Management 2.0
-   Exclusive territory access --- two additional powers compared with
    roles
-   Decision tree --- role hierarchy versus territory management

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## 06. Programmatic Sharing --- Apex Sharing Keywords, Visualforce, and Trigger Security

A practical guide to Apex sharing, share-object fields, Apex sharing
reasons, recalculation batch classes, tricky edge cases,
with/without/inherited sharing, inner-class behaviour, Execute
Anonymous, Visualforce controllers, and trigger security myths.

### Subtopics

-   Where programmatic sharing fits --- the final layer of the
    architecture
-   Core concept --- Apex sharing and manual sharing are conceptually
    similar
-   Four share-object fields and a working code example
-   Apex sharing reasons (Row Cause) --- optional, but useful
-   Debug with Expand List --- quickly answer why access was granted
-   Scenario 1 --- Account Plan and Account Team
-   Scenario 2 --- granting access to an Account from a potential
    repeat-sale Lead
-   Scenarios 3 and 4 --- recruiting applications and rule-engine-driven
    sharing
-   Apex sharing recalculation --- the batch class developers often
    forget
-   Four tricky gotchas that matter in exams and production
-   with sharing, without sharing, and inherited sharing --- risks of
    omitting a keyword
-   Cross-class calls --- which sharing context applies?
-   Inner-class trap --- sharing context is not automatically inherited
-   Sharing keywords versus access modifiers
-   Apex class access on profiles --- access to the entry point
-   Execute Anonymous does not simply run as unrestricted system mode
-   Visualforce controller matrix --- standard, extension, and custom
    controllers
-   Trigger myth --- understanding trigger execution context

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## 07. The Bigger Salesforce Security Picture --- Health Check, Shield, Audit, MFA/SSO, and Code Vulnerabilities

Covers security beyond record sharing: phishing, Security Health Check,
field history, Field Audit Trail, Shield, Transaction Security, password
policies, My Domain, SSO, MFA, file security, SOQL injection, XSS, and
CSRF.

### Subtopics

-   Six security areas and a recap of the sharing stack
-   Phishing and malware --- trust.salesforce.com and your
    responsibilities
-   Security Health Check --- an org security report card
-   Basic audit hygiene --- system information fields, login history,
    and Setup Audit Trail
-   Field History Tracking --- the 20-field limit and a Chatter Feed
    workaround
-   History retention gotcha --- data can disappear after 24 months
-   Salesforce Shield: Platform Encryption and how encryption works
-   Salesforce Shield: Event Monitoring
-   Salesforce Shield: Field Audit Trail and the DIY-trigger trap
-   Transaction Security policies and concurrent session limits
-   Password policies and setting passwords programmatically
-   Duplicate usernames --- documentation versus practical behaviour
-   Expire All Passwords --- what does 'all' actually mean?
-   Visualforce cookies --- available, but use them cautiously
-   My Domain and Single Sign-On --- from branding to security
-   MFA and SSO --- does SSO remove the need for MFA?
-   File upload and download security --- browser display versus
    download
-   SOQL injection --- how a search box can expose database data
-   XSS and CSRF --- script injection and request-forgery risks
-   HTTP versus HTTPS and SSL versus TLS

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## 08. Hands-On Setup --- Account Teams, Master-Detail Inheritance, Sharing Buttons, and Flow-to-Apex Sharing

A click-by-click implementation guide covering Account Teams, team
roles, default teams, AccountTeamMember, master-detail security,
share-object naming, missing Sharing buttons, an Apex/Flow recruiting
scenario, and territory-model setup.

### Subtopics

-   Account Teams --- definition, rules, and when to use them
-   Step by step --- enable Account Teams and configure team roles
-   Adding Account Team Members --- observations from the demo
-   Default Account Teams --- configured per user, not per org
-   AccountTeamMember object --- a useful extension point
-   Master-detail Controlled by Parent --- hands-on demo and terminology
    clarification
-   The key demo lesson --- record access does not replace object
    permissions
-   Why a child object may not expose OwnerId in a query
-   How share-object names are formed and how to inspect them in
    Developer Console
-   Where did the Sharing button go? A three-point checklist
-   Real Apex-managed sharing scenario --- recruiter shares a candidate
    with an interviewer
-   Invocable method pattern --- Apex callable from Flow
-   Calling Apex from a record-triggered Flow
-   Hands-on Enterprise Territory Management --- model, rules, and Run
    Rules
-   Hands-on gotchas checklist

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## 09. Revision and Edge Cases --- Primary Master, OWD Change Direction, Row Causes, and Inherited Sharing

A focused revision of less-obvious details: identify a junction object's
primary master, understand OWD changes in both directions, review
Campaign sharing, manager groups, the Row Cause catalogue, user/system
mode, and inherited-sharing combinations.

### Subtopics

-   External Access column --- default behaviour and irreversible
    enablement caveat
-   Four calendar access levels --- precise meanings from a live demo
-   Find a junction object's primary master using Workbench
    RelationshipOrder
-   OWD change direction matters --- increasing access versus
    restricting access
-   Are some objects always private? What live testing reveals
-   View All Data and Modify All Data depend on license type
-   Can you have Modify All without Delete? Understanding the limitation
-   Core license plus feature license --- an add-on model example
-   Sharing-rule access levels vary by object; Campaign can use Full
    Access
-   Three sharing-rule types and their limits
-   Manager Groups versus Manager Subordinate Groups and disabling
    hierarchy
-   Sharing rules for managed-package fields and expired-package effects
-   Row Cause catalogue --- three categories of sharing
-   Apex sharing on standard objects --- multiple access-level fields
-   External-object relationships --- a one-line decision rule
-   User mode versus system mode --- quick-reference sheet
-   Inherited sharing matrix and its practical value

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## 10. Scenario-Solving Playbook --- Sharing Toolkit, Partner Roles, and Requirement Breakdown

Break down a CTA-style scenario step by step, using a four-layer sharing
toolkit, implicit-sharing direction, role-setting pitfalls, partner role
hierarchies, queues, case assignment rules, and a decision framework for
Apex, manual sharing, and ETM.

### Subtopics

-   Sharing toolkit --- four layers and sixteen declarative tools
-   How to read a scenario without feeling overwhelmed
-   Case study --- the Sunflower Computing environment
-   Scenario 1a --- private Opportunities and the direction of implicit
    sharing
-   Scenarios 1b and 1c --- private Accounts and Contacts controlled by
    parent
-   The major trap --- role options can undermine the OWD design
-   Scenario 2 --- build the role hierarchy top-down and solve bottom-up
-   Partner user role hierarchy --- three roles Salesforce creates
    automatically
-   Scenario 2c --- service hierarchy and respecting requirements that
    are absent
-   Scenario 3 --- private Case OWD and queues for shared ownership
-   Case Assignment Rules --- routing by type/subtype and a catch-all
    queue
-   CTA artifact tips --- representing 127 countries on a whiteboard
-   Decision framework --- Apex versus manual sharing versus Territory
    Management
-   Q&A --- solving multi-level Account hierarchy sharing declaratively
-   Troubleshooting checklist and topic recap

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## 11. Experience Cloud Sharing --- Licenses, High-Volume Users, Sharing Sets, Share Groups, and Super User Access

Explains external-user sharing, five community license types,
high-volume-user restrictions, the opposite directions of Sharing Sets
and Share Groups, and Partner versus Community Plus Super User Access.

### Subtopics

-   Community overview --- how the three environments relate
-   Five community license types --- comparison
-   Login-based versus usage-based licensing and design implications
-   Four community-sharing approaches --- think of them as a funnel
-   High-volume community users --- what they cannot do
-   Enable the external sharing model --- two separate settings
-   Sharing Sets --- give external users access to records related to
    their Account
-   Share Groups --- the reverse direction, making external-user data
    visible internally
-   Setup walkthrough --- create Sharing Sets and Share Groups,
    including propagation delay
-   Super User Access 1 --- Partner Super User, configured through the
    Contact record
-   Super User Access 2 --- Community Plus Super User, configured with a
    permission set
-   Design patterns, practice plan, and topic recap

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## 12. CTA Review Board Practice --- Data Modeling, Large Data Volumes, and Sharing Trade-Offs

Learn from three solutions to one Clean Bikes scenario, review board
expectations, license and hierarchy trade-offs, sharing at 12,000
dealerships, large-data-volume options, and a formula-field workaround
when FLS is insufficient.

### Subtopics

-   CTA review-board format and why there may be no single perfect
    answer
-   Why data-model decisions cause so many failures
-   Clean Bikes scenario --- actors, licenses, and three key decisions
-   Three role-hierarchy designs and automatic partner-role creation
-   Data-model decisions --- Person Accounts, Opportunities versus
    Contracts, and custom objects
-   Lead routing by postal code --- a gap in Territory Management
-   Sharing at scale --- thirty sharing rules may be fine, twelve
    thousand are not
-   Large Data Volume --- complete decision framework
-   Big Objects --- index design, Async SOQL, and archival patterns
-   When FLS is not enough --- a formula-field technique
-   Trade-off thinking and topic recap

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## 13. Data Privacy & Consent Management --- GDPR/CCPA, Data Classification, and Layered Consent

Covers the privacy-regulation landscape, a three-pillar framework, data
classification, a four-layer consent model, Authorization Forms, Legal
Basis, Party Consent, and the Consent API.

### Subtopics

-   Privacy regulations --- developments after GDPR
-   Common principles shared by privacy regulations
-   Three-pillar framework --- find, manage, and control
-   Pillar 1 --- discover data, understand access, and assess risk
-   Data minimization and retention --- data that does not exist cannot
    leak
-   Pillar 2 --- Data Subject Access Requests (DSARs) and access reviews
-   Pillar 3 --- transparency, data rights, and consent preferences
-   Data Classification --- Salesforce governance fields
-   Four consent layers --- from broad to highly specific
-   Legal Basis and Authorization Forms --- consent is not the only
    basis
-   Consent API --- reconciling consent across multiple sources
-   Practical notes, relationship to sharing, and topic recap

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## 14. Practical Apex Sharing Playbook --- Coded Scenarios, Production Hardening, Tests, and Deferred Calculations

Covers when to use Apex sharing, share-object behaviour, record-type
visibility pitfalls, three coded scenarios, production hardening, Apex
security checks, System.runAs, Defer Sharing Calculations, and
guest-user security.

### Subtopics

-   Three sharing approaches and the exam's programmatic-sharing
    emphasis
-   When to use programmatic sharing --- four signals
-   Share objects --- when they exist and which fields they contain
-   Production gotcha --- record types do not control record visibility
-   User-managed versus Apex-managed sharing --- the decisive
    owner-change difference
-   Apex sharing reasons --- custom objects only, with standard-object
    limitations
-   Scenario 1 --- sharing based on lookup fields, with code walkthrough
-   Production hardening for Scenario 1 --- three essential improvements
-   Scenario 2 --- share an Account with public groups
-   Scenario 3 --- dynamic or time-based sharing with Schedulable Apex
-   Enforce Apex security --- sharing, CRUD, and FLS
-   Verify security with unit tests and understand System.runAs rules
-   Operational topics --- Defer Sharing Calculations and guest-user
    security
-   Why Apex sharing should be a last resort, plus topic recap

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## 15. Enterprise Territory Management Setup --- Models, Type Priority, Forecasting, and Assignment Gotchas

A step-by-step ETM guide covering scope, independent territory
hierarchy, territory type priority, model states, forecasting,
opportunity assignment, activation order, inherited assignment, and the
supplied Apex reassignment class.

### Subtopics

-   Territory Management scope --- Accounts only, with a separate path
    for Opportunities
-   Business case --- why use Territory Management instead of
    spreadsheets or custom objects?
-   Territory hierarchy versus role hierarchy --- independent structures
-   The truth about Territory Type Priority --- it is not assignment
    priority
-   Territory models --- four models, three states, and irreversible
    archiving
-   Territory fields, access levels, and permissions
-   Forecasting connection --- territory hierarchy can become the
    forecast hierarchy
-   Six-step build process and the 'activate before Run Rules' gotcha
-   Three assignment approaches --- rule-based, inherited, and manual
-   Opportunity territory assignment and the supplied Apex class
-   Decision guide, study plan, and topic recap

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## 16. User Sharing --- Who Can See Which Users? User OWD, Group Membership, and Visibility Settings

Explains sharing for User records rather than business records: User
OWD, membership-based user sharing rules, manual sharing, external-user
visibility settings, View All Users, effects on mentions and the People
tab, and role access options.

### Subtopics

-   What user sharing is and why it matters
-   User-object OWD --- only two options
-   User sharing rules --- the unique 'based on group membership' rule
    type
-   Manual sharing of a User record --- granular control
-   User Visibility settings --- special handling for portal/community
    users
-   View All Users permission --- the master key for User-object
    visibility
-   Complete scenario --- three requirements from AW Computing
-   Practical effects on the People tab, @mentions, and Chatter groups
-   Role-hierarchy access options --- clarifying a common confusion
-   OWD determines which role options appear
-   Profile permissions remain the ceiling --- proof through six
    combinations
-   Queues --- practical notes
-   Design guide and topic recap

### Revision checklist

-   [ ] Explain the core concept in your own words.
-   [ ] Review the scenarios and practical edge cases.
-   [ ] Compare the available design options and trade-offs.
-   [ ] Practise troubleshooting or interview-style questions where
    applicable.

------------------------------------------------------------------------

## Revision and Interview Preparation

Review each topic through the following progression:

``` text
Concept
   ↓
Simple Explanation
   ↓
Salesforce Behaviour and Configuration
   ↓
Scenario-Based Application
   ↓
Edge Cases and Common Mistakes
   ↓
Architecture Trade-offs
   ↓
Interview and Exam Recall
```

Always distinguish:

-   **Object permissions** from **record-level sharing**.
-   **Field-Level Security (FLS)** from page-layout behaviour.
-   **OWD baseline access** from access granted through hierarchy,
    sharing rules, teams, or Apex.
-   **Internal users** from **external and high-volume users**,
    including licensing constraints.
-   **User visibility** from access to business records.
-   Sharing a **list view or file** from sharing the underlying record.
-   **Declarative approaches** from **programmatic approaches**,
    including their operational and scale trade-offs.

------------------------------------------------------------------------

## Production and Design Review Checklist

-   [ ] Document the business requirement and expected access outcome
-   [ ] Check user licenses and external-user constraints
-   [ ] Validate object permissions and Field-Level Security
-   [ ] Choose appropriate OWD baseline access
-   [ ] Understand role-hierarchy and implicit-sharing effects
-   [ ] Evaluate sharing rules, teams, groups, queues, and manual
    sharing
-   [ ] Use Apex-managed sharing only when appropriate
-   [ ] Consider sharing recalculation, ownership changes, and
    exceptions
-   [ ] Evaluate large data volumes and sharing-lock risks
-   [ ] Review privacy, consent, audit, and security implications
-   [ ] Test with representative users and realistic scenarios
-   [ ] Document the design decision and its trade-offs

------------------------------------------------------------------------

## Definition of Done

A topic is ready for revision when you can:

-   Explain the concept clearly.
-   Describe the relevant Salesforce behaviour and configuration.
-   Apply the concept to a realistic business requirement.
-   Identify limitations, edge cases, and common mistakes.
-   Compare alternatives and justify a design choice.
-   Explain how to test and troubleshoot the outcome.

------------------------------------------------------------------------

## Repository Principle

> Do not just memorize sharing features. Understand the access
> requirement, choose the right security layer, prefer the simplest
> maintainable solution, and validate its behaviour in realistic
> scenarios.

## Source and Scope

-   The topic structure and subject matter come from the supplied
    Salesforce course index.
-   The English headings and descriptions are translated and rewritten
    for readability; they are not intended as a verbatim copy of the
    Hinglish phrasing.
-   This file is a roadmap and index, not the full lesson transcript for
    every subtopic.
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