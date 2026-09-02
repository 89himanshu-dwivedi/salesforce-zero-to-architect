[Home](../index.md) / [01 · SF Basics](index.md) / **Data Basics**

# Data Basics

7 topics · Series 1: SF Basics

**Topics on this page**

- [Object](#object)
- [Field](#field)
- [Record](#record)
- [Tab](#tab)
- [App](#app)
- [Related List](#related-list)
- [Record Page](#record-page)

## Object

*A database table — defines a type of data (Account, Contact, custom). Holds records.*

### 🌱 Simple

*Beginner - plain language*

An **object** is like a spreadsheet/table that holds one kind of information. *Account* is an object for companies, *Contact* for people. Each row in it is a **record**; each column is a **field**.

### 📏 Limits

*Governor & platform limits*

- Custom object limits are edition-based: 200 (Enterprise) to 2,000 (Unlimited).
- Big Objects and External Objects have separate allocations.
- Deleted objects hold their slot until purged from the Recycle Bin.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Field

*A column on an object — typed data (text, number, picklist, formula, relationship).*

### 🌱 Simple

*Beginner - plain language*

A **field** is a single piece of information on a record — like a column. *Account Name*, *Phone*, and *Industry* are fields on the Account object. Each field has a type that controls what data it accepts.

### 📏 Limits

*Governor & platform limits*

- Roughly 800 custom fields per object (500 in some editions).
- Roll-up summaries are limited to 25 per object and counted separately.
- Deleted fields hold their slot for 15 days.
- Wide objects slow every query, report and page load.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Record

*A single row/entry on an object — one Account, one Contact. Owned and shared.*

### 🌱 Simple

*Beginner - plain language*

A **record** is one entry on an object — one specific company in Account, one specific person in Contact. It has a unique **Id** and an **Owner**.

### 📏 Limits

*Governor & platform limits*

- Most records consume 2 KB of data storage regardless of field count.
- Deleted records sit in the Recycle Bin for 15 days.
- Record Ids are org-specific and regenerate on sandbox refresh.
- 15-character Ids are case-sensitive; 18-character are not.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Tab

*A navigation entry that surfaces an object/page in an app's nav bar or App Launcher.*

### 🌱 Simple

*Beginner - plain language*

A **tab** is a clickable item that opens an object's list (or a web/VisualForce/Lightning page) in the navigation bar. The "Accounts" tab takes you to account records.

### 📏 Limits

*Governor & platform limits*

- Custom tab counts are limited by edition and tab type.
- Tabs must be exposed per profile or permission set - creating one is not enough.
- Managed package tabs have their own allocation.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## App

*A branded collection of tabs/items for a persona — Sales, Service, or custom Lightning apps.*

### 🌱 Simple

*Beginner - plain language*

An **app** in Salesforce is a bundle of tabs and items grouped for a job — like the Sales app or Service app. Users switch apps from the App Launcher to get a focused set of tools.

### 📏 Limits

*Governor & platform limits*

- Custom app counts are edition-based.
- Each app carries its own navigation, utility bar and page assignments to maintain.
- Visibility is controlled per profile or permission set.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Related List

*Child records shown on a parent record page — Contacts on an Account, Cases on a Contact.*

### 🌱 Simple

*Beginner - plain language*

A **related list** shows records connected to the one you're viewing. On an Account page you see its related Contacts, Opportunities, and Cases — the "children" linked to that account.

### 📏 Limits

*Governor & platform limits*

- Each related list on a layout is a separate query on page load.
- Related lists show a limited number of rows before requiring "View All".
- Subject to query selectivity rules on large child sets.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Record Page

*The Lightning page that displays a record — assembled from components via App Builder.*

### 🌱 Simple

*Beginner - plain language*

A **record page** is the screen you see when you open a record. In Lightning it's built by dragging components (details, related lists, activity, custom LWCs) onto the page using the Lightning App Builder.

### 📏 Limits

*Governor & platform limits*

- Component count and related lists drive EPT far more than Apex does.
- Assignments are per app, record type and profile - drift between them is common.
- Mobile pages must be assigned separately.

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
