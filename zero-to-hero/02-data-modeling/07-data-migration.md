[Home](../index.md) / [02 · Data Modeling](index.md) / **Data Migration**

# Data Migration

4 topics · Series 2: Data Modeling

**Topics on this page**

- [Import Wizard](#import-wizard)
- [Data Loader](#data-loader)
- [Bulk API](#bulk-api)
- [ETL Tools](#etl-tools)

## Import Wizard

*Browser-based tool for simple, low-volume imports with built-in duplicate matching.*

### 🌱 Simple

*Beginner - plain language*

The **Data Import Wizard** is a point-and-click tool in Setup for loading data (Accounts, Contacts, Leads, custom objects). It's easy, runs in the browser, and can prevent duplicates while importing.

### 📏 Limits

*Governor & platform limits*

- Maximum 50,000 records per import.
- Supports a limited set of standard and custom objects.
- Cannot perform deletes and cannot use the Bulk API.
- Triggers, workflow rules and validation rules still fire.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Data Loader

*Desktop client for high-volume CRUD on any object, scriptable and schedulable.*

### 🌱 Simple

*Beginner - plain language*

**Data Loader** is a desktop application (and CLI) for importing, exporting, updating, and deleting Salesforce data via CSV. It handles far larger volumes than the Import Wizard and works with **any** object.

### 📏 Limits

*Governor & platform limits*

- Batch size 1-200 for SOAP, up to 10,000 for Bulk API.
- 5 million records per operation via the UI in practice.
- Hard delete bypasses the Recycle Bin irreversibly.
- Triggers fire in chunks of 200 regardless of batch size.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Bulk API

*Asynchronous, high-throughput REST API for loading/querying millions of records.*

### 🌱 Simple

*Beginner - plain language*

The **Bulk API** is built for processing very large data volumes asynchronously. You submit jobs of records in batches and Salesforce processes them in the background — far more efficient than row-by-row APIs.

### 📏 Limits

*Governor & platform limits*

- Bulk 1.0: 10,000 records per batch, 10,000 batches per 24 hours.
- Bulk 2.0: 150 million records per 24 hours.
- Parallel mode causes lock contention on skewed data; serial mode is slower but safe.
- 10-minute processing timeout per batch in Bulk 1.0.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## ETL Tools

*Extract-Transform-Load platforms (MuleSoft, Informatica, Talend…) for complex, ongoing integration.*

### 🌱 Simple

*Beginner - plain language*

**ETL tools** Extract data from sources, Transform it (clean, map, enrich), and Load it into Salesforce (or out). They handle complex, repeatable migrations and integrations beyond simple CSV loading.

### 📏 Limits

*Governor & platform limits*

- All consume the org's daily API request allocation.
- Bulk API limits apply to their Salesforce operations.
- Separate licensing, infrastructure and failure modes to manage.
- Error handling and retry must be configured explicitly - it is not automatic.

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
