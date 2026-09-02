[Home](../index.md) / [23 · Production Support & Client Handling](index.md) / **Common Prod Bug Scenarios**

# Common Prod Bug Scenarios

12 topics · Series 23: Production Support & Client Handling

**Topics on this page**

- [Null Pointer in Apex](#null-pointer-in-apex)
- [Trigger Recursion in Prod](#trigger-recursion-in-prod)
- [Bulk Load Hits Limits](#bulk-load-hits-limits)
- [Duplicate Records Created](#duplicate-records-created)
- [Email Not Sending](#email-not-sending)
- [Scheduled Job Stopped](#scheduled-job-stopped)
- [Integration Token Expired](#integration-token-expired)
- [Validation Blocking Integration](#validation-blocking-integration)
- [Record Locked Errors](#record-locked-errors)
- [Wrong Data After Deploy](#wrong-data-after-deploy)
- [Timezone & Date Bugs](#timezone-and-date-bugs)
- [Sharing Visibility Bug](#sharing-visibility-bug)

## Null Pointer in Apex

*Attempt to de-reference a null object — the #1 Apex error.*

### 🌱 Simple

*Beginner - plain language*

`Attempt to de-reference a null object` happens when you use a variable/field that's **null** — an empty SOQL result, an unset field, a missing map key. The fix is **defensive null checks and safe navigation**.

### 📏 Limits

*Governor & platform limits*

- Fields not in the SELECT list are null, not absent - there is no "not loaded" error.
- `Account a = [SELECT ...]` throws `QueryException` when 0 or >1 rows return.
- Safe navigation works on method calls and fields, not on list indexing.
- An uncaught NPE rolls the whole transaction back.
- NPEs in triggers surface to the user as an ugly "Apex trigger ... caused an unexpected exception" page.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Trigger Recursion in Prod

*Trigger re-fires itself causing limits/duplicate work.*

### 🌱 Simple

*Beginner - plain language*

When a trigger updates records that **re-fire the same trigger**, you get recursion: duplicate logic, hit limits, or `Maximum trigger depth exceeded`. The fix is a **static recursion guard** so the logic runs once per transaction/record.

### 📏 Limits

*Governor & platform limits*

- Max stack depth **16** for recursive trigger DML.
- Statics persist for the transaction only - reset between async transactions and batch chunks.
- Workflow field updates re-fire before/after update triggers once.
- Order across multiple triggers on the same object is undefined.
- Governor limits are shared, so recursion burns SOQL and DML quickly.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Bulk Load Hits Limits

*Works for 1 record, fails on a data load — not bulkified.*

### 🌱 Simple

*Beginner - plain language*

Code that passes manual testing often **fails on a bulk load** because it isn't bulkified — SOQL/DML inside loops hit governor limits at 200+ records. The fix is the **bulkification pattern**: query/DML outside loops, use Maps.

### 📏 Limits

*Governor & platform limits*

- Trigger chunk size **200**; Data Loader batch size configurable 1-200 (Bulk API up to 10,000).
- Bulk API 2.0: **150 million** records per 24h rolling window.
- CPU **10s** per transaction; DML **10,000** rows.
- Row lock timeout about **10 seconds**.
- Deferred sharing calculation must be explicitly re-enabled and recalculated.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Duplicate Records Created

*Same record inserted twice — retries, recursion, or no key.*

### 🌱 Simple

*Beginner - plain language*

Duplicates appear from **integration retries without idempotency, trigger recursion, double-click submits, or missing matching rules**. The fix combines an **External Id + upsert**, duplicate rules, and idempotent processing.

### 📏 Limits

*Governor & platform limits*

- `merge` supports Account, Contact, Lead, Case only; max **3** records per call.
- Unique fields are case-insensitive by default; choose the case-sensitive option deliberately.
- Duplicate rules apply to API inserts and can block integrations.
- Matching rules have a limit on active rules per object.
- Upsert on a non-unique External Id throws if multiple matches exist.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Email Not Sending

*Expected emails never arrive — deliverability or limits.*

### 🌱 Simple

*Beginner - plain language*

Emails not sending usually trace to **org Email Deliverability set to "System email only", daily limits, deactivated users, bounce handling, or an alert/Flow not firing**. Check deliverability settings and limits first.

### 📏 Limits

*Governor & platform limits*

- **5,000** external email addresses per org per day via Apex/API.
- **10**`sendEmail` invocations per transaction; **100** recipients per `SingleEmailMessage`.
- Attachments: **10 MB** total per email.
- Sandboxes default to System email only.
- Emails inside a rolled-back transaction are not sent.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Scheduled Job Stopped

*A nightly job silently stopped running.*

### 🌱 Simple

*Beginner - plain language*

Scheduled jobs can stop because they **errored and aborted, the scheduling user was deactivated, code was deployed that conflicts, or they were never rescheduled after a change**. Check **Scheduled Jobs** and **Apex Jobs** for status.

### 📏 Limits

*Governor & platform limits*

- Max **100** scheduled Apex jobs per org.
- Max **5** concurrent scheduled/batch executions.
- Cron expressions cannot schedule more often than hourly without stacking multiple jobs.
- Deploying a scheduled class may require unscheduling first.
- The job runs as the scheduling user with that user's permissions and sharing.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Integration Token Expired

*Callouts suddenly fail with 401 — auth token lapsed.*

### 🌱 Simple

*Beginner - plain language*

Integrations that "worked yesterday" and now return **401/403** usually have an **expired or revoked OAuth token, rotated credentials, or an expired certificate**. Named/External Credentials handle refresh automatically — if you rolled your own auth, the token likely lapsed.

### 📏 Limits

*Governor & platform limits*

- Access token lifetime = the profile's session timeout (default 2 hours).
- Max **5 active access tokens** per user per Connected App.
- Refresh token policy is per Connected App - options include immediate expiry and inactivity expiry.
- Certificates have a maximum validity and must be re-uploaded before expiry.
- Connected App changes take up to **10 minutes** to propagate.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Validation Blocking Integration

*Integration inserts fail on validation rules meant for users.*

### 🌱 Simple

*Beginner - plain language*

A validation rule designed for human input can **block an integration's bulk insert/update** (missing field, format the API doesn't send). The fix: **bypass validations for the integration user via a custom permission**, or make rules context-aware.

### 📏 Limits

*Governor & platform limits*

- Validation rules apply to API and Apex DML, not just the UI.
- Max **500** active validation rules per object.
- Validation rules run in the order-of-execution before after-triggers and are re-run after workflow field updates.
- Custom permissions are cached; `$Permission` evaluation has no SOQL cost.
- Rules cannot be disabled per transaction from Apex - only bypassed by design.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Record Locked Errors

*UNABLE_TO_LOCK_ROW in prod under real concurrency.*

### 🌱 Simple

*Beginner - plain language*

`UNABLE_TO_LOCK_ROW` appears in prod (not dev) because **real concurrency** — parallel jobs, data loads, and skew — makes transactions fight for the same record/parent lock. Fix by **serializing, sorting by parent, smaller batches, and removing skew**.

### 📏 Limits

*Governor & platform limits*

- Row lock wait is about **10 seconds** before the exception.
- Skew threshold: more than **10,000** child records per parent.
- Ownership skew: more than **10,000** records owned by one user.
- `FOR UPDATE` holds the lock until the transaction ends and cannot be used with `ORDER BY`.
- Locks are not visible or queryable - you can only infer them from errors.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Wrong Data After Deploy

*Data looks corrupted/changed right after a release.*

### 🌱 Simple

*Beginner - plain language*

If data looks wrong after a deploy, suspect a **new/changed automation** (trigger/Flow updating fields unexpectedly), a **one-time data script** that ran, or a **changed default/formula**. Identify what changed, stop the bad automation, then correct affected records.

### 📏 Limits

*Governor & platform limits*

- Field History: **20 fields** per object, retained 18-24 months (longer with Field Audit Trail).
- Recycle Bin: **15 days**, capacity 25x your data storage.
- Weekly Export is weekly - up to 7 days of data loss.
- Salesforce Data Recovery is paid, slow, and best-effort.
- Field history is not created for formula, roll-up or some standard fields.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Timezone & Date Bugs

*Dates off by a day / wrong time — timezone handling.*

### 🌱 Simple

*Beginner - plain language*

Date/time bugs come from **timezone differences** between the user, the org, and storage. Salesforce stores Datetime in **GMT** and displays in the user's timezone; `Date` has no timezone. Mishandling conversions makes dates appear **off by a day**.

### 📏 Limits

*Governor & platform limits*

- Datetime is stored in UTC; conversion happens at display and in `format()`.
- SOQL date literals evaluate in the running user's timezone.
- `Date` arithmetic is DST-safe; adding seconds to a Datetime is not.
- Business Hours and Entitlement timezones are independent of the user's.
- Scheduled jobs run in the timezone of the scheduling user.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Sharing Visibility Bug

*User can't see (or wrongly sees) a record — sharing.*

### 🌱 Simple

*Beginner - plain language*

"I can't see the record" (or "I can see one I shouldn't") is a **sharing** issue: OWD, role hierarchy, sharing rules, team membership, or manual shares. Debug by checking the user's **access path** to that specific record.

### 📏 Limits

*Governor & platform limits*

- Max **300** sharing rules per object (up to 50 criteria-based).
- Role hierarchy: **500** roles recommended max, 10 levels deep.
- Sharing recalculation is asynchronous and can take hours in large orgs.
- Apex-managed sharing requires a `RowCause` and is not recalculated automatically.
- Implicit sharing cannot be disabled or inspected directly.

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
