[Home](../index.md) / [10 · LWC Advanced](index.md) / **Advanced Rendering**

# Advanced Rendering

4 topics · Series 10: LWC Advanced

**Topics on this page**

- [Dynamic Components](#dynamic-components)
- [Slots](#slots)
- [Named Slots](#named-slots)
- [Conditional Rendering](#conditional-rendering)

## Dynamic Components

*Instantiating components at runtime via lwc:component / lwc:is.*

### 🌱 Simple

*Beginner - plain language*

**Dynamic components** let you decide **which** component to render at runtime — using `<lwc:component lwc:is={componentConstructor}>` with a dynamically imported constructor.

### 📏 Limits

*Governor & platform limits*

- `lwc:dynamic` and `lwc:component` require specific API versions.
- The imported constructor must be resolvable at build time unless using dynamic import.
- Dynamic imports add a network round trip on first use.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Slots

*Composition by projecting parent-provided markup into a child.*

### 🌱 Simple

*Beginner - plain language*

**Slots** (`<slot>`) let a child component render markup provided by its parent — enabling reusable container/layout components that wrap arbitrary content.

### 📏 Limits

*Governor & platform limits*

- Slotted content is owned by the parent, so parent CSS applies, not the child's.
- Slot content is not reactive to the child's internal state.
- Cannot query slotted elements with `this.template.querySelector` - use `assignedElements`.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Named Slots

*Multiple labeled insertion points for structured composition.*

### 🌱 Simple

*Beginner - plain language*

**Named slots** let a child define multiple insertion points (`<slot name="header">`) so the parent can place content into specific regions via `slot="header"`.

### 📏 Limits

*Governor & platform limits*

- Unmatched slot names render nothing, silently.
- Default slot content only renders when no content is passed.
- Slots cannot be conditionally named at runtime.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Conditional Rendering

*Choosing render vs hide for performance and lifecycle correctness.*

### 🌱 Simple

*Beginner - plain language*

**Conditional rendering** shows/hides UI with `lwc:if/elseif/else` (adds/removes from DOM) or CSS (`class="slds-hide"`, keeps in DOM). The choice affects lifecycle and performance.

### 📏 Limits

*Governor & platform limits*

- Removed elements are destroyed - state and scroll position are lost.
- Re-adding re-runs `connectedCallback` and `renderedCallback`.
- Conditions must be properties or getters, not inline expressions.

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
