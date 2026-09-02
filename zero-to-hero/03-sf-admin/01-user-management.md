[Home](../index.md) / [03 · SF Admin](index.md) / **User Management**

# User Management

4 topics · Series 3: SF Admin

**Topics on this page**

- [User Creation](#user-creation)
- [Freeze User](#freeze-user)
- [Deactivate User](#deactivate-user)
- [Login As User](#login-as-user)

## User Creation

*Provisioning a user: license, profile, role and access — the gateway to everything.*

### 🌱 Simple

*Beginner - plain language*

**Creating a user** gives a person a login to Salesforce. You set their name/email, a **username** (must be globally unique, email-format), a **license**, a **profile**, and optionally a **role**. Salesforce then emails them to set a password.

### 📏 Limits

*Governor & platform limits*

- Licence counts are contractual and enforced - you cannot exceed them.
- Users cannot be deleted, only deactivated.
- User is a setup object, so Mixed DML rules apply in Apex.
- Usernames must be globally unique across all Salesforce orgs.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Freeze User

*Instantly block a user's login without freeing their license — fast, reversible.*

### 🌱 Simple

*Beginner - plain language*

**Freezing** a user immediately stops them logging in, but does *not* deactivate them or free their license. It's a quick, reversible "lock the door now" action — perfect while you prepare a proper deactivation.

### 📏 Limits

*Governor & platform limits*

- License still consumed; doesn't reassign records; only blocks new logins (revoke sessions separately).

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Deactivate User

*Permanently disable a user and reclaim their license — handle their data first.*

### 🌱 Simple

*Beginner - plain language*

**Deactivating** a user removes their access permanently and **frees their license** for reuse. Salesforce never lets you *delete* users (to preserve data integrity) — you deactivate them instead.

### 📏 Limits

*Governor & platform limits*

- Users can't be deleted; deactivation blocked by active dependencies; ownership must be handled separately.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Login As User

*Admins impersonate a user to troubleshoot what they actually see and can do.*

### 🌱 Simple

*Beginner - plain language*

**Login As** lets an admin log in *as* another user to see exactly what they see — perfect for troubleshooting "why can't I access this?" without guessing.

### 📏 Limits

*Governor & platform limits*

- Needs admin permission/org setting or user-granted access; actions attributed to target user; governed/logged.

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
