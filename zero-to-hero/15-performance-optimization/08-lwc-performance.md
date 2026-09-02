[Home](../index.md) / [15 · Performance Optimization](index.md) / **LWC Performance**

# LWC Performance

7 topics · Series 15: Performance Optimization

**Topics on this page**

- [Debouncing](#debouncing)
- [Throttling](#throttling)
- [Infinite Scroll](#infinite-scroll)
- [Lazy Rendering](#lazy-rendering)
- [Virtualization](#virtualization)
- [Client Side Cache](#client-side-cache)
- [RefreshApex](#refreshapex)

## Debouncing

*Wait for input to pause before acting (e.g., search).*

### 🌱 Simple

*Beginner - plain language*

**Debouncing** delays an action until the user **stops triggering it** for a set time — e.g., wait 300ms after the last keystroke before firing a search, instead of querying on every keypress.

### 📏 Limits

*Governor & platform limits*

- Timers must be cleared in `disconnectedCallback` or they fire against a destroyed component.
- Does not prevent out-of-order responses - track a request sequence.
- Excessive delay makes the UI feel unresponsive.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Throttling

*Limit an action to at most once per interval.*

### 🌱 Simple

*Beginner - plain language*

**Throttling** ensures an action runs **at most once per fixed interval** during continuous activity — e.g., handle a scroll or resize event every 200ms rather than on every single event.

### 📏 Limits

*Governor & platform limits*

- Can drop the trailing event unless explicitly handled.
- Client-side throttling does not protect server limits from other clients.
- Timers require cleanup on component teardown.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Infinite Scroll

*Load more records as the user scrolls to the bottom.*

### 🌱 Simple

*Beginner - plain language*

**Infinite scroll** loads data in **pages**, fetching the next chunk only when the user scrolls near the bottom — instead of loading thousands of rows upfront. It keeps initial load fast and memory low.

### 📏 Limits

*Governor & platform limits*

- SOQL `OFFSET` caps at 2,000 - keyset pagination is required beyond that.
- Accumulated DOM nodes become the bottleneck, not Apex.
- Requires a fixed-height container to detect the scroll boundary.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Lazy Rendering

*Render components/sections only when needed.*

### 🌱 Simple

*Beginner - plain language*

**Lazy rendering** defers building parts of the UI until they're actually shown — e.g., render a tab's content only when the tab is opened, or a section only when expanded — speeding initial load.

### 📏 Limits

*Governor & platform limits*

- Deferred components still run their full lifecycle when revealed.
- Conditional rendering destroys state - scroll position and inputs are lost.
- Server calls are deferred, not eliminated.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Virtualization

*Render only visible rows of a large list.*

### 🌱 Simple

*Beginner - plain language*

**Virtualization** (windowing) renders only the rows **currently visible** in the viewport plus a small buffer — not all thousands of rows — keeping the DOM small and scrolling smooth.

### 📏 Limits

*Governor & platform limits*

- Not provided natively by `lightning-datatable` beyond infinite loading.
- Custom virtualisation breaks accessibility and keyboard navigation if done carelessly.
- Row height must be predictable for windowing to work.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Client Side Cache

*Lightning framework caches cacheable Apex/LDS data.*

### 🌱 Simple

*Beginner - plain language*

**Client-side cache** stores server data in the browser so repeated requests don't hit the server. In LWC, `@AuraEnabled(cacheable=true)` with `@wire`, and **Lightning Data Service (LDS)**, cache record data automatically.

### 📏 Limits

*Governor & platform limits*

- Only `cacheable=true` Apex results are cached by LDS.
- Cached data is stale until `refreshApex` or a record-update notification.
- No API flushes the Lightning client cache for all users.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## RefreshApex

*Force a wired cacheable result to re-fetch fresh data.*

### 🌱 Simple

*Beginner - plain language*

`refreshApex` re-fetches a **wired Apex result**, bypassing the client cache to get fresh data — used after a DML/update so the UI reflects the latest server state.

### 📏 Limits

*Governor & platform limits*

- Works only on the full provisioned wire result, not on `.data`.
- Has no effect on imperative call results.
- Triggers a fresh server round trip with full Apex limits.

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
