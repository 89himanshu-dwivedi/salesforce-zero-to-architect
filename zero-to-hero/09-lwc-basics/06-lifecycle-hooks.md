[Home](../index.md) / [09 · LWC Basics](index.md) / **Lifecycle Hooks**

# Lifecycle Hooks

5 topics · Series 9: LWC Basics

**Topics on this page**

- [constructor](#constructor)
- [connectedCallback](#connectedcallback)
- [renderedCallback](#renderedcallback)
- [disconnectedCallback](#disconnectedcallback)
- [errorCallback](#errorcallback)

## constructor

*The first hook — component created, but DOM not yet available.*

### 🌱 Simple

*Beginner - plain language*

The **constructor** runs first when a component is created. You can set initial state, but **not** access child elements or `@api` properties (not set yet) — and must call `super()` first.

### 📏 Limits

*Governor & platform limits*

- Must call `super()` first and cannot access `this.template`.
- Public properties are not yet set - do not read `@api` values here.
- No DOM access and no dispatching events.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## connectedCallback

*Fires when the component is inserted into the DOM — ideal for init/fetch.*

### 🌱 Simple

*Beginner - plain language*

**connectedCallback** runs when the component is inserted into the DOM. It's where you do setup that needs `@api` values — subscribe to events, kick off imperative data loads, register LMS.

### 📏 Limits

*Governor & platform limits*

- Fires before render, so `this.template.querySelector` returns null.
- Can fire more than once if the element is moved in the DOM.
- Subscriptions started here must be cleaned up in `disconnectedCallback`.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## renderedCallback

*Runs after every render — the only safe place for DOM access.*

### 🌱 Simple

*Beginner - plain language*

**renderedCallback** fires after the component renders — and after **every re-render**. It's the only hook where you can safely access rendered child elements via `this.template.querySelector`.

### 📏 Limits

*Governor & platform limits*

- Fires after every render - unguarded work here multiplies with every state change.
- Mutating reactive state here can cause an infinite render loop.
- Use a boolean flag for one-time initialisation such as `loadScript`.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## disconnectedCallback

*Fires on removal from the DOM — the cleanup hook.*

### 🌱 Simple

*Beginner - plain language*

**disconnectedCallback** runs when the component is removed from the DOM. Use it to **clean up** — unsubscribe from channels, clear timers, remove listeners, destroy third-party instances.

### 📏 Limits

*Governor & platform limits*

- Must unsubscribe from LMS, pubsub, timers and manual listeners or the console leaks handlers.
- Not guaranteed to fire on full page unload.
- The component may reconnect later, so cleanup must be idempotent.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## errorCallback

*Captures errors from descendant components for graceful handling.*

### 🌱 Simple

*Beginner - plain language*

**errorCallback(error, stack)** is an error boundary — it catches errors thrown during the lifecycle/render of **descendant** components, letting you show a fallback UI instead of a broken page.

### 📏 Limits

*Governor & platform limits*

- Only catches errors from descendant components, not from the component itself.
- Does not catch errors in async callbacks or promise rejections.
- Wire adapter errors surface through the wire's `error` property instead.

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
