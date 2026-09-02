[Home](../index.md) / [03 · SF Admin](index.md) / **Security Basics**

# Security Basics

6 topics · Series 3: SF Admin

**Topics on this page**

- [Profiles](#profiles)
- [Permission Sets](#permission-sets)
- [Permission Set Groups](#permission-set-groups)
- [Roles](#roles)
- [OWD](#owd)
- [Sharing Rules](#sharing-rules)

## Profiles

*The baseline of what a user CAN do — object/field permissions, plus settings & defaults.*

### 🌱 Simple

*Beginner - plain language*

A **profile** defines a user's baseline permissions: which objects/fields they can see or edit, what apps/tabs they get, login hours, and more. Every user has **exactly one** profile.

### 📏 Limits

*Governor & platform limits*

- One profile per user; permission sets only additive; some settings live only on profiles; profile being slimmed over releases.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Permission Sets

*Additive bundles of permissions assigned on top of a profile — grant access flexibly.*

### 🌱 Simple

*Beginner - plain language*

A **permission set** is a reusable bundle of permissions you assign *on top of* a profile to grant extra access — without changing the profile or creating a new one. A user can have many.

### 📏 Limits

*Governor & platform limits*

- Additive only; some require permission set licenses; assignment limits; can't reduce profile baseline.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Permission Set Groups

*Bundles of permission sets representing a job role — assign one group, get all access.*

### 🌱 Simple

*Beginner - plain language*

A **permission set group** bundles several permission sets together to match a **job role**. Assign the one group and the user gets every permission inside — simpler than assigning many sets individually.

### 📏 Limits

*Governor & platform limits*

- Composed of permission sets; muting only subtracts within the group; recalculation needed after changes.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Roles

*Position in the role hierarchy — controls record visibility upward via sharing.*

### 🌱 Simple

*Beginner - plain language*

A **role** places a user in the **role hierarchy**, which controls *record visibility*: managers (higher roles) can see records owned by people below them. Roles are about **data access**, not feature permissions (that's profiles/permission sets).

### 📏 Limits

*Governor & platform limits*

- Roles affect visibility not CRUD; hierarchy depth impacts recalculation; "grant access using hierarchies" toggle per custom object.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## OWD

*Org-Wide Defaults — the baseline record access (Private / Public R / Public R-W) per object.*

### 🌱 Simple

*Beginner - plain language*

**Org-Wide Defaults (OWD)** set the *baseline* sharing for each object: **Private**, **Public Read Only**, or **Public Read/Write**. It's the most restrictive starting point; everything else (roles, sharing rules) only opens access *up* from here.

### 📏 Limits

*Governor & platform limits*

- Baseline only (additive layers grant more); tightening recalculates org-wide; child objects "controlled by parent".

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Sharing Rules

*Automatically grant record access laterally based on owner or criteria — open up Private OWD.*

### 🌱 Simple

*Beginner - plain language*

**Sharing rules** automatically give extra users access to records they don't own — based on the record's **owner** (owner-based) or its **field values** (criteria-based). They open up a Private/Read-Only OWD; they never restrict.

### 📏 Limits

*Governor & platform limits*

- Maximum 300 per object, of which 50 may be criteria-based.
- Rules grant access only - they can never remove it.
- Changes trigger asynchronous recalculation that locks group membership.
- Criteria-based rules re-evaluate on every relevant field change.

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
