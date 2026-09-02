[Home](../index.md) / [10 · LWC Advanced](index.md) / **Browser APIs**

# Browser APIs

5 topics · Series 10: LWC Advanced

**Topics on this page**

- [Local Storage](#local-storage)
- [Session Storage](#session-storage)
- [Clipboard API](#clipboard-api)
- [Geolocation API](#geolocation-api)
- [Web Workers](#web-workers)

## Local Storage

*Persistent key-value browser storage and its LWC caveats.*

### 🌱 Simple

*Beginner - plain language*

**localStorage** is a browser key-value store that persists across sessions (until cleared). In LWC you can use it for non-sensitive client-side caching, but with caveats around Locker/LWS and origins.

### 📏 Limits

*Governor & platform limits*

- Roughly 5-10 MB per origin, shared across all components.
- Distorted under Lightning Web Security - behaviour differs from a plain page.
- Never store PII or session data; it is readable by any script on the origin.
- Synchronous API - large reads block the main thread.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Session Storage

*Tab-scoped storage cleared when the tab closes.*

### 🌱 Simple

*Beginner - plain language*

**sessionStorage** is like localStorage but scoped to a single tab/session — it's cleared when the tab closes. Useful for transient, non-sensitive state within one browsing session.

### 📏 Limits

*Governor & platform limits*

- Same ~5-10 MB origin quota, cleared when the tab closes.
- Not shared between tabs, which surprises console users.
- Subject to the same LWS distortions as local storage.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Clipboard API

*Programmatic copy/paste with permissions and fallbacks.*

### 🌱 Simple

*Beginner - plain language*

The **Clipboard API** (`navigator.clipboard.writeText/readText`) lets you copy/paste text programmatically — e.g., a "Copy link" button. It's async and may require user gesture/permission.

### 📏 Limits

*Governor & platform limits*

- Requires a secure context and, for reads, explicit user permission.
- Must be triggered by a user gesture - it cannot be called programmatically on load.
- Browser support and permission prompts vary; always provide a fallback.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Geolocation API

*Getting device location with permission and accuracy options.*

### 🌱 Simple

*Beginner - plain language*

The **Geolocation API** (`navigator.geolocation.getCurrentPosition`) retrieves the device's coordinates — useful for check-ins or finding nearby records. It requires user permission and a secure context.

### 📏 Limits

*Governor & platform limits*

- Requires HTTPS and explicit user permission.
- Users can deny permanently - handle the denial path.
- Accuracy and timeout vary by device; always set a timeout option.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Web Workers

*Offloading heavy computation off the main thread.*

### 🌱 Simple

*Beginner - plain language*

**Web Workers** run JavaScript on a background thread, keeping the UI responsive during heavy computation (parsing, calculations). They communicate with the main thread via messages.

### 📏 Limits

*Governor & platform limits*

- Worker scripts must be served from a Static Resource - no inline blob workers under LWS.
- Workers cannot access the DOM or Lightning APIs.
- Message passing is structured-clone only; functions cannot be transferred.

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
