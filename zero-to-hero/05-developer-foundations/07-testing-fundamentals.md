[Home](../index.md) / [05 · Developer Foundations](index.md) / **Testing Fundamentals**

# Testing Fundamentals

3 topics · Series 5: Developer Foundations

**Topics on this page**

- [Unit Testing](#unit-testing)
- [Integration Testing](#integration-testing)
- [Regression Testing](#regression-testing)

## Unit Testing

*Testing a single unit of logic in isolation — Apex's mandatory test layer.*

### 🌱 Simple

*Beginner - plain language*

**Unit testing** verifies one piece of code (a method/class) in isolation. In Apex, tests are required: production needs **≥75% coverage** and all tests passing. Use `@isTest` and `System.assert` to check results.

### 📏 Limits

*Governor & platform limits*

- Prod requires ≥75% coverage, all tests pass.
- No real callouts/data by default.
- One Test.startTest/stopTest pair per test.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Integration Testing

*Verifying that components and external systems work correctly together.*

### 🌱 Simple

*Beginner - plain language*

**Integration testing** checks that different pieces work **together** — Apex + flows + external systems — end to end, rather than each unit in isolation. It catches problems at the boundaries between components.

### 📏 Limits

*Governor & platform limits*

- Needs SIT environment + partner test endpoints.
- External system availability/data dependencies.
- Harder to fully automate than unit tests.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Regression Testing

*Ensuring new changes don't break existing, previously working functionality.*

### 🌱 Simple

*Beginner - plain language*

**Regression testing** re-runs tests for **existing features** after a change to confirm nothing that used to work is now broken. It guards against unintended side effects of new code/config.

### 📏 Limits

*Governor & platform limits*

- Suite maintenance effort grows with the org.
- UI automation is more brittle than Apex tests.
- Requires CI + preview-org testing discipline.

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
