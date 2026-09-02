[Home](../index.md) / [09 · LWC Basics](index.md) / **Events**

# Events

4 topics · Series 9: LWC Basics

**Topics on this page**

- [DOM Events](#dom-events)
- [Custom Events](#custom-events)
- [Event Bubbling](#event-bubbling)
- [Event Propagation](#event-propagation)

## DOM Events

*Handling standard browser events declaratively in templates.*

### 🌱 Simple

*Beginner - plain language*

**DOM events** (click, change, input, submit) are handled declaratively: `onclick={handleClick}` in markup wires to a method in JS that receives the event object.

### 📏 Limits

*Governor & platform limits*

- Shadow DOM retargets events, so `event.target` may not be the element you expect.
- Non-composed events do not cross shadow boundaries.
- Inline handler attributes are not allowed; use `on*` bindings.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Custom Events

*Child-to-parent communication via dispatched CustomEvent.*

### 🌱 Simple

*Beginner - plain language*

**Custom events** let a child notify its parent: the child calls `this.dispatchEvent(new CustomEvent('select', {detail: data}))` and the parent listens with `onselect={handler}`.

### 📏 Limits

*Governor & platform limits*

- Names must be lowercase with no hyphens or camelCase.
- Default `bubbles` and `composed` are both false.
- `detail` should carry primitives - passing live object references shares mutable state.
- Events cannot be cancelled across shadow boundaries unless composed.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Event Bubbling

*How events propagate up the DOM tree to ancestor listeners.*

### 🌱 Simple

*Beginner - plain language*

**Event bubbling** is when an event travels up from the target element through its ancestors. In LWC, a custom event bubbles only if `bubbles: true` is set.

### 📏 Limits

*Governor & platform limits*

- Requires `bubbles: true`; crossing a shadow boundary also needs `composed: true`.
- Composed bubbling events can be caught anywhere in the page - a coupling risk.
- Retargeting changes `event.target` as the event crosses boundaries.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Event Propagation

*The capture and bubble phases and controlling event flow.*

### 🌱 Simple

*Beginner - plain language*

**Event propagation** describes how an event travels: down (capture phase) then up (bubble phase). You control it with `stopPropagation()` and design events with the right `bubbles`/`composed` settings.

### 📏 Limits

*Governor & platform limits*

- Capture phase listeners require explicit registration with `addEventListener`.
- `stopPropagation` does not cross a shadow boundary the event never entered.
- Listeners added manually must be removed in `disconnectedCallback` or they leak.

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
