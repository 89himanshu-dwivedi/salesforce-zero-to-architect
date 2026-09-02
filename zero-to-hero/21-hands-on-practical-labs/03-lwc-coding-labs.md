[Home](../index.md) / [21 · Hands-On Practical Labs](index.md) / **LWC Coding Labs**

# LWC Coding Labs

7 topics · Series 21: Hands-On Practical Labs

**Topics on this page**

- [First LWC Component](#first-lwc-component)
- [Wire Apex to LWC](#wire-apex-to-lwc)
- [Imperative Apex Call](#imperative-apex-call)
- [LDS No Apex](#lds-no-apex)
- [Parent Child Events](#parent-child-events)
- [Lightning Message Service](#lightning-message-service)
- [Navigation Service](#navigation-service)

## First LWC Component

*The 3 files of every LWC and how data flows HTML↔JS.*

### 🌱 Simple

*Beginner - plain language*

A **Lightning Web Component (LWC)** is a reusable UI block built with standard HTML + JavaScript. Every component is a folder with **3 core files**: `.html` (template), `.js` (logic), and `.js-meta.xml` (where it can be used). Data flows by binding JS properties into the HTML.

### 📏 Limits

*Governor & platform limits*

- Template expressions cannot contain arbitrary JS - getters only.
- Shadow DOM prevents `document.querySelector` reaching inside a child; use `this.template.querySelector`.
- No inline scripts, no `eval` - blocked by CSP.
- Third-party JS must load from a static resource under Lightning Web Security.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Wire Apex to LWC

*Reactively pull Salesforce data into LWC with @wire.*

### 🌱 Simple

*Beginner - plain language*

The **@wire** service reactively calls an Apex method (or LDS adapter) and pushes the result into your component. When inputs change, it re-fetches automatically. Use `@wire` for **reading** data that should stay fresh.

### 📏 Limits

*Governor & platform limits*

- `cacheable=true` methods cannot perform DML.
- Wire cannot be called on demand and has no `await`; it is push-based.
- Apex heap/SOQL limits still apply server-side per wire invocation.
- Wire results are cached client-side - not suitable for data that must be real-time.
- The user needs Apex class access *and* FLS on every field returned, or the wire errors.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Imperative Apex Call

*Call Apex on demand (button click) and handle the promise.*

### 🌱 Simple

*Beginner - plain language*

**Imperative** Apex calls run **when you decide** — e.g., on a button click — instead of reactively like `@wire`. Use it for **actions**: saving data, running a process, or fetching only after the user clicks. It returns a **Promise**.

### 📏 Limits

*Governor & platform limits*

- No client-side caching unless the method is `cacheable=true`.
- Each call is a server round trip - watch the daily API/request budget in high-traffic Experience Cloud sites.
- Apex governor limits apply per call, not per component.
- Response payload is subject to heap limits on the Apex side; return DTOs, not 50,000 records.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## LDS No Apex

*Read/create/update records with zero Apex via Lightning Data Service.*

### 🌱 Simple

*Beginner - plain language*

**Lightning Data Service (LDS)** lets you read and write single records **without any Apex, SOQL, or DML**. Components like `lightning-record-form` and wire adapters (`getRecord`, `updateRecord`) handle the database, sharing, FLS, and caching for you.

### 📏 Limits

*Governor & platform limits*

- Single-record operations only - no bulk create/update.
- No support for `Task`/`Event` polymorphic edge cases and a few non-LDS-supported objects.
- Record fields must be imported with the schema import syntax or referenced by API name string.
- `getRelatedListRecords` paginates; it is not a substitute for SOQL over large child sets.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Parent Child Events

*Pass data down with props, send data up with CustomEvent.*

### 🌱 Simple

*Beginner - plain language*

LWC communication follows one rule: **data down via properties (`@api`), events up via `CustomEvent`**. A parent passes values into a child's public properties; the child notifies the parent by dispatching an event the parent listens to.

### 📏 Limits

*Governor & platform limits*

- Event names: lowercase, no hyphens, no camelCase.
- Non-composed events cannot cross a shadow boundary.
- No sibling-to-sibling communication without a common parent or LMS.
- `@api` properties are read-only inside the child - mutating them throws in strict scenarios.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Lightning Message Service

*Pub/sub messaging across components anywhere on the page (even Aura/VF).*

### 🌱 Simple

*Beginner - plain language*

**Lightning Message Service (LMS)** lets components that are **not** parent/child — anywhere on the page, even across LWC, Aura, and Visualforce — talk to each other via **publish/subscribe** on a shared **Message Channel**.

### 📏 Limits

*Governor & platform limits*

- No delivery guarantee, no ordering guarantee, no replay - messages are lost if nobody is listening.
- Message channel is metadata; changes require a deploy.
- Does not work in Salesforce Mobile for all surfaces - verify per target.
- Payload should stay small; it is not a data transport layer.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Navigation Service

*Navigate to records, pages, and URLs the supported way.*

### 🌱 Simple

*Beginner - plain language*

The **NavigationMixin** is how an LWC sends users to another record, list view, tab, or external URL — without hard-coding URLs (which break across environments). You describe a **page reference** and Salesforce builds the correct link.

### 📏 Limits

*Governor & platform limits*

- Not every page type is supported on every surface (mobile vs desktop vs Experience Cloud).
- `standard__webPage` to an external URL still respects the org's URL redirect warnings.
- Custom URL state parameters must be prefixed with `c__`.
- Navigation is asynchronous - do not assume the target has rendered after the call returns.

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
