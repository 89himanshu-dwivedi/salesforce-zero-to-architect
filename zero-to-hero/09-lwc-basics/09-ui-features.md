[Home](../index.md) / [09 · LWC Basics](index.md) / **UI Features**

# UI Features

5 topics · Series 9: LWC Basics

**Topics on this page**

- [Lightning Base Components](#lightning-base-components)
- [Toast](#toast)
- [Spinner](#spinner)
- [Modal](#modal)
- [NavigationMixin](#navigationmixin)

## Lightning Base Components

*Pre-built, SLDS-styled, accessible building blocks.*

### 🌱 Simple

*Beginner - plain language*

**Lightning base components** (`lightning-input`, `lightning-button`, `lightning-datatable`, etc.) are ready-made, SLDS-styled, accessible UI building blocks you compose instead of writing raw HTML.

### 📏 Limits

*Governor & platform limits*

- Behaviour and styling differ between Lightning Experience, Experience Cloud and mobile.
- Internal DOM is not stylable - only documented CSS custom properties.
- Some components are unavailable on certain targets.
- Base component APIs change between releases - check release notes.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Toast

*Transient notification messages via ShowToastEvent.*

### 🌱 Simple

*Beginner - plain language*

**Toasts** show brief notification messages (success/error/warning/info). Dispatch a `ShowToastEvent` from `lightning/platformShowToastEvent` with title, message, and variant.

### 📏 Limits

*Governor & platform limits*

- Not supported in all containers - Experience Cloud and some mobile surfaces ignore it.
- Message length is practically limited; long text is truncated.
- Only `sticky` mode persists; others auto-dismiss.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Spinner

*Indicating loading state with lightning-spinner.*

### 🌱 Simple

*Beginner - plain language*

A **spinner** (`lightning-spinner`) shows a loading indicator during async work. Toggle it with a boolean (`lwc:if={isLoading}`) around imperative calls or wire loading.

### 📏 Limits

*Governor & platform limits*

- Purely visual - it does not block user interaction unless you also disable inputs.
- Must be positioned inside a relatively-positioned container to overlay correctly.
- Leaving it visible on an unhandled error is a common UX bug - use `finally`.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Modal

*Overlay dialogs — via LightningModal or custom markup.*

### 🌱 Simple

*Beginner - plain language*

**Modals** are overlay dialogs for focused tasks/confirmations. Use the platform **LightningModal** (`lightning/modal`) base class, or build one with SLDS modal markup toggled by a boolean.

### 📏 Limits

*Governor & platform limits*

- Shadow DOM prevents true full-page overlay from a nested component.
- Focus trapping and accessibility must be handled by you unless using the modal base component.
- Not supported inside all containers, including some Experience Cloud templates.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## NavigationMixin

*Programmatic navigation to pages, records, and components.*

### 🌱 Simple

*Beginner - plain language*

**NavigationMixin** lets a component navigate programmatically — to a record page, list view, URL, or web page — via `this[NavigationMixin.Navigate](pageReference)`.

### 📏 Limits

*Governor & platform limits*

- Not every page type is supported on every surface (desktop, mobile, Experience Cloud).
- Custom URL state parameters must be prefixed `c__`.
- Navigation is asynchronous - do not assume the target has rendered.
- Console tab control requires the Workspace API, not NavigationMixin.

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
