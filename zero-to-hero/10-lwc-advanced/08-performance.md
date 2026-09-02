[Home](../index.md) / [10 · LWC Advanced](index.md) / **Performance**

# Performance

8 topics · Series 10: LWC Advanced

**Topics on this page**

- [Debouncing](#debouncing)
- [Throttling](#throttling)
- [Lazy Loading](#lazy-loading)
- [Pagination](#pagination)
- [Infinite Scroll](#infinite-scroll)
- [Client Side Cache](#client-side-cache)
- [RefreshApex](#refreshapex)
- [Memoization](#memoization)

## Debouncing

*Delaying action until input pauses to cut redundant work.*

### 🌱 Simple

*Beginner - plain language*

**Debouncing** delays an action until the user stops triggering it for a set time — e.g., wait 300ms after the last keystroke before searching, instead of querying on every keystroke.

### 📏 Limits

*Governor & platform limits*

- Timers must be cleared in `disconnectedCallback` or they fire against a destroyed component.
- Too long a delay feels unresponsive; 250-350 ms is the usual range.
- Debouncing does not prevent out-of-order responses - track a request sequence.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Throttling

*Capping how often a handler runs for continuous events.*

### 🌱 Simple

*Beginner - plain language*

**Throttling** limits how often an action runs to at most once per interval — e.g., handle a scroll/resize event no more than every 200ms, regardless of how many fire.

### 📏 Limits

*Governor & platform limits*

- Guarantees a maximum rate but can drop the final event - pair with a trailing call.
- Timers must be cleaned up on disconnect.
- Not a substitute for server-side rate limiting.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Lazy Loading

*Deferring loading of components/data until needed.*

### 🌱 Simple

*Beginner - plain language*

**Lazy loading** defers loading components, modules, or data until they're actually needed — shrinking initial load time and only paying for what's used.

### 📏 Limits

*Governor & platform limits*

- Dynamic imports add a network round trip on first use.
- Deferred components still run their full lifecycle when revealed.
- Static Resources are capped at 5 MB each, 250 MB per org.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Pagination

*Splitting large datasets into navigable pages.*

### 🌱 Simple

*Beginner - plain language*

**Pagination** breaks a large result set into pages (e.g., 25 rows each) with next/previous controls — reducing payload and render cost versus loading everything at once.

### 📏 Limits

*Governor & platform limits*

- SOQL `OFFSET` is capped at 2,000 - use keyset pagination beyond that.
- Total record counts require a separate `COUNT()`, expensive on large objects.
- Concurrent inserts shift rows between pages.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Infinite Scroll

*Auto-loading more content as the user scrolls.*

### 🌱 Simple

*Beginner - plain language*

**Infinite scroll** automatically loads the next chunk of content as the user nears the bottom — a continuous browse experience without explicit page controls.

### 📏 Limits

*Governor & platform limits*

- Accumulated DOM nodes are the bottleneck, not Apex.
- Requires a fixed-height container to detect the scroll boundary.
- Offset-based loading breaks at 2,000 rows.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Client Side Cache

*Reusing fetched data in memory to avoid re-querying.*

### 🌱 Simple

*Beginner - plain language*

**Client-side caching** stores fetched data in the component (or a shared module) so repeated needs reuse it instead of re-calling Apex — faster UX and fewer server round-trips.

### 📏 Limits

*Governor & platform limits*

- Only `cacheable=true` Apex results are cached by LDS.
- Cached data is stale until `refreshApex` or a record-update notification.
- No API flushes the Lightning client cache for all users.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## RefreshApex

*Refreshing wired Apex data after server-side changes.*

### 🌱 Simple

*Beginner - plain language*

**refreshApex** re-fetches a wired Apex result so the UI reflects server-side changes (after a DML/update) — without reloading the page. You pass it the wired provisioned value.

### 📏 Limits

*Governor & platform limits*

- Only works on the full provisioned wire result object, not on `.data`.
- Has no effect on imperative call results.
- Triggers a fresh server round trip and its Apex governor limits.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Memoization

*Caching pure function results by their inputs.*

### 🌱 Simple

*Beginner - plain language*

**Memoization** caches a function's output for given inputs so repeated calls with the same arguments return instantly instead of recomputing — for pure, expensive functions.

### 📏 Limits

*Governor & platform limits*

- Cached values must be invalidated manually - there is no automatic expiry.
- Memoised objects consume browser memory for the component's lifetime.
- Stale memoised data after a save is a common bug - clear on refresh.

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
