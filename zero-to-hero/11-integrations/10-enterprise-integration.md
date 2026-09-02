[Home](../index.md) / [11 · Integrations](index.md) / **Enterprise Integration**

# Enterprise Integration

6 topics · Series 11: Integrations

**Topics on this page**

- [SAP Integration](#sap-integration)
- [Oracle Integration](#oracle-integration)
- [Workday Integration](#workday-integration)
- [ServiceNow Integration](#servicenow-integration)
- [Data Lake Integration](#data-lake-integration)
- [Data Warehouse Integration](#data-warehouse-integration)

## SAP Integration

*Connecting Salesforce with SAP ERP systems.*

### 🌱 Simple

*Beginner - plain language*

**SAP integration** connects Salesforce with SAP ERP (ECC/S4HANA) — syncing customers, orders, inventory, and pricing — typically via middleware (MuleSoft/Boomi), SAP APIs (OData/BAPI/IDoc), or SAP's integration suite.

### 📏 Limits

*Governor & platform limits*

- IDoc and BAPI payloads frequently exceed the 6 MB callout body limit - chunk or use middleware.
- SAP transaction semantics do not map to Salesforce transactions; compensating actions are required.
- Latency is high, so synchronous integration is rarely viable.
- Certificate and credential rotation must be coordinated with the SAP team.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Oracle Integration

*Connecting Salesforce with Oracle systems.*

### 🌱 Simple

*Beginner - plain language*

**Oracle integration** connects Salesforce with Oracle databases, E-Business Suite, or Oracle Fusion/Cloud apps — via Oracle Integration Cloud (OIC), middleware, REST/SOAP APIs, or direct DB connectors.

### 📏 Limits

*Governor & platform limits*

- Direct database access is not possible - an API or middleware layer is mandatory.
- Large result sets exceed the 6 MB callout limit; paginate or use Bulk API.
- Schema changes on the Oracle side break integrations silently.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Workday Integration

*Connecting Salesforce with Workday HCM/Finance.*

### 🌱 Simple

*Beginner - plain language*

**Workday integration** connects Salesforce with Workday (HR/HCM and Finance) — syncing employees, org structure, and financial data — via Workday's web services, RaaS reports, or middleware.

### 📏 Limits

*Governor & platform limits*

- Workday API rate limits and scheduled maintenance windows are outside your control.
- Report-as-a-Service responses can exceed the 6 MB callout limit.
- Worker data is highly sensitive - PII handling and DPA obligations apply.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## ServiceNow Integration

*Connecting Salesforce with ServiceNow ITSM.*

### 🌱 Simple

*Beginner - plain language*

**ServiceNow integration** connects Salesforce with ServiceNow (ITSM/CSM) — syncing incidents, cases, and tasks — via ServiceNow's REST Table API, IntegrationHub, or middleware, often for case-to-incident workflows.

### 📏 Limits

*Governor & platform limits*

- ServiceNow instance rate limits apply on top of Salesforce limits.
- Bidirectional sync of Cases and Incidents needs explicit ownership rules to avoid loops.
- Attachment transfer is bounded by the 6 MB callout body limit.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Data Lake Integration

*Streaming Salesforce data into a data lake.*

### 🌱 Simple

*Beginner - plain language*

**Data lake integration** moves Salesforce data into a data lake (S3, ADLS, Google Cloud Storage) — storing raw, large-scale data in open formats for analytics, ML, and archival.

### 📏 Limits

*Governor & platform limits*

- Bulk API 2.0 is the practical extraction mechanism: 150 million records per 24 hours.
- Full extracts consume the daily API allocation - use incremental with SystemModstamp.
- Deleted records need `queryAll` or a separate deletion feed.
- Data leaving the org carries residency and retention obligations.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Data Warehouse Integration

*Loading curated Salesforce data for BI/analytics.*

### 🌱 Simple

*Beginner - plain language*

**Data warehouse integration** loads structured, curated Salesforce data into a warehouse (Snowflake, BigQuery, Redshift, Synapse) for fast SQL analytics and BI dashboards.

### 📏 Limits

*Governor & platform limits*

- Incremental extraction requires a reliable watermark - SystemModstamp, not LastModifiedDate.
- Hard-deleted records never appear in a query-based feed.
- Formula and roll-up values must be recomputed downstream or extracted explicitly.
- Bulk API limits and the daily API allocation bound refresh frequency.

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
