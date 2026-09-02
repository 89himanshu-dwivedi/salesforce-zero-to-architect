[Home](../index.md) / [01 · SF Basics](index.md) / **Company Setup**

# Company Setup

7 topics · Series 1: SF Basics

**Topics on this page**

- [Company Information](#company-information)
- [Fiscal Year](#fiscal-year)
- [Business Hours](#business-hours)
- [Holidays](#holidays)
- [Currency](#currency)
- [Multi Currency](#multi-currency)
- [Advanced Currency](#advanced-currency)

## Company Information

*Org-wide settings: default locale, currency, licenses, storage, org Id — the org's identity.*

### 🌱 Simple

*Beginner - plain language*

**Company Information** (Setup → Company Information) holds your org's core details: organization name, default locale/timezone/currency, primary contact, and a summary of licenses and storage used. It defines org-wide defaults.

### 📏 Limits

*Governor & platform limits*

- Shows licence, storage and API limits - the fastest place to check headroom.
- Default currency cannot be changed once transactions exist.
- Locale and timezone here set the org defaults, not per-user behaviour.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Fiscal Year

*Defines your reporting year — Standard (calendar/offset) or Custom (4-4-5, 13-period).*

### 🌱 Simple

*Beginner - plain language*

The **fiscal year** is the 12-month period your company uses for financial reporting. Salesforce lets you set when your fiscal year starts so forecasts and reports align to your accounting calendar.

### 📏 Limits

*Governor & platform limits*

- Switching from standard to custom fiscal years is effectively irreversible.
- Custom fiscal years break some standard forecasting and reporting features.
- Changing the fiscal year start month recalculates historical periods.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Business Hours

*Defines working hours/timezone used by SLAs, escalation and milestone timers.*

### 🌱 Simple

*Beginner - plain language*

**Business Hours** define when your support team works (e.g., Mon–Fri 9–5 in a timezone). Salesforce uses them so SLA timers and escalations only count working time, not nights/weekends.

### 📏 Limits

*Governor & platform limits*

- Each Business Hours record carries its own timezone - a null or wrong one silently breaks milestone calculations.
- Limited number of Business Hours records per org (typically 1,000).
- Only one can be marked as the org default.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Holidays

*Non-working days that pause SLA/escalation timers when linked to Business Hours.*

### 🌱 Simple

*Beginner - plain language*

**Holidays** are dates your company doesn't work (public holidays, shutdowns). When linked to Business Hours, SLA and escalation timers skip them, so customers aren't counted as overdue on a holiday.

### 📏 Limits

*Governor & platform limits*

- Holidays must be linked to Business Hours records explicitly.
- Recurring holidays follow fixed rules and cannot express arbitrary calendars.
- Limited number of holidays per org.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Currency

*The org's default currency and how monetary fields are stored/displayed.*

### 🌱 Simple

*Beginner - plain language*

**Currency** settings define the money type your org uses (e.g., USD). Every currency field (Amount, Annual Revenue) is shown in this currency unless you enable multiple currencies.

### 📏 Limits

*Governor & platform limits*

- The org's corporate currency cannot be changed once transactions exist.
- Enabling multi-currency is irreversible.
- Currency fields cannot be used in some formula contexts across currencies.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Multi Currency

*Support many currencies with conversion rates — for multinational operations. Irreversible.*

### 🌱 Simple

*Beginner - plain language*

**Multi-Currency** lets your org work in several currencies at once. Each record can have its own currency, and Salesforce converts amounts to the corporate currency for roll-ups and reporting using configured exchange rates.

### 📏 Limits

*Governor & platform limits*

- Cannot be disabled once enabled.
- Roll-up summaries and cross-currency formulas behave differently and need careful design.
- Historical conversion requires Advanced Currency Management.
- Reports convert at the current rate unless dated exchange rates are enabled.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Advanced Currency

*Advanced Currency Management (ACM) — dated exchange rates for accurate historical conversion.*

### 🌱 Simple

*Beginner - plain language*

**Advanced Currency Management (ACM)** adds *dated* exchange rates on top of Multi-Currency. Instead of one current rate, each rate has a date range, so old deals convert using the rate that was correct at the time.

### 📏 Limits

*Governor & platform limits*

- Dated exchange rates apply only to Opportunity and related objects, not to all currency fields.
- Cannot be disabled once enabled.
- Roll-up summaries on Opportunity currency fields are restricted.
- Adds complexity to every currency-based report.

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
