[Home](../index.md) / [22 · Limits & Alternate Solutions](index.md) / **UI Performance Limits**

# UI Performance Limits

10 topics · Series 22: Limits & Alternate Solutions

**Topics on this page**

- [Visualforce View State 170KB](#visualforce-view-state-170kb)
- [High EPT Page Slow](#high-ept-page-slow)
- [Too Many Page Components](#too-many-page-components)
- [Slow Related Lists](#slow-related-lists)
- [List View 2000 Display](#list-view-2000-display)
- [Dashboard 20 Component Limit](#dashboard-20-component-limit)
- [Report Type 4 Object Limit](#report-type-4-object-limit)
- [Console Performance](#console-performance)
- [Inline Edit Limits](#inline-edit-limits)
- [Mobile Performance](#mobile-performance)

## Visualforce View State 170KB

*VF view state capped at 170KB — slim the controller.*

### 🌱 Simple

*Beginner - plain language*

Visualforce serializes page+controller state into **view state**, capped at **170KB**. Large collections/objects held in the controller blow it and slow every postback. Alternate: **mark fields `transient`, query less, paginate, and prefer LWC for new UI**.

### 📏 Limits

*Governor & platform limits*

- 170 KB view state.
- Applies to Visualforce with a custom controller/extension.
- Transient fields are excluded.
- LWC has no view state.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## High EPT Page Slow

*Experienced Page Time too high — find & cut the cost.*

### 🌱 Simple

*Beginner - plain language*

**EPT (Experienced Page Time)** measures how long a Lightning page takes to become usable. High EPT = slow page. You diagnose with the **Lightning Usage App** and browser tools, then cut the biggest cost (heavy components, many server calls, big DOM).

### 📏 Limits

*Governor & platform limits*

- Usage App data is aggregated daily, not real-time.
- Each related list adds query cost.
- Concurrent long-running request limit can be triggered by slow pages.
- Mobile EPT differs from desktop.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Too Many Page Components

*Each component adds load cost — curate the record page.*

### 🌱 Simple

*Beginner - plain language*

Every component on a Lightning page can fire its own server call and render work on load. Dozens of components = slow first paint. Alternate: **fewer components, tabs for lazy loading, and dynamic visibility** so only relevant ones load.

### 📏 Limits

*Governor & platform limits*

- No documented hard cap; practical limits are performance-driven.
- Each component may issue its own server request.
- Shadow DOM adds per-component overhead.
- Conditional visibility still evaluates on the server.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Slow Related Lists

*Related lists on big children load slowly — limit & index.*

### 🌱 Simple

*Beginner - plain language*

Related lists query child records on page load. On objects with **huge or non-selective children**, they're slow. Alternate: **show fewer related lists, limit columns, ensure the relationship field is indexed, and collapse non-essential lists**.

### 📏 Limits

*Governor & platform limits*

- Related lists show a limited number of rows before requiring "View All".
- Each list is a separate query.
- Subject to selectivity rules.
- Cannot be indexed independently.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## List View 2000 Display

*List views render a capped page — filter for speed.*

### 🌱 Simple

*Beginner - plain language*

List views display a bounded page of rows and slow down with **non-selective filters** on big objects. Alternate: **scoped, indexed filters and fewer columns**; use reports/exports for full datasets.

### 📏 Limits

*Governor & platform limits*

- 2,000 records displayed.
- Mass action limits are lower (typically 200 selected).
- Subject to query selectivity.
- Filters on formula fields are non-selective.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Dashboard 20 Component Limit

*Dashboards cap components; too many sources slow refresh.*

### 🌱 Simple

*Beginner - plain language*

A dashboard allows up to **20 components**, each backed by a report — many heavy reports make refresh slow. Alternate: **fewer, focused dashboards, lighter source reports, and CRM Analytics for complex/high-volume analytics**.

### 📏 Limits

*Governor & platform limits*

- 20 components per dashboard.
- Dynamic dashboards: 3/5/10 by edition.
- Each component executes its own report.
- Dynamic dashboards cannot be scheduled.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Report Type 4 Object Limit

*Custom report types join up to 4 objects — model around it.*

### 🌱 Simple

*Beginner - plain language*

A custom report type can relate up to **4 objects**. Needing data from more requires a different approach. Alternate: **flatten needed fields onto a reporting object (via formula/automation) or use CRM Analytics** which joins many datasets.

### 📏 Limits

*Governor & platform limits*

- 4 objects per custom report type.
- Joined reports: 5 blocks.
- Report types cannot traverse arbitrary relationships.
- Formula fields count toward the source object, not as an extra object.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Console Performance

*Service Console with many tabs/components can lag.*

### 🌱 Simple

*Beginner - plain language*

The **Service/Sales Console** keeps multiple tabs and components open at once, multiplying load. Too many open subtabs, heavy components, or auto-loading everything makes it lag. Alternate: **lean console layouts, lazy-loaded components, and limited open tabs**.

### 📏 Limits

*Governor & platform limits*

- Practical tab limits are browser-memory bound.
- Utility bar components are always loaded.
- Each tab retains its own component state.
- Workspace API available only in console apps.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Inline Edit Limits

*Inline/mass edit has row & field constraints.*

### 🌱 Simple

*Beginner - plain language*

Inline editing in list views/related lists has constraints (certain field types and some objects aren't inline-editable, and mass inline edit is bounded). Alternate for bulk changes: **Data Loader, a custom LWC mass-edit, or a Flow**.

### 📏 Limits

*Governor & platform limits*

- Not available for formula, roll-up, auto-number fields.
- Mass inline edit limited to a small selection count.
- Requires edit access at object, field and record level.
- Custom datatable types have restricted edit support.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Mobile Performance

*Salesforce mobile is constrained — design light for it.*

### 🌱 Simple

*Beginner - plain language*

On the **Salesforce mobile app**, device CPU/network are limited, so heavy pages and large components feel slow. Alternate: **mobile-optimized pages, fewer components, smaller payloads, and cacheable data**.

### 📏 Limits

*Governor & platform limits*

- Mobile page assignment is separate from desktop.
- Some components and navigation types are unsupported.
- Iframe rendering is inconsistent.
- Offline capability is limited to specific features.

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
