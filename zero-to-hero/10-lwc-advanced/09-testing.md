[Home](../index.md) / [10 · LWC Advanced](index.md) / **Testing**

# Testing

5 topics · Series 10: LWC Advanced

**Topics on this page**

- [Jest](#jest)
- [Mock Apex](#mock-apex)
- [Mock Wire](#mock-wire)
- [Snapshot Testing](#snapshot-testing)
- [Coverage](#coverage)

## Jest

*Unit testing LWC with sfdx-lwc-jest.*

### 🌱 Simple

*Beginner - plain language*

**Jest** (via `@salesforce/sfdx-lwc-jest`) is the framework for unit-testing LWC JavaScript — rendering components in a simulated DOM (jsdom) and asserting behavior without a Salesforce org.

### 📏 Limits

*Governor & platform limits*

- Runs in jsdom - no real browser, no real Salesforce runtime.
- All wire adapters and Apex imports must be mocked.
- Navigation, LMS and platform modules require the official stubs.
- Jest coverage does not count toward the Salesforce 75% requirement.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Mock Apex

*Stubbing imperative and wired Apex calls in Jest.*

### 🌱 Simple

*Beginner - plain language*

**Mocking Apex** replaces real server calls in Jest with controlled fakes — so tests run offline and you control returned data, errors, and call assertions.

### 📏 Limits

*Governor & platform limits*

- Apex modules must be mocked with `jest.mock()` - real calls never happen.
- Mocks must resolve or reject explicitly; unhandled promises fail silently.
- Mocked data does not validate against the real Apex signature.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Mock Wire

*Driving @wire data in tests via wire adapter mocks.*

### 🌱 Simple

*Beginner - plain language*

**Mocking wires** uses sfdx-lwc-jest's wire adapter mocks to push controlled data/errors into a component's `@wire` — so you test how it reacts to wired results.

### 📏 Limits

*Governor & platform limits*

- Requires the wire adapter test utilities (`createTestWireAdapter` or the LDS test stubs).
- Emitting data is asynchronous - flush promises before asserting.
- Wire error paths must be tested explicitly; they are easy to miss.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Snapshot Testing

*Asserting rendered output matches a saved snapshot.*

### 🌱 Simple

*Beginner - plain language*

**Snapshot testing** captures a component's rendered DOM and compares future runs against the saved snapshot — flagging any unintended markup changes.

### 📏 Limits

*Governor & platform limits*

- Snapshots break on any markup change, including base component upgrades.
- They assert structure, not behaviour - low diagnostic value on failure.
- Large snapshots are unreviewable and get blindly updated.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Coverage

*Measuring and interpreting LWC test coverage.*

### 🌱 Simple

*Beginner - plain language*

**Coverage** measures how much of your LWC JS is exercised by Jest tests (lines/branches/functions). Run with `--coverage`; aim for meaningful coverage of logic, not just a percentage.

### 📏 Limits

*Governor & platform limits*

- Jest coverage is separate from Apex coverage and does not affect deployment.
- Coverage thresholds must be configured in `jest.config.js`.
- High coverage without assertions proves nothing.

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
