[Home](../index.md) / [02 · Data Modeling](index.md) / **Field Types**

# Field Types

19 topics · Series 2: Data Modeling

**Topics on this page**

- [Text](#text)
- [Long Text](#long-text)
- [Rich Text](#rich-text)
- [Number](#number)
- [Percent](#percent)
- [Currency](#currency)
- [Date](#date)
- [Date Time](#date-time)
- [Time](#time)
- [Email](#email)
- [Phone](#phone)
- [URL](#url)
- [Picklist](#picklist)
- [Multi Picklist](#multi-picklist)
- [Geolocation](#geolocation)
- [Encrypted](#encrypted)
- [Formula](#formula)
- [Auto Number](#auto-number)
- [Rollup Summary](#rollup-summary)

## Text

*Single-line text up to 255 chars — names, codes, short labels. Indexable, often External ID.*

### 🌱 Simple

*Beginner - plain language*

A **Text** field holds a single line of characters — up to **255** — for things like names, reference codes, or short labels.

### 📏 Limits

*Governor & platform limits*

- Maximum 255 characters.
- Indexed only if marked External Id or Unique.
- Case-insensitive for uniqueness unless the case-sensitive option is chosen.
- Leading-wildcard searches on text fields are non-selective.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Long Text

*Multi-line plain text up to 131,072 chars — descriptions, notes. Not filterable in SOQL WHERE.*

### 🌱 Simple

*Beginner - plain language*

A **Long Text Area** stores multiple lines of plain text — descriptions, notes, comments — up to **131,072** characters (you set a limit and visible rows).

### 📏 Limits

*Governor & platform limits*

- Maximum 131,072 characters; default 32,768.
- Not filterable, not sortable, not groupable in reports.
- Cannot be used in formulas or as an External Id.
- Counts toward record storage more heavily than standard fields.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Rich Text

*Formatted text (HTML) with images/links — for content needing styling. Heavier; sanitize input.*

### 🌱 Simple

*Beginner - plain language*

A **Rich Text Area** is like Long Text but supports formatting — bold, lists, links, and images — stored as HTML. Good for content that needs styling.

### 📏 Limits

*Governor & platform limits*

- Maximum 131,072 characters including markup.
- Embedded images count toward file storage.
- A stored-XSS vector if rendered without sanitisation in custom components.
- Not filterable or sortable in reports.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Number

*Numeric values with set precision/scale — quantities, counts. Indexable, usable in math/formulas.*

### 🌱 Simple

*Beginner - plain language*

A **Number** field stores numeric values — quantities, counts, scores. You set **length** (digits left of decimal) and **decimal places** (scale).

### 📏 Limits

*Governor & platform limits*

- Maximum 18 digits total including decimal places.
- Precision plus scale cannot exceed 18.
- Not suitable for currency - use the Currency type to avoid rounding surprises.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Percent

*A number shown/used as a percentage — stored as the number, displayed with %.*

### 🌱 Simple

*Beginner - plain language*

A **Percent** field holds a percentage like 25%. You enter `25` and it displays with a % sign. It's a number type specialised for percentages.

### 📏 Limits

*Governor & platform limits*

- Stored as a number - 50% is stored as 50, not 0.5.
- Maximum 18 digits total including decimals.
- Formula arithmetic must divide by 100 explicitly.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Currency

*Monetary values with locale formatting & multi-currency conversion (CurrencyIsoCode).*

### 🌱 Simple

*Beginner - plain language*

A **Currency** field stores money. It formats with the currency symbol and, in multi-currency orgs, carries a currency code and converts to the corporate currency for roll-ups/reports.

### 📏 Limits

*Governor & platform limits*

- Maximum 18 digits total including decimals.
- In a multi-currency org, values convert at report time unless dated rates are enabled.
- Roll-up summaries across currencies require careful design.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Date

*Calendar date (no time) — birthdays, close dates. Filterable with date literals.*

### 🌱 Simple

*Beginner - plain language*

A **Date** field stores a calendar date with no time — like a close date or birthday. Displayed per the user's locale.

### 📏 Limits

*Governor & platform limits*

- No time and no timezone component.
- Date literals in SOQL evaluate in the running user's timezone.
- Date arithmetic with `addDays()` is DST-safe; second arithmetic on Datetime is not.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Date Time

*A precise moment stored in UTC, displayed in the user's timezone — timestamps.*

### 🌱 Simple

*Beginner - plain language*

A **Date/Time** field stores an exact moment — date plus time. Salesforce keeps it in UTC and shows it in each user's timezone.

### 📏 Limits

*Governor & platform limits*

- Stored in UTC and rendered in the user's timezone.
- `.date()` returns the GMT date, which is often the wrong day for the user.
- Integrations must exchange ISO-8601 with an explicit offset.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Time

*A time-of-day value (no date) — opening hours, appointment slots.*

### 🌱 Simple

*Beginner - plain language*

A **Time** field stores a time of day without a date — like store opening time 09:00 or a slot at 14:30.

### 📏 Limits

*Governor & platform limits*

- No date and no timezone component - it is a wall-clock value.
- Not supported in all formula functions or in some report groupings.
- Comparisons across timezones are meaningless without an accompanying date.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Email

*Validated email address field — enables email actions, Email-to-Case matching.*

### 🌱 Simple

*Beginner - plain language*

An **Email** field stores an email address and validates the format. It powers "send email" actions and clickable mailto links.

### 📏 Limits

*Governor & platform limits*

- Maximum 80 characters.
- Format validation is basic and accepts many invalid real-world addresses.
- Bounced addresses are suppressed silently for future sends.
- Indexed only if marked Unique or External Id.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Phone

*Phone number field — click-to-dial (CTI), light formatting. Store normalized for matching.*

### 🌱 Simple

*Beginner - plain language*

A **Phone** field stores a telephone number. In Lightning it renders as click-to-dial when CTI/softphone is set up.

### 📏 Limits

*Governor & platform limits*

- Maximum 40 characters with no format validation or normalisation.
- Automatic formatting applies only to some locales.
- Matching and deduplication require normalisation you implement yourself.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## URL

*A web address field — clickable hyperlink to external/internal resources.*

### 🌱 Simple

*Beginner - plain language*

A **URL** field stores a web link. It renders as a clickable hyperlink on the record (e.g., a company website or document link).

### 📏 Limits

*Governor & platform limits*

- Maximum 255 characters.
- No validation that the URL resolves or uses a safe scheme.
- Rendered as a clickable link - a phishing vector if populated by untrusted input.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Picklist

*Single-select from defined values — governed, reportable choices. Use Global Value Sets.*

### 🌱 Simple

*Beginner - plain language*

A **Picklist** lets users pick one value from a predefined list (e.g., Industry, Stage). It keeps data consistent and easy to report on.

### 📏 Limits

*Governor & platform limits*

- Maximum 1,000 active values; 500 for a Global Value Set.
- Changing an API value breaks integrations, reports and formulas silently.
- Restricted picklists block API writes of unlisted values.
- Inactive values still count toward some limits.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Multi Picklist

*Multi-select picklist — multiple values per record. Reporting & query gotchas; often an anti-pattern.*

### 🌱 Simple

*Beginner - plain language*

A **Multi-Select Picklist** lets users choose several values at once (e.g., a contact's multiple interests). The selected values are stored together in one field.

### 📏 Limits

*Governor & platform limits*

- Maximum 500 active values; 100 selected values per record.
- Total selected length capped at 4,099 characters.
- `INCLUDES`/`EXCLUDES` filters are non-selective and slow at volume.
- Cannot be used in roll-up summaries or many formula contexts.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Geolocation

*Latitude/longitude compound field — distance queries (DISTANCE/GEOLOCATION).*

### 🌱 Simple

*Beginner - plain language*

A **Geolocation** field stores a latitude and longitude (a map point). It lets you calculate distances — e.g., find accounts near a location.

### 📏 Limits

*Governor & platform limits*

- Counts as three custom fields against the per-object limit.
- `DISTANCE()` filters are non-selective and cannot use an index.
- Not supported in some formula and report contexts.
- Precision options are fixed at field creation.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Encrypted

*Classic Encrypted Text field — masks data in UI. Limited; Shield Platform Encryption is the modern choice.*

### 🌱 Simple

*Beginner - plain language*

An **Encrypted Text** field hides sensitive data (like an ID number) behind a mask, showing it only to users with the "View Encrypted Data" permission. It's the legacy way to protect a single text field.

### 📏 Limits

*Governor & platform limits*

- Classic encrypted text fields are limited to 175 characters and are not filterable or sortable.
- Cannot be used in formulas, roll-ups, or report filters.
- Requires the "View Encrypted Data" permission to see plaintext.
- Shield encryption has its own separate, broader restrictions.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Formula

*Read-only calculated field evaluated at runtime — no storage. Powerful but limit-bound.*

### 🌱 Simple

*Beginner - plain language*

A **Formula** field shows a value calculated from other fields — like `Full Name = First & ' ' & Last`. It's read-only and recalculates automatically; nothing is stored.

### 📏 Limits

*Governor & platform limits*

- Compile size 5,000 bytes; formula text 3,900 characters.
- Maximum 10 unique cross-object relationships per object across all formulas.
- Generally not indexable, so filtering on them is non-selective.
- Evaluated on every query and every row.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Auto Number

*System-generated sequential identifier (e.g., INV-{0000}) — unique human-readable record numbers.*

### 🌱 Simple

*Beginner - plain language*

An **Auto Number** field gives each new record a unique sequential number with a format you choose — like `INV-0001`, `INV-0002`. Salesforce assigns it automatically.

### 📏 Limits

*Governor & platform limits*

- Values are assigned on insert and cannot be edited afterwards.
- Gaps occur on failed inserts and rollbacks - never treat it as a contiguous sequence.
- Format changes apply only to new records.
- Not suitable as a business key across orgs - use an External Id.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Rollup Summary

*Parent-side aggregate of master-detail children (COUNT/SUM/MIN/MAX) — see also Field Types.*

### 🌱 Simple

*Beginner - plain language*

A **Roll-Up Summary** field on a master record automatically aggregates its detail (child) records — count them, or sum/min/max a field. It updates as children change.

### 📏 Limits

*Governor & platform limits*

- Maximum 25 per object.
- Only on master-detail relationships (or via DLRS on lookups).
- Recalculation locks the parent record and is a primary cause of lock contention.
- Roll-up updates do not fire triggers.

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
