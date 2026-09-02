[Home](../index.md) / [01 · SF Basics](index.md) / **Standard Objects**

# Standard Objects

9 topics · Series 1: SF Basics

**Topics on this page**

- [Account](#account)
- [Contact](#contact)
- [Lead](#lead)
- [Opportunity](#opportunity)
- [Case](#case)
- [Campaign](#campaign)
- [Task](#task)
- [Event](#event)
- [User](#user)

## Account

*A company/organization (or person in B2C) — the central hub of the CRM data model.*

### 🌱 Simple

*Beginner - plain language*

An **Account** represents a company or organization you do business with. It's the anchor record — contacts, opportunities, and cases all link back to an account.

### 📏 Limits

*Governor & platform limits*

- More than 10,000 children under one Account causes data skew and lock contention.
- Implicit sharing to Contacts, Cases and Opportunities cannot be disabled.
- Person Accounts, once enabled, cannot be turned off.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Contact

*A person, usually linked to an Account — the people you interact with.*

### 🌱 Simple

*Beginner - plain language*

A **Contact** is an individual person, normally associated with an Account (company). It stores name, email, phone, role, etc. Contacts are who you actually talk to.

### 📏 Limits

*Governor & platform limits*

- Contacts to Multiple Accounts uses a junction object with its own sharing behaviour.
- Contact access is implicitly granted through the parent Account.
- Email Opt Out and bounce status suppress future sends silently.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Lead

*An unqualified prospect — captured, qualified, then converted to Account/Contact/Opportunity.*

### 🌱 Simple

*Beginner - plain language*

A **Lead** is a potential customer you haven't qualified yet — someone who filled a web form or attended an event. Once qualified, you **convert** the lead into an Account, Contact, and (optionally) an Opportunity.

### 📏 Limits

*Governor & platform limits*

- Conversion is one-way and cannot be undone without custom work.
- Conversion follows its own execution path - trigger behaviour differs from a normal update.
- Custom field mapping to Account, Contact and Opportunity must be configured explicitly.
- Converted Leads become read-only.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Opportunity

*A potential deal/sale with an amount, stage and close date — the heart of the sales pipeline.*

### 🌱 Simple

*Beginner - plain language*

An **Opportunity** is a potential sale you're working — a deal. It has an amount, a stage (e.g., Prospecting → Closed Won), and a close date. The sum of open opportunities is your pipeline.

### 📏 Limits

*Governor & platform limits*

- Stage and probability are linked through Sales Process, which is tied to Record Type.
- Amount is roll-up-driven when Products are used and cannot be edited directly.
- Forecast categories are derived from stage and are not freely editable.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Case

*A customer support ticket/issue — the core of Service Cloud.*

### 🌱 Simple

*Beginner - plain language*

A **Case** is a customer issue or request — a support ticket. It tracks the problem, who's working it, status, and resolution. Cases are the heart of Service Cloud.

### 📏 Limits

*Governor & platform limits*

- Assignment, escalation and auto-response rules each allow only one active rule per object.
- Entitlement and milestone timing depends on Business Hours, which carry their own timezone.
- Case comments and emails count toward data storage quickly at volume.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Campaign

*A marketing initiative (email, event, webinar) tracking members and ROI/influence.*

### 🌱 Simple

*Beginner - plain language*

A **Campaign** represents a marketing effort — an email blast, webinar, or trade show. It tracks who you targeted (**campaign members**: leads/contacts) and how it influenced sales.

### 📏 Limits

*Governor & platform limits*

- CampaignMember is a junction with its own record limits at very high volume.
- Campaign hierarchy is limited to 5 levels.
- Campaign influence models are limited and partly configurable.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Task

*A to-do / activity (call, email, follow-up) with a due date — polymorphic to many records.*

### 🌱 Simple

*Beginner - plain language*

A **Task** is a to-do item — "call this customer," "send proposal." It has a subject, due date, status, and is linked to a record (an account, contact, opportunity, etc.).

### 📏 Limits

*Governor & platform limits*

- Activities are stored in a shared underlying table - high volume affects both Tasks and Events.
- WhoId and WhatId are polymorphic, which limits reporting and query flexibility.
- Archived activities (older than 365 days by default) are excluded from most views.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Event

*A calendared activity with start/end time (meeting, call) — the other half of Activities.*

### 🌱 Simple

*Beginner - plain language*

An **Event** is a scheduled activity with a start and end time — a meeting or call on the calendar — linked to records like contacts and opportunities.

### 📏 Limits

*Governor & platform limits*

- Shares the Activity table with Tasks, so volume affects both.
- Recurring events create individual child records and multiply storage.
- Polymorphic WhoId/WhatId restricts reporting options.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## User

*A licensed person who logs in — drives security (profile, roles, perm sets, ownership).*

### 🌱 Simple

*Beginner - plain language*

A **User** is a person with a login to Salesforce. Each user has a profile, role, permission sets, and a license that determine what they can see and do. Records are *owned* by users.

### 📏 Limits

*Governor & platform limits*

- Users cannot be deleted, only deactivated - the licence is freed but the record remains.
- User is a setup object, so Mixed DML rules apply in Apex.
- Deactivating a user stops any scheduled jobs they own.
- Licence counts are contractual and enforced.

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
