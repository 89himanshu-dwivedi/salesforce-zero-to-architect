[Home](../index.md) / [10 · LWC Advanced](index.md) / **Reactivity**

# Reactivity

5 topics · Series 10: LWC Advanced

**Topics on this page**

- [Reactive Properties](#reactive-properties)
- [Reactive Wire Parameters](#reactive-wire-parameters)
- [Getters](#getters)
- [Render Cycle](#render-cycle)
- [Virtual DOM](#virtual-dom)

## Reactive Properties

*How field reassignment drives re-rendering in LWC.*

### 🌱 Simple

*Beginner - plain language*

**Reactive properties** are fields whose change triggers a re-render. In modern LWC, all fields are reactive — reassigning a field updates the UI. `@track` is only needed for deep reactivity on object/array internals.

### 📏 Limits

*Governor & platform limits*

- Reactivity is triggered by assignment, not by mutation of an existing object or array.
- `@track` is only needed for deep reactivity on objects and arrays.
- Reassigning large arrays on every keystroke re-renders the whole list.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Reactive Wire Parameters

*Driving automatic wire re-fetches with $-prefixed parameters.*

### 🌱 Simple

*Beginner - plain language*

**Reactive wire parameters** use the `$` prefix (`{id: '$recordId'}`) so the wire re-invokes whenever that property changes. Without `$`, the value is static and runs once.

### 📏 Limits

*Governor & platform limits*

- Require the `$` prefix; without it the value is read once at creation.
- An `undefined` parameter prevents the wire from calling Apex at all.
- Rapid parameter changes fire multiple wire invocations - debounce upstream.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Getters

*Computed, reactive, cacheable-per-render derived values.*

### 🌱 Simple

*Beginner - plain language*

**Getters** compute derived values from reactive state for the template (`get fullName(){...}`). They keep templates expression-free and re-evaluate when their backing reactive fields change.

### 📏 Limits

*Governor & platform limits*

- Run on every render - heavy computation here is a direct performance cost.
- Cannot be async and cannot return a Promise for template use.
- Side effects in getters cause unpredictable render behaviour.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Render Cycle

*The sequence from state change to DOM update.*

### 🌱 Simple

*Beginner - plain language*

The **render cycle** is how LWC turns state changes into DOM updates: a reactive change schedules a re-render, the template render function runs, the engine diffs the virtual DOM, and patches only what changed.

### 📏 Limits

*Governor & platform limits*

- Rendering is asynchronous and batched - DOM is not updated synchronously after assignment.
- Mutating reactive state inside `renderedCallback` can loop infinitely.
- Order of child rendering is not guaranteed.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Virtual DOM

*The in-memory representation enabling efficient diffing.*

### 🌱 Simple

*Beginner - plain language*

The **virtual DOM** is an in-memory representation of the UI. LWC builds a new virtual DOM on each render, diffs it against the previous one, and updates only the changed real DOM nodes — making updates efficient.

### 📏 Limits

*Governor & platform limits*

- Diffing cost scales with DOM size - thousands of nodes is the real bottleneck.
- Missing or unstable `key` values force full list re-creation.
- You cannot opt out of the diffing algorithm.

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
