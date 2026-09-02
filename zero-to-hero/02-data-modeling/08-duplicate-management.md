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
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Duplicate Rules

*Define the action when a match is found — allow, alert, or block — plus reporting.*

### 🌱 Simple

*Beginner - plain language*

A **duplicate rule** decides what happens when a matching rule finds a potential duplicate: **allow** (with a warning), **block** (prevent saving), and whether to **report** it. It's the action layer on top of matching rules.

### 📏 Limits

*Governor & platform limits*

- Active duplicate rules per object; depends on active matching rule; doesn't merge existing dupes; API enforcement settings.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

---

## Connect

These pages carry the **definitions and limits** only. The advanced depth, real-world
scenarios, error playbooks, best-option reasoning and interview questions are kept aside.

If you would like them, or you want to talk about anything on this page:

- **LinkedIn** - [in/himanshukumar-sf](https://www.linkedin.com/in/himanshukumar-sf/)
- **X** - [@kum60094](https://x.com/kum60094)
- **GitHub** - [89himanshu-dwivedi](https://github.com/89himanshu-dwivedi)
- **Email** - [himanshu.jee.1996@gmail.com](mailto:himanshu.jee.1996@gmail.com)

*- Himanshu Kumar*
