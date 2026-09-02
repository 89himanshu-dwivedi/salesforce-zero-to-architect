[Home](../index.md) / [08 · Apex Advanced](index.md) / **Platform Cache**

# Platform Cache

4 topics · Series 8: Apex Advanced

**Topics on this page**

- [Org Cache](#org-cache)
- [Session Cache](#session-cache)
- [Cache Strategy](#cache-strategy)
- [Cache Invalidation](#cache-invalidation)

## Org Cache

*Org-wide shared cache for data common to all users.*

### 🌱 Simple

*Beginner - plain language*

**Org Cache** stores data shared across **all users** in the org. Use it for reference data that's expensive to compute and the same for everyone — e.g., pricing tables, config maps. Access via `Cache.Org.put/get`.

### 📏 Limits

*Governor & platform limits*

- Default TTL 24 hours, maximum 48 hours.
- Maximum 100 KB per cached item; total capacity is purchased.
- Entries can be evicted before TTL - code must work on a cache miss.
- Org cache is shared across users, so never cache row-level restricted data.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Session Cache

*Per-user cache tied to the user's session.*

### 🌱 Simple

*Beginner - plain language*

**Session Cache** stores data **per user session** — visible only to that user, cleared when the session ends. Use it for user-specific computed data. Access via `Cache.Session.put/get`.

### 📏 Limits

*Governor & platform limits*

- Maximum TTL 8 hours and tied to the user's session.
- Maximum 100 KB per item; capacity is shared with org cache allocation.
- Cleared on logout and not available in async Apex without a session.
- Not shared between users, which makes it safe for user-specific data.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Cache Strategy

*Choosing what, where, and how long to cache.*

### 🌱 Simple

*Beginner - plain language*

**Cache strategy** decides **what** to cache (expensive, reused, stable data), **where** (Org vs Session), and **how long** (TTL). The goal: maximize hit rate and value while keeping data acceptably fresh.

### 📏 Limits

*Governor & platform limits*

- Cache is best-effort: always implement the miss path.
- Version cache keys so a deploy that changes the cached shape cannot serve stale structures.
- Do not cache data subject to sharing in org cache.
- Cache capacity is purchased - exceeding it causes silent eviction, not an error.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Cache Invalidation

*Keeping cached data correct when the source changes.*

### 🌱 Simple

*Beginner - plain language*

**Cache invalidation** removes or refreshes cached entries when the underlying data changes, preventing **stale reads**. Strategies include TTL expiry and event/trigger-based eviction on writes.

### 📏 Limits

*Governor & platform limits*

- There is no automatic invalidation on deploy or on data change.
- TTL is the only built-in expiry; you must handle correctness yourself.
- No API flushes the entire cache for all users.
- Key versioning is the practical invalidation mechanism.

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
