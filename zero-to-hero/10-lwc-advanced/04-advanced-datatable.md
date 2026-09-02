[Home](../index.md) / [10 · LWC Advanced](index.md) / **Advanced Datatable**

# Advanced Datatable

5 topics · Series 10: LWC Advanced

**Topics on this page**

- [Custom Types](#custom-types)
- [Inline Editing](#inline-editing)
- [Row Actions](#row-actions)
- [Dynamic Columns](#dynamic-columns)
- [Infinite Loading](#infinite-loading)

## Custom Types

*Extending lightning-datatable to render custom cell content.*

### 🌱 Simple

*Beginner - plain language*

**Custom data types** let you render non-standard content in a `lightning-datatable` cell (e.g., a picklist, image, or custom component) by extending `LightningDatatable` with a custom template.

### 📏 Limits

*Governor & platform limits*

- Requires extending `LightningDatatable` and a separate template per type.
- Not all standard behaviours (sorting, inline edit) are available on custom types.
- Custom type templates cannot use every base component.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Inline Editing

*Editing datatable cells in place with draft tracking and save.*

### 🌱 Simple

*Beginner - plain language*

**Inline editing** lets users edit cells directly in a `lightning-datatable` by marking columns `editable: true`. Edits are tracked as **draft values** and saved via the `save` event.

### 📏 Limits

*Governor & platform limits*

- Requires the field to be editable and the user to have edit access - FLS wins silently.
- Draft values must be handled and saved by your code; the datatable does not persist them.
- Validation rules fire on save and errors must be surfaced manually.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Row Actions

*Per-row menu actions handled via the rowaction event.*

### 🌱 Simple

*Beginner - plain language*

**Row actions** add a per-row action menu (edit, delete, view) to a datatable via an `action`-type column. Selecting one fires a `rowaction` event with the action and row.

### 📏 Limits

*Governor & platform limits*

- Action menus are limited in depth and do not support nested submenus.
- Dynamic per-row actions require a Promise-based provider and add latency.
- Row action handlers receive only the row and action - not the full component context.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Dynamic Columns

*Building datatable columns at runtime from metadata.*

### 🌱 Simple

*Beginner - plain language*

**Dynamic columns** means generating the datatable's `columns` array at runtime — from configuration or object metadata — instead of hardcoding them, for reusable, configurable tables.

### 📏 Limits

*Governor & platform limits*

- Column definitions must be reassigned, not mutated, to trigger re-render.
- Very wide tables are DOM-bound and degrade quickly.
- Field-level security must be applied server-side before building columns.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Infinite Loading

*Loading more rows on scroll to handle large datasets.*

### 🌱 Simple

*Beginner - plain language*

**Infinite loading** loads more datatable rows as the user scrolls near the bottom — set `enable-infinite-loading` and handle `loadmore` to fetch the next page.

### 📏 Limits

*Governor & platform limits*

- Requires `enable-infinite-loading` and a fixed table height.
- SOQL `OFFSET` caps at 2,000 - use keyset pagination beyond that.
- Accumulated rows are DOM-bound; the browser degrades well before Apex does.

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
