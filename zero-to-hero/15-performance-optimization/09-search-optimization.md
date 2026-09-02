[Home](../index.md) / [15 · Performance Optimization](index.md) / **Search Optimization**

# Search Optimization

4 topics · Series 15: Performance Optimization

**Topics on this page**

- [SOSL Optimization](#sosl-optimization)
- [Global Search Optimization](#global-search-optimization)
- [Search Layout](#search-layout)
- [Search Ranking](#search-ranking)

## SOSL Optimization

*Efficient full-text search across objects.*

### 🌱 Simple

*Beginner - plain language*

**SOSL** (Salesforce Object Search Language) searches **text across multiple objects** using the search index. Optimizing it means scoping the search (objects, fields, filters, LIMIT) so it returns relevant results fast.

### 📏 Limits

*Governor & platform limits*

- 20 SOSL queries per transaction; 2,000 rows per query.
- Search index lag means very recent records may be missing.
- Leading wildcards are expensive and should be avoided at volume.
- Results count toward the 50,000 query-row limit.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Global Search Optimization

*Tuning the org-wide search experience and relevance.*

### 🌱 Simple

*Beginner - plain language*

**Global search** is the Salesforce header search box that queries across many objects. Optimizing it means configuring **searchable objects/fields, search layouts, and synonyms** so users find the right records quickly.

### 📏 Limits

*Governor & platform limits*

- Search relies on the search index, which updates asynchronously.
- Only searchable field types are indexed - long text and encrypted fields are limited.
- Search results are capped and subject to sharing filtering.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Search Layout

*Configuring fields shown in search results.*

### 🌱 Simple

*Beginner - plain language*

**Search layouts** control which **fields/columns appear in search results** (and related lists, lookup dialogs) for each object — helping users distinguish and pick the right record quickly.

### 📏 Limits

*Governor & platform limits*

- Limited number of columns can be displayed in search results.
- Adding columns increases payload and slows result rendering.
- Layouts are configured per object and per profile.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Search Ranking

*How relevance ordering surfaces the best results first.*

### 🌱 Simple

*Beginner - plain language*

**Search ranking** determines the **order** of search results by relevance — so the most likely record appears first. Salesforce (especially with **Einstein Search**) ranks by match quality and user behavior.

### 📏 Limits

*Governor & platform limits*

- Ranking is controlled by the platform and cannot be directly tuned.
- Personalised ranking based on user activity is not configurable.
- Recently viewed and ownership influence results in ways you cannot override.

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
