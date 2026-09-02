[Home](../index.md) / [02 · Data Modeling](index.md) / **Duplicate Management**

# Duplicate Management

2 topics · Series 2: Data Modeling

**Topics on this page**

- [Matching Rules](#matching-rules)
- [Duplicate Rules](#duplicate-rules)

## Matching Rules

*Define how Salesforce decides two records are duplicates (which fields, fuzzy vs exact).*

### 🌱 Simple

*Beginner - plain language*

A **matching rule** tells Salesforce *how* to recognize duplicates — e.g., "same email" or "similar name + same company". It defines the criteria; the duplicate rule then decides what to do about matches.

### 📏 Limits

*Governor & platform limits*

- Active matching rules per object; fields per rule; match-key index cost; native only single-object focus.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Duplicate Rules

*Define the action when a match is found — allow, alert, or block — plus reporting.*

### 🌱 Simple

*Beginner - plain language*

A **duplicate rule** decides what happens when a matching rule finds a potential duplicate: **allow** (with a warning), **block** (prevent saving), and whether to **report** it. It's the action layer on top of matching rules.

### 📏 Limits

*Governor & platform limits*

- Active duplicate rules per object; depends on active matching rule; doesn't merge existing dupes; API enforcement settings.

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
