[Home](../index.md) / [13 · Security Deep Dive](index.md) / **Apex Security**

# Apex Security

7 topics · Series 13: Security Deep Dive

**Topics on this page**

- [with sharing](#with-sharing)
- [without sharing](#without-sharing)
- [inherited sharing](#inherited-sharing)
- [USER_MODE](#user-mode)
- [SYSTEM_MODE](#system-mode)
- [stripInaccessible](#stripinaccessible)
- [WITH SECURITY_ENFORCED](#with-security-enforced)

## with sharing

*Enforce the running user's record sharing in Apex.*

### 🌱 Simple

*Beginner - plain language*

**`with sharing`** on an Apex class tells the platform to **enforce the running user's sharing rules** (OWD, hierarchy, sharing rules) for records the class queries/modifies — the user sees only what they're allowed to.

### 📏 Limits

*Governor & platform limits*

- Enforces record-level sharing but **not** CRUD or FLS.
- Inner classes do not inherit the outer class's sharing keyword.
- Does not apply to the running user in system contexts such as batch `start()`.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## without sharing

*Run in system context, ignoring the user's sharing.*

### 🌱 Simple

*Beginner - plain language*

**`without sharing`** makes an Apex class run in **system context** — ignoring the running user's sharing rules so it can access all records. Use deliberately for trusted operations.

### 📏 Limits

*Governor & platform limits*

- Ignores record-level sharing entirely - a deliberate privilege escalation.
- Still does not bypass CRUD or FLS unless you also skip those checks.
- A standard security review finding when used without justification.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## inherited sharing

*Adopt the calling context's sharing.*

### 🌱 Simple

*Beginner - plain language*

**`inherited sharing`** makes an Apex class run with the **sharing of whoever calls it** — secure by default when entered directly, flexible when called by others.

### 📏 Limits

*Governor & platform limits*

- Runs with the caller's sharing context; defaults to `with sharing` when entered directly.
- Available from API version 45.0.
- Does not affect CRUD or FLS enforcement.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## USER_MODE

*Run SOQL/DML enforcing sharing + CRUD/FLS automatically.*

### 🌱 Simple

*Beginner - plain language*

**`USER_MODE`** (in SOQL/DML, e.g., `[SELECT ... WITH USER_MODE]` or `Database.insert(rec, AccessLevel.USER_MODE)`) runs the operation enforcing the user's **sharing AND object/field permissions** automatically.

### 📏 Limits

*Governor & platform limits*

- Enforces CRUD, FLS and sharing on SOQL and DML from API version 55.0.
- Cannot be combined with dynamic SOQL built by string concatenation without `queryWithBinds`.
- An inaccessible field causes an exception rather than a silent null.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## SYSTEM_MODE

*Run SOQL/DML ignoring sharing + CRUD/FLS.*

### 🌱 Simple

*Beginner - plain language*

**`SYSTEM_MODE`** (`AccessLevel.SYSTEM_MODE` / default Apex behavior) runs operations **ignoring** the user's sharing, CRUD, and FLS — full access. Use only for trusted system logic.

### 📏 Limits

*Governor & platform limits*

- The default for Apex - CRUD, FLS and sharing are all ignored.
- Explicit enforcement is entirely the developer's responsibility.
- A common source of data exposure found only in security review.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## stripInaccessible

*Remove fields/relationships the user can't access.*

### 🌱 Simple

*Beginner - plain language*

**`Security.stripInaccessible`** takes records and **strips out fields the running user can't read or update** (per FLS/CRUD), returning a sanitized copy — preventing accidental exposure or unauthorized writes.

### 📏 Limits

*Governor & platform limits*

- Removes inaccessible fields but does not throw - callers may not notice.
- Does not enforce record-level sharing, only CRUD and FLS.
- Returns a new collection; the original is unchanged.
- Adds CPU cost proportional to record and field count.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## WITH SECURITY_ENFORCED

*Enforce FLS/CRUD in a SOQL query clause.*

### 🌱 Simple

*Beginner - plain language*

**`WITH SECURITY_ENFORCED`** is a SOQL clause that makes the query **throw an exception if the user lacks FLS/CRUD** on any selected field or object — enforcing field/object permissions inline.

### 📏 Limits

*Governor & platform limits*

- Throws on the first inaccessible field rather than stripping it.
- Does not cover fields referenced only in the WHERE clause in all versions.
- Largely superseded by `WITH USER_MODE`.

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
