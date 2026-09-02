[Home](../index.md) / [04 · Automation](index.md) / **Validation Rules**

# Validation Rules

4 topics · Series 4: Automation

**Topics on this page**

- [ISPICKVAL](#ispickval)
- [ISBLANK](#isblank)
- [REGEX](#regex)
- [VLOOKUP](#vlookup)

## ISPICKVAL

*Compare a picklist field to a value safely inside validation/formula logic.*

### 🌱 Simple

*Beginner - plain language*

`ISPICKVAL(field, "value")` checks whether a **picklist** equals a specific value. You use it in validation rules and formulas because you can't compare picklists with plain `=` reliably.

### 📏 Limits

*Governor & platform limits*

- Single-select only (INCLUDES for multi); operates on API value; needs bypass design for admins/integrations.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## ISBLANK

*Check whether a field has no value — the standard 'is empty' test in formulas/validation.*

### 🌱 Simple

*Beginner - plain language*

`ISBLANK(field)` returns true when a field is **empty**. It's how you require fields conditionally — e.g., "if Type is 'Other', Description can't be blank".

### 📏 Limits

*Governor & platform limits*

- `ISBLANK` works on text and numeric fields; `ISNULL` does not work correctly on text.
- Empty string and null are treated the same for text fields.
- Number fields treat zero as not blank, which is a common logic bug.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## REGEX

*Pattern-match text with REGEX() for format validation — emails, phones, IDs, postcodes.*

### 🌱 Simple

*Beginner - plain language*

`REGEX(text, "pattern")` checks whether text matches a **pattern** — like a valid phone number, postal code, or ID format. It returns true on a full match, so you can block badly-formatted data.

### 📏 Limits

*Governor & platform limits*

- Whole-string match; Java regex; runs on all save paths; performance risk on bad patterns; format-only (not uniqueness).

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## VLOOKUP

*Validation-only function that looks up a value in a custom object to validate input.*

### 🌱 Simple

*Beginner - plain language*

`VLOOKUP` (in validation rules) checks a field against values stored in a **custom object** — like verifying an entered ZIP exists in your official ZIP-code reference table. It's Salesforce's spreadsheet-style lookup for validation.

### 📏 Limits

*Governor & platform limits*

- Validation rules only; custom object + Name-field key; first match; save-path cost; small reference sets.

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
