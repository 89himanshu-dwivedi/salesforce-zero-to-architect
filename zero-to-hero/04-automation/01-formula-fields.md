[Home](../index.md) / [04 · Automation](index.md) / **Formula Fields**

# Formula Fields

5 topics · Series 4: Automation

**Topics on this page**

- [Text Functions](#text-functions)
- [Date Functions](#date-functions)
- [Logical Functions](#logical-functions)
- [Math Functions](#math-functions)
- [Advanced Formula](#advanced-formula)

## Text Functions

*Manipulate strings in formulas — concatenate, extract, search, transform text.*

### 🌱 Simple

*Beginner - plain language*

**Text functions** let formulas work with words and characters — join them, cut pieces out, find text, or change case. Example: build a full name from first + last.

### 📏 Limits

*Governor & platform limits*

- Formula compile size 5,000 bytes; formula text 3,900 characters.
- Text functions on Long Text and Rich Text fields are unsupported or truncated.
- Results are not indexable, so filtering on them is non-selective.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Date Functions

*Compute with dates/datetimes — differences, components, today/now, conversions.*

### 🌱 Simple

*Beginner - plain language*

**Date functions** let formulas work with dates — find how many days between two dates, get today, or pull the year/month out of a date. Example: "days since created".

### 📏 Limits

*Governor & platform limits*

- DateTime math in decimal days; TZ-sensitive; no built-in business-day/holiday function; null-propagation.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Logical Functions

*Branch and combine conditions — IF, AND, OR, NOT, CASE, ISBLANK.*

### 🌱 Simple

*Beginner - plain language*

**Logical functions** make formulas decide things: `IF` (this or that), `AND/OR/NOT` (combine conditions), and `CASE` (many branches). They're the "if-then" brain of formulas.

### 📏 Limits

*Governor & platform limits*

- Compile-size limit; three-valued logic with nulls; ISCHANGED/PRIORVALUE only in validation/automation contexts.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Math Functions

*Numeric computation in formulas — rounding, absolute, modulo, min/max, powers.*

### 🌱 Simple

*Beginner - plain language*

**Math functions** do calculations in formulas — round numbers, get absolute values, find remainders, pick the largest/smallest. Example: round a commission to 2 decimals.

### 📏 Limits

*Governor & platform limits*

- Number precision is capped at 18 digits total including decimals.
- Division by zero throws a formula error that blocks the save.
- Rounding behaviour differs between Currency and Number types.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Advanced Formula

*Cross-object references, nested logic, and the limits/patterns of complex formulas.*

### 🌱 Simple

*Beginner - plain language*

**Advanced formulas** combine many functions, reference **related records** (parent fields), and handle complex business rules — all without code. Example: pull the parent account's tier onto the contact and act on it.

### 📏 Limits

*Governor & platform limits*

- ~5,000-byte compile; ~10 spanning relationships; cross-object up lookups/MD only; recalc-on-read cost; not stored/indexed.

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
