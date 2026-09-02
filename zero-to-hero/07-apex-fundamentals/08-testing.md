[Home](../index.md) / [07 · Apex Fundamentals](index.md) / **Testing**

# Testing

9 topics · Series 7: Apex Fundamentals

**Topics on this page**

- [Test Class](#test-class)
- [Test Method](#test-method)
- [Assertions](#assertions)
- [Test Data](#test-data)
- [@testSetup](#testsetup)
- [SeeAllData](#seealldata)
- [Test.startTest()](#test-starttest)
- [Test.stopTest()](#test-stoptest)
- [Code Coverage](#code-coverage)

## Test Class

*An @isTest class containing automated unit tests for Apex.*

### 🌱 Simple

*Beginner - plain language*

A **test class** is marked `@isTest` and holds test methods that verify your code: `@isTest private class OrderServiceTest { ... }`. Salesforce **requires 75% coverage** to deploy.

### 📏 Limits

*Governor & platform limits*

- Test classes must be annotated `@isTest` and do not count toward the 6 MB Apex limit.
- Test data is rolled back and never committed - except non-transactional artefacts like emails.
- `SeeAllData=true` makes tests dependent on org data and is disallowed with `@testSetup`.
- Deployment requires 75% org-wide coverage and every trigger covered.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Test Method

*An individual @isTest method exercising one behavior.*

### 🌱 Simple

*Beginner - plain language*

A **test method** is annotated `@isTest` (or `static testMethod`) and tests a single scenario: `@isTest static void submitOrder_succeeds() {...}`.

### 📏 Limits

*Governor & platform limits*

- Must be `static` and annotated `@isTest` (or the legacy `testMethod` keyword).
- Each method gets its own governor limits and its own data rollback.
- Only one `Test.startTest()/stopTest()` pair per method.
- Test methods cannot be called from non-test code.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Assertions

*Statements that verify expected outcomes — the heart of a test.*

### 🌱 Simple

*Beginner - plain language*

**Assertions** check that results match expectations: `Assert.areEqual(expected, actual, msg)`, `Assert.isTrue(cond)`, `Assert.isNull(x)`. A test without assertions verifies nothing.

### 📏 Limits

*Governor & platform limits*

- Assertion failures cannot be caught by `catch(Exception e)`.
- The modern `Assert` class replaces `System.assert*` and gives clearer messages.
- Coverage without assertions passes deployment but proves nothing.
- Always assert on queried data, not on the in-memory object you just built.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Test Data

*Creating isolated, realistic data for tests via factories.*

### 🌱 Simple

*Beginner - plain language*

**Test data** is created *by the test* (tests can't see org data by default). Build it in the test or a **test data factory** so every test has consistent, isolated records.

### 📏 Limits

*Governor & platform limits*

- Tests cannot see org data unless `SeeAllData=true`, which is discouraged.
- Some objects (User, RecordType, Profile, Organization) are always visible and cannot be created freely.
- Creating Users triggers Mixed DML rules - wrap in `System.runAs`.
- Test data creation consumes the same DML limits as production code.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## @testSetup

*A method that creates shared baseline data once per test class.*

### 🌱 Simple

*Beginner - plain language*

`@testSetup` marks a method that runs **once before** all test methods in the class, creating shared data each method can use (and modify in isolation).

### 📏 Limits

*Governor & platform limits*

- One `@testSetup` method per test class; it must be `static void` with no parameters.
- Not allowed in a class with `SeeAllData=true`.
- Data is rolled back to the post-setup state between test methods, not deleted.
- Records created there carry Ids that differ per test method run - re-query rather than caching Ids.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## SeeAllData

*The annotation that exposes org data to tests — and why to avoid it.*

### 🌱 Simple

*Beginner - plain language*

`@isTest(SeeAllData=true)` lets a test **see existing org data**. It's generally **discouraged** — tests should create their own isolated data instead.

### 📏 Limits

*Governor & platform limits*

- Defaults to false for API version 24.0 and later.
- Makes tests dependent on org data, so they break on a fresh sandbox or scratch org.
- Cannot be combined with `@testSetup`.
- Tests can still modify real data with it enabled - a genuine production risk in sandboxes.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Test.startTest()

*Marks the start of the code under test and resets governor limits.*

### 🌱 Simple

*Beginner - plain language*

`Test.startTest()` marks where the actual code under test begins. It gives that block a **fresh set of governor limits**, separate from your test data setup.

### 📏 Limits

*Governor & platform limits*

- Usable once per test method.
- Resets governor limits for the enclosed code only.
- Async work queued before `startTest()` does not run at `stopTest()`.
- Does not reset the async daily execution allocation.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Test.stopTest()

*Ends the test block, restores limits, and forces async work to run.*

### 🌱 Simple

*Beginner - plain language*

`Test.stopTest()` closes the code-under-test block. Crucially, it **forces queued async jobs** (future/queueable/batch) to execute, so you can assert their results afterward.

### 📏 Limits

*Governor & platform limits*

- Forces queued async work (future, Queueable, Batch) to execute synchronously.
- Only one batch `execute()` chunk runs, regardless of data volume.
- Chained Queueables do not chain further inside a test.
- Limits revert to the pre-`startTest()` counters afterwards.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Code Coverage

*The percentage of Apex lines executed by tests — a deploy requirement, not a quality metric.*

### 🌱 Simple

*Beginner - plain language*

**Code coverage** is the percent of your Apex lines run by tests. You need **75% org-wide** to deploy to production (and every trigger must have *some* coverage).

### 📏 Limits

*Governor & platform limits*

- 75% org-wide required for production deployment; every trigger needs at least some coverage.
- Individual classes have no minimum, but the org-wide figure must pass.
- Coverage figures can be stale - run all tests before a release-critical deploy.
- Coverage counts executed lines only; assertions are not measured at all.

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
