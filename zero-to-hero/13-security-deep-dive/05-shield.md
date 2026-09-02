[Home](../index.md) / [13 · Security Deep Dive](index.md) / **Shield**

# Shield

4 topics · Series 13: Security Deep Dive

**Topics on this page**

- [Deterministic Encryption](#deterministic-encryption)
- [Probabilistic Encryption](#probabilistic-encryption)
- [Event Monitoring](#event-monitoring)
- [Field Audit Trail](#field-audit-trail)

## Deterministic Encryption

*Shield Platform Encryption that allows filtering.*

### 🌱 Simple

*Beginner - plain language*

**Deterministic encryption** (Shield Platform Encryption) encrypts a field so the **same plaintext always yields the same ciphertext** — enabling exact-match filtering and lookups on encrypted data.

### 📏 Limits

*Governor & platform limits*

- Allows exact-match filtering but not ranges, sorting, or LIKE.
- Weaker than probabilistic - identical plaintext produces identical ciphertext.
- Not supported on all field types; formulas and roll-ups are excluded.
- Requires Shield Platform Encryption licensing.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Probabilistic Encryption

*Strongest Shield encryption; not filterable.*

### 🌱 Simple

*Beginner - plain language*

**Probabilistic encryption** (Shield's default) encrypts a field so the **same plaintext yields different ciphertext** each time — maximum confidentiality, but the field can't be filtered or sorted.

### 📏 Limits

*Governor & platform limits*

- Prevents filtering, sorting and grouping on the field entirely.
- Breaks reports, list view filters and SOQL WHERE clauses.
- Not supported on all field types.
- Key rotation is an operational process with re-encryption cost.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Event Monitoring

*Granular log data on user/app activity (Shield).*

### 🌱 Simple

*Beginner - plain language*

**Event Monitoring** (Shield) provides detailed **log event data** — logins, API calls, report exports, page views, Apex executions — for security, performance, and adoption analysis.

### 📏 Limits

*Governor & platform limits*

- Requires the Shield or Event Monitoring add-on.
- Log files are generated daily (hourly with Real-Time) - not real-time by default.
- Retention is 1 day or 30 days depending on licence.
- Meaningful analysis requires exporting to an external platform.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Field Audit Trail

*Extended field history retention and tracking.*

### 🌱 Simple

*Beginner - plain language*

**Field Audit Trail** (Shield) extends **field history tracking** — retaining a forensic-grade history of field changes for up to **10 years**, far beyond standard tracking.

### 📏 Limits

*Governor & platform limits*

- Requires Shield; extends field history retention to up to 10 years.
- Still limited to 60 tracked fields per object with the add-on (20 without).
- Archived history lives in Big Object storage with restricted querying.
- Cannot retroactively capture history that was never tracked.

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
