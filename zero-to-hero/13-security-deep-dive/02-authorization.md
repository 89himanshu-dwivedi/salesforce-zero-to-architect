[Home](../index.md) / [13 · Security Deep Dive](index.md) / **Authorization**

# Authorization

9 topics · Series 13: Security Deep Dive

**Topics on this page**

- [Object Level CRUD](#object-level-crud)
- [Field Level Security (FLS)](#field-level-security-fls)
- [OWD](#owd)
- [Role Hierarchy](#role-hierarchy)
- [Sharing Rules](#sharing-rules)
- [Teams](#teams)
- [Territory Management](#territory-management)
- [Manual Sharing](#manual-sharing)
- [Apex Sharing](#apex-sharing)

## Object Level CRUD

*Permissions controlling object-level Create/Read/Update/Delete.*

### 🌱 Simple

*Beginner - plain language*

**Object-level CRUD** controls whether a user can Create, Read, Update, or Delete records of an object — set via **profiles** and **permission sets**. It's the first gate of authorization.

### 📏 Limits

*Governor & platform limits*

- Not enforced automatically in Apex system mode.
- Permission sets are additive - only muting sets can remove.
- "View All" and "Modify All" bypass sharing entirely and should be tightly controlled.
- Profile deployments are partial; pair with permission sets.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Field Level Security (FLS)

*Per-field read/edit permissions.*

### 🌱 Simple

*Beginner - plain language*

**Field-Level Security (FLS)** controls whether a user can see or edit specific *fields* on an object — even if they have object CRUD. Set per field on profiles/permission sets.

### 📏 Limits

*Governor & platform limits*

- Not enforced in Apex system mode - use `WITH USER_MODE` or `stripInaccessible`.
- Cannot be set on required or master-detail fields.
- Formula fields inherit access from referenced fields.
- Applies to reports, list views, exports and the API.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## OWD

*Org-Wide Defaults — baseline record visibility.*

### 🌱 Simple

*Beginner - plain language*

**Org-Wide Defaults (OWD)** set the *baseline* record-level access for each object — the most restrictive starting point (Private, Public Read Only, Public Read/Write) that sharing then *opens up*.

### 📏 Limits

*Governor & platform limits*

- Changing OWD triggers org-wide sharing recalculation that can take hours.
- Child OWD cannot be more permissive than the parent in master-detail.
- External OWD is set separately for Experience Cloud users.
- Cannot be changed safely during business hours in a large org.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Role Hierarchy

*Vertical access: managers see subordinates' records.*

### 🌱 Simple

*Beginner - plain language*

The **role hierarchy** grants users access to records owned by people *below* them in the hierarchy — so managers automatically see their subordinates' data, regardless of OWD.

### 📏 Limits

*Governor & platform limits*

- Recommended maximum 500 roles and 10 levels.
- Role changes trigger asynchronous sharing recalculation for the branch.
- Deep hierarchies make report row-level filtering expensive.
- Roles grant read-up access only, never lateral access.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Sharing Rules

*Lateral access grants by ownership or criteria.*

### 🌱 Simple

*Beginner - plain language*

**Sharing rules** open record access *laterally* — granting groups/roles access to records they wouldn't get from the hierarchy, based on the record's **owner** or its **field criteria**.

### 📏 Limits

*Governor & platform limits*

- Maximum 300 per object, of which 50 may be criteria-based.
- Criteria-based rules re-evaluate on every relevant field change.
- Rules can only grant access, never remove it.
- Rule changes trigger recalculation that locks group membership.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Teams

*Account/Opportunity/Case teams sharing records with collaborators.*

### 🌱 Simple

*Beginner - plain language*

**Teams** (Account Team, Opportunity Team, Case Team) let a record owner add collaborators who get specific access to that record — sharing with people working together on it.

### 📏 Limits

*Governor & platform limits*

- Available on Account, Opportunity and Case only.
- Team member limits apply per record (typically a few hundred).
- Team-based access is recalculated on membership change.
- Team access cannot exceed the member's object permissions.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Territory Management

*Enterprise Territory Management for territory-based access.*

### 🌱 Simple

*Beginner - plain language*

**Enterprise Territory Management** grants record access based on **sales territories** — modeling who covers which accounts by geography, product, or other criteria, and sharing those accounts/opportunities accordingly.

### 📏 Limits

*Governor & platform limits*

- Enterprise Territory Management has its own hierarchy limits and recalculation cost.
- Territory recalculation can run for hours in large orgs.
- Only one active territory model at a time.
- Adds a second hierarchy on top of roles, doubling access-evaluation complexity.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Manual Sharing

*Ad-hoc record sharing by owners/admins.*

### 🌱 Simple

*Beginner - plain language*

**Manual sharing** lets a record owner (or admin) grant a specific user or group access to an individual record via the "Sharing" button — a one-off, ad-hoc grant.

### 📏 Limits

*Governor & platform limits*

- Available only when OWD is more restrictive than Public Read/Write.
- Removed automatically when record ownership changes.
- Not available on all objects and not visible in Lightning for every object.
- Does not scale - it is a per-record exception mechanism.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Apex Sharing

*Programmatic managed sharing via share objects.*

### 🌱 Simple

*Beginner - plain language*

**Apex managed sharing** grants record access programmatically by inserting **share records** (e.g., `AccountShare`) — for sharing logic too complex for declarative rules.

### 📏 Limits

*Governor & platform limits*

- Requires a `RowCause` defined as an Apex Sharing Reason on custom objects.
- Not recalculated automatically - your code owns the lifecycle.
- Share records count toward data storage.
- Only works when OWD is Private or Public Read Only.

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
