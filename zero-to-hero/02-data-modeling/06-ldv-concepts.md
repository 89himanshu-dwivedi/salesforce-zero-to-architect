[Home](../index.md) / [02 · Data Modeling](index.md) / **LDV Concepts**

# LDV Concepts

3 topics · Series 2: Data Modeling

**Topics on this page**

- [Data Skew](#data-skew)
- [Ownership Skew](#ownership-skew)
- [Account Skew](#account-skew)

## Data Skew

*Too many child records under one parent — causes lock contention and slow performance.*

### 🌱 Simple

*Beginner - plain language*

**Data skew** happens when a single parent record has a huge number of children (e.g., 10,000+). Updating many children at once forces them to lock the same parent, causing errors and slowness.

### 📏 Limits

*Governor & platform limits*

- Threshold is around 10,000 child records per parent.
- Causes `UNABLE_TO_LOCK_ROW` under concurrent load.
- Cannot be fixed by indexing - only by distributing the data.
- Amplified by implicit sharing on Account children.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Ownership Skew

*One user/queue owns a huge number of records — slows sharing recalculation and causes locks.*

### 🌱 Simple

*Beginner - plain language*

**Ownership skew** is when a single user (or queue) owns a very large number of records — typically over ~10,000. Operations that recalculate sharing for that owner become slow and can lock.

### 📏 Limits

*Governor & platform limits*

- More than 10,000 records owned by a single user.
- Ownership changes trigger expensive sharing recalculation.
- Placing the owner outside the role hierarchy reduces recalculation scope.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Account Skew

*A specific lookup/data skew: too many child records (contacts, cases…) under one Account.*

### 🌱 Simple

*Beginner - plain language*

**Account skew** is data skew on the Account object — one account has an enormous number of related records (contacts, cases, opportunities). Bulk updates to those children contend for the same account lock.

### 📏 Limits

*Governor & platform limits*

- More than 10,000 children under one Account.
- Implicit sharing to Contacts, Cases and Opportunities amplifies the cost.
- Degrades sharing recalculation, reporting and every child save on that branch.

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
