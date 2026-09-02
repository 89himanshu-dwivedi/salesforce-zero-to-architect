[Home](../index.md) / [03 · SF Admin](index.md) / **Activity Management**

# Activity Management

3 topics · Series 3: SF Admin

**Topics on this page**

- [Tasks](#tasks)
- [Events](#events)
- [Calendars](#calendars)

## Tasks

*Trackable to-dos linked to records — follow-ups, calls, with due dates and status.*

### 🌱 Simple

*Beginner - plain language*

A **Task** is a to-do item — "Call customer", "Send proposal" — with a due date, status, and priority, usually linked to a record (Account, Contact, Opportunity). It tracks *what needs doing* and whether it's done.

### 📏 Limits

*Governor & platform limits*

- Shared Activity object with Events; WhoId/WhatId polymorphic; high-volume object (archiving matters); Shared Activities for multiple contacts.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Events

*Calendared activities with start/end times — meetings, calls — shown on calendars and timelines.*

### 🌱 Simple

*Beginner - plain language*

An **Event** is a scheduled activity with a **start and end time** — a meeting, demo, or call. Unlike a task (a to-do), an event sits on a **calendar** and can have invitees.

### 📏 Limits

*Governor & platform limits*

- Shared Activity object; start/end DateTime + invitees; external sync via EAC/Lightning Sync; high-volume; TZ stored in UTC.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Calendars

*Visual scheduling surface for events; object/shared calendars surface any date field.*

### 🌱 Simple

*Beginner - plain language*

The **Calendar** shows your events (and others' shared calendars) in day/week/month views. You can also create **object calendars** that plot any record's date field — like Close Dates or campaign dates — on a calendar.

### 📏 Limits

*Governor & platform limits*

- Object calendars need a date field + list view; sharing for others' calendars; Scheduler for advanced booking; TZ per user.

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
