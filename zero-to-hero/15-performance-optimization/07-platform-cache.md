[Home](../index.md) / [15 · Performance Optimization](index.md) / **Platform Cache**

# Platform Cache

3 topics · Series 15: Performance Optimization

**Topics on this page**

- [Session Cache](#session-cache)
- [Org Cache](#org-cache)
- [Cache Invalidation](#cache-invalidation)

## Session Cache

*Per-user cache tied to a session, up to 8 hours.*

### 🌱 Simple

*Beginner - plain language*

**Session Cache** is a Platform Cache partition scoped to an **individual user's session** — storing per-user data (preferences, wizard state, user-specific lookups) that persists across requests for that user, up to **8 hours**.

### 📏 Limits

*Governor & platform limits*

- Maximum TTL 8 hours and tied to the user session.
- Not available in async Apex without a session context.
- Cleared on logout; not shared across users or tabs in all cases.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Org Cache

*Org-wide shared cache for all users, up to 48 hours.*

### 🌱 Simple

*Beginner - plain language*

**Org Cache** is a Platform Cache partition **shared across all users** in the org — ideal for common, rarely-changing data like configuration, reference tables, and metadata, with a TTL up to **48 hours**.

### 📏 Limits

*Governor & platform limits*

- Default TTL 24 hours, maximum 48 hours.
- Maximum 100 KB per item; shared org-wide.
- Entries can be evicted early under pressure - always handle a miss.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Cache Invalidation

*Keeping cached data fresh: TTL, versioning, explicit clears.*

### 🌱 Simple

*Beginner - plain language*

**Cache invalidation** is the strategy for ensuring cached data doesn't go **stale** — using time-to-live (TTL) expiry, **versioned keys**, or **explicit removal** when the underlying data changes.

### 📏 Limits

*Governor & platform limits*

- There is no automatic invalidation on deploy or on data change.
- TTL is the only built-in expiry mechanism.
- No API flushes the entire cache for all users.
- Key versioning is the practical solution.

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
