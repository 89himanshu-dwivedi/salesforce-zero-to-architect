[Home](../index.md) / [06 · SOQL & SOSL](index.md) / **SOSL Fundamentals**

# SOSL Fundamentals

9 topics · Series 6: SOQL & SOSL

**Topics on this page**

- [FIND RETURNING LIMIT](#find-returning-limit)
- [ALL FIELDS](#all-fields)
- [NAME FIELDS](#name-fields)
- [EMAIL FIELDS](#email-fields)
- [PHONE FIELDS](#phone-fields)
- [Wildcards](#wildcards)
- [Fuzzy Matching](#fuzzy-matching)
- [Exact Search](#exact-search)
- [Multi Object Search](#multi-object-search)

## FIND RETURNING LIMIT

*The core SOSL syntax — search text across objects and shape what comes back.*

### 🌱 Simple

*Beginner - plain language*

**SOSL** (Salesforce Object Search Language) does full-text search. The shape is `FIND 'term' RETURNING Object(fields) LIMIT n` — search for text, choose which objects/fields to return, and cap results.

### 📏 Limits

*Governor & platform limits*

- 2,000 records/SOSL; 20 SOSL/transaction.
- Index lag after writes.
- Returns nested result lists.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## ALL FIELDS

*A SOSL search group that searches across every searchable field.*

### 🌱 Simple

*Beginner - plain language*

`IN ALL FIELDS` tells SOSL to search the term across **all searchable fields** of the targeted objects — the broadest search scope.

### 📏 Limits

*Governor & platform limits*

- Only searchable/indexed fields.
- Broadest scope = more index work.
- Encrypted/unsupported types excluded.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## NAME FIELDS

*Restricting a SOSL search to name fields for precise entity lookups.*

### 🌱 Simple

*Beginner - plain language*

`IN NAME FIELDS` searches only the **name fields** (Account Name, Contact Name, etc.) — perfect for "find a record by its name" lookups.

### 📏 Limits

*Governor & platform limits*

- Searches only Name and name-like fields, so matches in other fields are missed.
- SOSL returns a maximum of 2,000 rows per query.
- Depends on the search index, which updates asynchronously.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## EMAIL FIELDS

*Restricting a SOSL search to email fields for fast email lookups.*

### 🌱 Simple

*Beginner - plain language*

`IN EMAIL FIELDS` searches only **email fields** across objects — the fastest, most precise way to find a record by email address.

### 📏 Limits

*Governor & platform limits*

- Searches only fields of type Email.
- 2,000-row SOSL cap applies.
- Partial-token matching behaviour differs from a SOQL `LIKE`.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## PHONE FIELDS

*Restricting a SOSL search to phone fields for caller/number lookups.*

### 🌱 Simple

*Beginner - plain language*

`IN PHONE FIELDS` searches only **phone fields** — the targeted way to find a record by phone number (e.g., for screen-pop in a call center).

### 📏 Limits

*Governor & platform limits*

- Searches only Phone-type fields.
- Formatting differences (spaces, dashes, country codes) cause misses - normalise on write.
- 2,000-row SOSL cap applies.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Wildcards

*Using * and ? in SOSL to match partial and single-character patterns.*

### 🌱 Simple

*Beginner - plain language*

SOSL **wildcards**: `*` matches any number of characters and `?` matches a single character. `FIND 'Acme*'` finds Acme, Acme Inc, Acmecorp, etc.

### 📏 Limits

*Governor & platform limits*

- `*` many chars, `?` one char.
- Leading wildcards restricted/less efficient.
- Escape special characters.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Fuzzy Matching

*Tolerant matching that finds results despite spelling/format variations.*

### 🌱 Simple

*Beginner - plain language*

**Fuzzy matching** finds records even when the search term isn't an exact match — handling typos, partial words, and variations — which SOSL supports far better than exact SOQL filters.

### 📏 Limits

*Governor & platform limits*

- SOSL = token/wildcard tolerance, not full fuzzy.
- Advanced fuzziness via matching rules.
- No edit-distance tuning in SOSL.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Exact Search

*Forcing precise matching when tolerance would return unwanted results.*

### 🌱 Simple

*Beginner - plain language*

**Exact search** means matching the term precisely (e.g., quoting a phrase) so you don't get the broad, tolerant results that wildcards/tokenization produce — useful when precision matters.

### 📏 Limits

*Governor & platform limits*

- SOSL is relevance-based, not strictly exact.
- Exact equality → SOQL.
- Phrase quoting needed for sequences.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Multi Object Search

*Searching several objects in a single SOSL query and handling the results.*

### 🌱 Simple

*Beginner - plain language*

**Multi-object search** is SOSL's ability to search **many objects at once** — list several in `RETURNING` and get parallel result lists, like the platform's global search.

### 📏 Limits

*Governor & platform limits*

- 2,000 records/SOSL; 20/transaction.
- Results in RETURNING order.
- Per-object WHERE/ORDER/LIMIT.

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
