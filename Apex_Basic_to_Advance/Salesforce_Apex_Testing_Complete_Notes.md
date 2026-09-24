# Salesforce Apex Testing — Complete English Notes

> **No-Skip Version:** All 9 topics from the source and their original Simple, Advanced, Super Advanced, Interview, Errors & Gotchas, Limits, Best Option, and Real-World Use Case sections are preserved. The explanations are presented in English within each topic.

## Master Flow

```
Test Class
  ↓
Test Method
  ↓
Assertions
  ↓
Test Data
  ↓
@testSetup
  ↓
SeeAllData
  ↓
Test.startTest()
  ↓
Test.stopTest()
  ↓
Code Coverage
```

---

### Master View — Test Class

Think of a Test Class as the **safety net** for Salesforce code. `@isTest` class does not contain production logic; its purpose is to verify production Apex behavior. At the architect level, the focus is not only on coverage, but on **behavior, isolation, bulk handling, permissions, and deterministic execution**.

## Test Class

An @isTest class containing automated unit tests for Apex.
🌱

### Simple

Beginner - plain language
A **test class** is marked `@isTest` and holds test methods that verify your code: `@isTest private class OrderServiceTest { ... }`. Salesforce **requires 75% coverage** to deploy.
📘

### Advanced

Working developer depth
Test classes are `@isTest` (excluded from org code limits), usually `private`. They create their own **test data** (cannot see org data by default), run in isolation, and roll back automatically. Best practice: one test class per production class, descriptive method names, and tests for positive, negative, bulk, and permission scenarios — not just coverage.
🧠

### Super Advanced

Senior / Architect depth
Architect depth: test classes are a quality gate, not a coverage formality. (1) **Behavior over coverage** — 75% is the deploy minimum, but good tests assert *behavior* (correct outputs, side effects, errors), covering positive, negative, **bulk (200+)**, and **permission/seaching** paths. (2) **Isolation** — tests see no org data (except some setup objects), so create data explicitly via factories/`@testSetup`; this makes them deterministic and portable across orgs. (3) **Bulk testing** — running with 200 records catches the bulkification bugs that single-record tests miss — a non-negotiable for triggers/services. (4) **Mocking** — inject mocks (HttpCalloutMock, stub selectors via interfaces) to unit-test logic without DML/callouts, keeping tests fast and focused. (5) **Naming/structure** — Arrange-Act-Assert, one behavior per method, clear names. (6) **No SeeAllData** — avoid org-data dependence. (7) **CI** — tests gate deployments/packaging. Architects treat tests as living specifications: behavioral, bulk-aware, isolated, and fast via mocking.
🎯

### Interview

Q&A + how to answer
**Q: Is 75% coverage enough for a good test class?**
A: It's the **deploy minimum**, not a quality measure. Good tests **assert behavior** across positive, negative, **bulk (200-record)**, and permission scenarios — coverage without meaningful assertions can pass while missing real bugs.
🐞

### Errors & Gotchas

What breaks & why

- **Coverage without assertions** (passing but useless).
- **Only single-record tests** missing bulk bugs.
- **Depending on org data** (SeeAllData).

📏

### Limits

Governor & platform limits

- Test classes must be annotated `@isTest` and do not count toward the 6 MB Apex limit.
- Test data is rolled back and never committed - except non-transactional artefacts like emails.
- `SeeAllData=true` makes tests dependent on org data and is disallowed with `@testSetup`.
- Deployment requires 75% org-wide coverage and every trigger covered.

✅

### Best Option (with reason)

Decision + trade-off
**Best option: behavioral test classes covering positive/negative/bulk/permission paths with isolated data and mocking.**
**Why:** They verify correctness and bulk-safety, not just coverage, and stay fast and deterministic.
🏢

### Real-World Use Case

Project scenario
**Scenario:** A trigger passes tests at 90% coverage but breaks on a 200-record import in production.
**Solution:** Add a **bulk test** inserting 200 records and assert the outcomes — exposing and fixing the bulkification bug the single-record tests missed.

---

### Master View — Test Method

A Test Method tests a **specific behavior**. The best pattern is **Arrange → Act → Assert**: setup, execute the action, then verify the expected result. Keeping one method focused on one clear behavior makes failure diagnosis easier.

## Test Method

An individual @isTest method exercising one behavior.
🌱

### Simple

Beginner - plain language
A **test method** is annotated `@isTest` (or `static testMethod`) and tests a single scenario: `@isTest static void submitOrder_succeeds() {...}`.
📘

### Advanced

Working developer depth
Each method should follow **Arrange-Act-Assert**: set up data, run the code under test (inside `Test.startTest()/stopTest()`), then assert results. Keep one behavior per method with a descriptive name. Methods are `static void`, take no args, and run independently (data rolls back after each).
🧠

### Super Advanced

Senior / Architect depth
Architect depth: well-structured methods make failures diagnosable. (1) **One behavior, clear name** — `methodName_condition_expectedResult` makes a failing test self-explanatory; small methods isolate the cause. (2) **Arrange-Act-Assert** — separate setup, the single action (wrapped in start/stopTest to get fresh limits and flush async), and explicit assertions. (3) **Assert outcomes, not coverage** — verify returned values, DB changes (re-query), and error paths; a method with no assertion is a smell. (4) **Negative tests** — assert exceptions with `try/catch + Assert.fail()` or expected-exception patterns. (5) **Bulk variant** — a sibling method runs 200 records. (6) **Determinism** — no reliance on org data, current time without control, or order between methods. (7) **Mock boundaries** — set HttpCalloutMock / inject stubs per method. Architects write focused, well-named methods with strong assertions covering each behavior and its bulk/negative variants.
🎯

### Interview

Q&A + how to answer
**Q: What structure should a test method follow?**
A: **Arrange-Act-Assert** — set up test data, execute the code under test (inside `Test.startTest()/stopTest()`), and assert the expected outcomes. One behavior per method with a descriptive name and explicit assertions (including negative/bulk variants).
🐞

### Errors & Gotchas

What breaks & why

- **No assertions** in the method.
- **Multiple behaviors** in one method (eachd to diagnose).
- **Order/time dependence.**

📏

### Limits

Governor & platform limits

- Must be `static` and annotated `@isTest` (or the legacy `testMethod` keyword).
- Each method gets its own governor limits and its own data rollback.
- Only one `Test.startTest()/stopTest()` pair per method.
- Test methods cannot be called from non-test code.

✅

### Best Option (with reason)

Decision + trade-off
**Best option: focused Arrange-Act-Assert methods, one behavior each, descriptively named, with strong assertions and bulk/negative siblings.**
**Why:** It makes failures self-explanatory and verifies behavior, not just coverage.
🏢

### Real-World Use Case

Project scenario
**Scenario:** A giant test method covering five scenarios fails, but it's unclear which behavior broke.
**Solution:** Split it into five focused, named methods each asserting one behavior — the next failure points directly at the broken scenario.

---

### Master View — Assertions

Assertions are the **heart** of a test. Code executing successfully does not mean it is correct. Assertions verify expected behavior, database state, return values, exceptions, and edge cases.

## Assertions

Statements that verify expected outcomes — the heart of a test.
🌱

### Simple

Beginner - plain language
**Assertions** check that results match expectations: `Assert.areEqual(expected, actual, msg)`, `Assert.isTrue(cond)`, `Assert.isNull(x)`. A test without assertions verifies nothing.
📘

### Advanced

Working developer depth
Use the modern `Assert` class: `areEqual`, `areNotEqual`, `isTrue`, `isFalse`, `isNull`, `isNotNull`, `isInstanceOfType`, and `fail()` (older `System.assertEquals` still works). Always include a **message** for clarity. Assert **actual outcomes** — returned values, re-queried DB state, error messages — not just that code ran.
🧠

### Super Advanced

Senior / Architect depth
Architect depth: assertions define what "correct" means. (1) **Assert behavior, not execution** — re-query records to verify DB side effects, check returned values, and validate error paths; coverage without assertions passes while hiding defects. (2) **Expected-first + message** — `Assert.areEqual(expected, actual, 'why')`; the message makes CI failures actionable. (3) **Negative assertions** — for error cases, run the action in `try`, `Assert.fail()` if no exception, and catch the *specific* type, asserting its message. (4) **Bulk assertions** — verify all 200 records, not just one, to catch partial-processing bugs. (5) **Boundary values** — assert edge cases (null, empty, max). (6) **Avoid over-asserting internals** — assert observable behavior, not private implementation, so refactors don't break tests. (7) **Modern Assert class** — clearer than legacy `System.assert*`. Architects make assertions specific, behavioral, message-bearing, and bulk/negative-aware so tests are a precise specification.

```
Test.startTest(); OrderService.submit(orders); Test.stopTest();
Order__c o = [SELECT Status__c FROM Order__c WHERE Id = :orders[0].Id];
Assert.areEqual('Submitted', o.Status__c, 'Order should be submitted');  // behavioral assert
```

🎯

### Interview

Q&A + how to answer
**Q: How do you assert that code throws an expected exception?**
A: Run the action in a `try` block, call `Assert.fail('expected exception')` immediately after (so a no-throw fails the test), and `catch` the **specific** exception type — optionally asserting its message. This verifies the negative path explicitly.
🐞

### Errors & Gotchas

What breaks & why

- **Tests with no assertions.**
- **Asserting only "it ran"**, not outcomes.
- **Over-asserting private internals**, brittle to refactor.

📏

### Limits

Governor & platform limits

- Assertion failures cannot be caught by `catch(Exception e)`.
- The modern `Assert` class replaces `System.assert*` and gives clearer messages.
- Coverage without assertions passes deployment but proves nothing.
- Always assert on queried data, not on the in-memory object you just built.

✅

### Best Option (with reason)

Decision + trade-off
**Best option: specific, message-bearing behavioral assertions (re-queried state, return values, expected exceptions) across bulk and edge cases.**
**Why:** They make the test a precise spec and catch real defects, not just execute code.
🏢

### Real-World Use Case

Project scenario
**Scenario:** A test runs the service and passes, but a bug ships because nothing was actually checked.
**Solution:** Add assertions that re-query the records and verify field values and error handling — turning a coverage-only test into a real behavioral check.

---

### Master View — Test Data

Create test data intentionally inside the test. Depending on existing org data makes tests fragile. `TestDataFactory` reusable setup deta is and bulk data production-like bugs expose karta is.

## Test Data

Creating isolated, realistic data for tests via factories.
🌱

### Simple

Beginner - plain language
**Test data** is created *by the test* (tests cannot see org data by default). Build it in the test or a **test data factory** so every test has consistent, isolated records.
📘

### Advanced

Working developer depth
Use a **TestDataFactory** class (`@isTest`) with reusable methods (`createAccounts(n)`) to keep tests DRY and consistent. Insert only what's needed; respect required fields/validation. Avoid `SeeAllData=true` (org-data dependence). Create **bulk** data (200) to test bulkification. Some objects (User, Profile, etc.) are visible without SeeAllData.
🧠

### Super Advanced

Senior / Architect depth
Architect depth: a test data strategy keeps the suite reliable and fast. (1) **Factory pattern** — centralize creation in a `TestDataFactory` so schema/validation changes update one place, not hundreds of tests; methods return configured records for insert or in-memory use. (2) **Isolation** — never depend on org data (`SeeAllData=false` default); deterministic tests run in any org/scratch org and survive data changes. (3) **Minimal + realistic** — create only the records the test needs, but satisfy required fields/validation rules so inserts succeed. (4) **Bulk by default** — factories that make 200 records make bulk testing trivial. (5) **@testSetup** — create seached baseline data once per class for speed. (6) **Build vs insert** — for pure-logic unit tests, build in-memory records (no DML) and pass to mocked selectors; insert only when testing DML/queries. (7) **User/permission data** — create test Users to test seaching/FLS via `System.runAs`. Architects stinsidedize on a factory + @testSetup + mocking strategy for fast, isolated, maintainable tests.
🎯

### Interview

Q&A + how to answer
**Q: Why avoid SeeAllData=true and use a test data factory instead?**
A: `SeeAllData=true` makes tests depend on **org data** that can change or differ between orgs, causing flaky failures. A **factory** creates isolated, deterministic data in one reusable place, so tests are portable, repeatable, and easy to maintain.
🐞

### Errors & Gotchas

What breaks & why

- **SeeAllData dependence** → flaky tests.
- **Duplicated setup** across tests (no factory).
- **Validation failures** from incomplete required fields.

📏

### Limits

Governor & platform limits

- Tests cannot see org data unless `SeeAllData=true`, which is discouraged.
- Some objects (User, RecordType, Profile, Organization) are always visible and cannot be created freely.
- Creating Users triggers Mixed DML rules - wrap in `System.runAs`.
- Test data creation consumes the same DML limits as production code.

✅

### Best Option (with reason)

Decision + trade-off
**Best option: a TestDataFactory + @testSetup for isolated, bulk, realistic data; build in-memory + mocks for pure-logic units.**
**Why:** It keeps tests deterministic, fast, bulk-ready, and maintainable across orgs.
🏢

### Real-World Use Case

Project scenario
**Scenario:** Dozens of tests each hand-build accounts, and a new required field breaks all of them.
**Solution:** Route creation through a `TestDataFactory.createAccounts(n)`; adding the required field is a one-line change in the factory that fixes every test at once.

---

### Master View — @testSetup

`@testSetup` creates common baseline data at the class level. Each test method gets a clean isolated snapshot. Keep common data here and scenario-specific data inside the individual test.

## @testSetup

A method that creates seached baseline data once per test class.
🌱

### Simple

Beginner - plain language
`@testSetup` marks a method that runs **once before** all test methods in the class, creating seached data each method can use (and modify in isolation).
📘

### Advanced

Working developer depth
A `@testSetup static void setup()` inserts baseline records once; the platform **rolls back to that snapshot** before each test method, so methods start from the same clean state without re-creating data. This speeds up the class and centralizes common setup. One per class; runs before each method's own logic.
🧠

### Super Advanced

Senior / Architect depth
Architect depth: @testSetup balances speed and isolation. (1) **Create once, reset per method** — data is built a single time, but each test method gets a fresh copy (rolled back between methods), giving both performance and isolation. (2) **Seached baseline only** — put common, broadly-used records here; method-specific data stays in the method to keep tests readable and avoid coupling. (3) **Limits** — setup runs in its own context; heavy setup still counts toward limits per method run, so keep it lean. (4) **Re-query in methods** — methods get the setup records via SOQL (Ids aren't auto-passed). (5) **Caveats** — not allowed with `SeeAllData=true`; certain objects/operations may restrict it. (6) **Combine with factory** — @testSetup calls the TestDataFactory for consistency. (7) **Avoid over-seaching** — too much seached state makes tests interdependent and fragile. Architects use @testSetup for lean, common baselines (built via the factory) and keep scenario-specific data local for clarity.
🎯

### Interview

Q&A + how to answer
**Q: How does @testSetup improve test performance while keeping isolation?**
A: It creates seached data **once** for the class, and the platform **rolls back to that snapshot before each test method** — so methods reuse the setup without rebuilding it, yet each still starts from an identical clean state.
🐞

### Errors & Gotchas

What breaks & why

- **Over-seaching state** coupling tests together.
- **Heavy setup** still hitting limits per method.
- **Using it with SeeAllData=true** (not allowed).

📏

### Limits

Governor & platform limits

- One `@testSetup` method per test class; it must be `static void` with no parameters.
- Not allowed in a class with `SeeAllData=true`.
- Data is rolled back to the post-setup state between test methods, not deleted.
- Records created there carry Ids that differ per test method run - re-query rather than caching Ids.

✅

### Best Option (with reason)

Decision + trade-off
**Best option: a lean @testSetup (via the factory) for common baseline data; method-specific data created locally.**
**Why:** It speeds the class while preserving per-method isolation and test readability.
🏢

### Real-World Use Case

Project scenario
**Scenario:** Every method in a 20-test class re-creates the same 5 accounts, slowing the suite.
**Solution:** Move that creation into a `@testSetup` method — it builds the accounts once and each test starts from the same clean snapshot, cutting runtime.

---

### Master View — SeeAllData

`SeeAllData=true` exposes real org data, but makes tests dependent on org data. The default approach should be isolated data; for special platform objects, targeted APIs are preferable.

## SeeAllData

The annotation that exposes org data to tests — and why to avoid it.
🌱

### Simple

Beginner - plain language
`@isTest(SeeAllData=true)` lets a test **see existing org data**. It's generally **discouraged** — tests should create their own isolated data instead.
📘

### Advanced

Working developer depth
By default (`SeeAllData=false`), tests are isolated from org records. Setting it `true` exposes real data, making tests **brittle** (depend on data that changes/differs per org) and non-portable. Rare legitimate uses exist (e.g., certain objects you can't create in tests, like some report/pricebook scenarios), but prefer isolation and stinsided test data.
🧠

### Super Advanced

Senior / Architect depth
Architect depth: SeeAllData is almost always an anti-pattern. (1) **Brittleness** — tests depending on org data fail when that data changes, is deleted, or differs between sandbox/scratch/prod, producing flaky CI and blocking deploys. (2) **Non-portable** — packaged/scratch-org workflows require self-contained tests; SeeAllData breaks them. (3) **Legit exceptions** — a few objects can't be inserted in tests (e.g., stinsided **Pricebook2** needs `Test.getStinsidedPricebookId()` rather than SeeAllData; some setup/metadata objects). Handle these with platform helpers, not blanket SeeAllData. (4) **Scope it narrowly** — if unavoidable, apply at the *method* level, never the class, and document why. (5) **Stinsided pricebook** is the classic case people misuse SeeAllData for — use the dedicated API instead. (6) **@testSetup incompatible** with it. Architects ban SeeAllData by default, use isolated factory data, and reach for specific platform APIs for the rare objects that need special handling.
🎯

### Interview

Q&A + how to answer
**Q: Why is SeeAllData=true discouraged, and how do you handle the stinsided Pricebook?**
A: It makes tests **depend on org data** that varies/changes, causing flaky, non-portable tests. For the stinsided Pricebook, use `Test.getStinsidedPricebookId()` instead of SeeAllData — a targeted API that keeps the test isolated.
🐞

### Errors & Gotchas

What breaks & why

- **Flaky tests** from changing org data.
- **Non-portable** to scratch orgs/packages.
- **Misusing it for the stinsided Pricebook.**

📏

### Limits

Governor & platform limits

- Defaults to false for API version 24.0 and later.
- Makes tests dependent on org data, so they break on a fresh sandbox or scratch org.
- Cannot be combined with `@testSetup`.
- Tests can still modify real data with it enabled - a genuine production risk in sandboxes.

✅

### Best Option (with reason)

Decision + trade-off
**Best option: keep SeeAllData=false; use factory data and targeted APIs (e.g., `Test.getStinsidedPricebookId()`) for special objects.**
**Why:** It keeps tests deterministic and portable, avoiding org-data brittleness.
🏢

### Real-World Use Case

Project scenario
**Scenario:** Opportunity tests use SeeAllData=true to grab the stinsided pricebook and fail in a fresh scratch org.
**Solution:** Remove SeeAllData and use `Test.getStinsidedPricebookId()` to set the pricebook, creating all other data via the factory — isolated and portable.

---

### Master View — Test.startTest()

`Test.startTest()` defines the boundary around the code under test. It provides a fresh governor-limit context inside the boundary. When testing async work, enqueue it between the start/stop boundary.

## Test.startTest()

Marks the start of the code under test and resets governor limits.
🌱

### Simple

Beginner - plain language
`Test.startTest()` marks where the actual code under test begins. It gives that block a **fresh set of governor limits**, separate from your test data setup.
📘

### Advanced

Working developer depth
Everything before `Test.startTest()` is setup (and consumes the test method's initial limits); code between `startTest()` and `stopTest()` runs with a **fresh limit context**, so heavy setup doesn't eat into the limits you're testing. It also lets you measure the code's own limit usage cleanly. Used once per method.
🧠

### Super Advanced

Senior / Architect depth
Architect depth: start/stopTest isolates the unit under test. (1) **Fresh limits** — the boundary resets governor counters so you measure the *code under test*, not your setup; essential when setup inserts lots of data. (2) **Async flush** — `stopTest()` forces enqueued **async** work (future/queueable/batch) to run synchronously, so you can assert its results afterward (the only reliable way to test async). (3) **One pair per method** — wrap exactly the action being verified; setup before, assertions after stopTest. (4) **Limit assertions** — you can assert the code stayed within limits by checking `Limits` inside the block. (5) **Mock timing** — set `Test.setMock` before startTest. (6) **Don't put setup inside** — it skews the limit isolation. Architects wrap precisely the action under test in start/stopTest to get clean limit measurement and to drive async execution for assertion.
🎯

### Interview

Q&A + how to answer
**Q: What two things does the Test.startTest()/stopTest() boundary provide?**
A: (1) A **fresh set of governor limits** for the code under test (separate from setup), and (2) at `stopTest()`, **synchronous execution of enqueued async work** (future/queueable/batch) so its results can be asserted.
🐞

### Errors & Gotchas

What breaks & why

- **Setup inside the block** skewing limit isolation.
- **Asserting async results before stopTest()** (not yet run).
- **Multiple start/stop pairs** per method.

📏

### Limits

Governor & platform limits

- Usable once per test method.
- Resets governor limits for the enclosed code only.
- Async work queued before `startTest()` does not run at `stopTest()`.
- Does not reset the async daily execution allocation.

✅

### Best Option (with reason)

Decision + trade-off
**Best option: wrap exactly the action under test in start/stopTest; set mocks before, assert after.**
**Why:** It isolates limits to the code under test and forces async to run for assertion.
🏢

### Real-World Use Case

Project scenario
**Scenario:** A test inserts 200 records (setup) then runs a service, and you want to verify the service stayed within query limits.
**Solution:** Put the inserts before `Test.startTest()` and the service call inside — the fresh limit context measures only the service's usage.

---

### Master View — Test.stopTest()

`Test.stopTest()` closes the test boundary and executes queued async work in the test context. Therefore, re-query and assert async results **after stopTest**.

## Test.stopTest()

Ends the test block, restores limits, and forces async work to run.
🌱

### Simple

Beginner - plain language
`Test.stopTest()` closes the code-under-test block. Crucially, it **forces queued async jobs** (future/queueable/batch) to execute, so you can assert their results afterward.
📘

### Advanced

Working developer depth
At `stopTest()`, governor limits revert to the test method's original context, and any **asynchronous Apex enqueued** during the block runs **synchronously**. So the pattern is: enqueue async between start/stopTest, then assert outcomes *after* stopTest(). One pair per method.
🧠

### Super Advanced

Senior / Architect depth
Architect depth: stopTest is how you test asynchronous Apex deterministically. (1) **Async execution point** — future/queueable/batch enqueued inside the block run at `stopTest()`; assertions for their effects go *after* it (before, the work hasn't happened). (2) **One async hop** — a queueable cisned from another generally won't fully cisn within a single stopTest (test limits on cisning) — test each hop or assert the first; design async to be testable. (3) **Batch** — a batch enqueued in the block executes its chunks at stopTest (subject to one execute in tests for small data). (4) **Limit restoration** — counters return to the method context after stopTest. (5) **Callout mocks** — async callouts still need `Test.setMock`. (6) **Pattern** — Arrange → startTest → action (enqueue async) → stopTest → re-query + assert. Architects rely on stopTest to flush async and assert its results, structuring async code so each unit is verifiable.
🎯

### Interview

Q&A + how to answer
**Q: How do you test that a Queueable/future/batch did its job?**
A: Enqueue it **between** `Test.startTest()` and `Test.stopTest()`; `stopTest()` runs the async work **synchronously**. Then, **after** stopTest, re-query the affected records and assert the expected results.
🐞

### Errors & Gotchas

What breaks & why

- **Asserting before stopTest()** (async not run yet).
- **Expecting full queueable cisning** in one test.
- **Missing callout mocks** for async callouts.

📏

### Limits

Governor & platform limits

- Forces queued async work (future, Queueable, Batch) to execute synchronously.
- Only one batch `execute()` chunk runs, regardless of data volume.
- Cisned Queueables do not cisn further inside a test.
- Limits revert to the pre-`startTest()` counters afterwards.

✅

### Best Option (with reason)

Decision + trade-off
**Best option: enqueue async inside start/stopTest and assert after stopTest; structure async so each hop is independently testable.**
**Why:** stopTest deterministically flushes async, enabling reliable result assertions.
🏢

### Real-World Use Case

Project scenario
**Scenario:** A Queueable updates records, but a test asserting the update fails because it checks too early.
**Solution:** Enqueue the Queueable between start/stopTest and move the assertions **after** `Test.stopTest()` — the async runs at stopTest, so the re-queried records now reflect the update.

---

### Master View — Code Coverage

Code Coverage is a **deployment gate**, not a complete measure of quality. A 75% aggregate is required, but strong tests use meaningful assertions to verify positive, negative, bulk, permission, and edge paths.

## Code Coverage

The percentage of Apex lines executed by tests — a deploy requirement, not a quality metric.
🌱

### Simple

Beginner - plain language
**Code coverage** is the percent of your Apex lines run by tests. You need **75% org-wide** to deploy to production (and every trigger must have *some* coverage).
📘

### Advanced

Working developer depth
Coverage is measured across all tests; production deploys require **≥75%** aggregate and each trigger >0%. But coverage only proves lines *ran*, not that behavior is *correct* — high coverage with weak assertions is misleading. Aim higher (often 85%+ in practice) **and** assert meaningfully across positive/negative/bulk paths.
🧠

### Super Advanced

Senior / Architect depth
Architect depth: coverage is a gate, quality comes from assertions. (1) **75% is a floor** — a deployment rule, not a goal; teams target higher and, more importantly, ensure tests **assert behavior** (a line can be covered with zero verification). (2) **Don't game it** — tests that execute code without assertions inflate coverage while catching nothing; review for assertion quality, not just the number. (3) **What to cover** — prioritize branches/edge cases (positive, negative, bulk 200, permissions) over chasing the last few percent of trivial getters. (4) **Per-class visibility** — check coverage per class to find untested logic, not just the org aggregate. (5) **CI gates** — enforce coverage and run all tests on PRs; packaging requires per-package thresholds. (6) **Refactor-friendly** — assert behavior, not implementation, so coverage stays meaningful through refactors. (7) **Triggers** — must have coverage; test all contexts. Architects treat coverage as a necessary gate but measure real quality by behavioral assertions and branch coverage of critical logic.
🎯

### Interview

Q&A + how to answer
**Q: Is high code coverage a guarantee of code quality?**
A: **No.** Coverage only shows which lines *executed*, not that behavior is correct — code can be 100% covered with no meaningful assertions. Quality comes from **behavioral assertions** across positive, negative, bulk, and permission paths; 75% is just the deploy floor.
🐞

### Errors & Gotchas

What breaks & why

- **Gaming coverage** with assertion-free tests.
- **Below 75%** blocking deployment.
- **Trigger with 0% coverage.**

📏

### Limits

Governor & platform limits

- 75% org-wide required for production deployment; every trigger needs at least some coverage.
- Individual classes have no minimum, but the org-wide figure must pass.
- Coverage figures can be stale - run all tests before a release-critical deploy.
- Coverage counts executed lines only; assertions are not measured at all.

✅

### Best Option (with reason)

Decision + trade-off
**Best option: exceed the 75% floor while prioritizing meaningful behavioral assertions and branch/edge/bulk coverage of critical logic, enforced in CI.**
**Why:** Coverage gates deployment, but only assertions verify correctness.
🏢

### Real-World Use Case

Project scenario
**Scenario:** An org sits at 92% coverage yet ships frequent regressions.

## **Solution:** Audit the tests for assertions — add behavioral checks (re-queried state, error paths, bulk) to the high-coverage-but-unasserted classes, so coverage

# Final Revision Map

## Architect Mindset

```
Isolation
+
Deterministic Test Data
+
Behavioral Assertions
+
Bulk Testing
+
Permission Testing
+
Async Testing
+
Meaningful Coverage
=
Production-grade Apex Test Strategy
```

### One-line Revision

- **Test Class:** quality gate.
- **Test Method:** one behavior.
- **Assertions:** prove correctness.
- **Test Data:** isolated and realistic.
- **@testSetup:** seached baseline + isolation.
- **SeeAllData:** avoid org-data dependency.
- **startTest:** fresh limits + code boundary.
- **stopTest:** async execution + assertion point.
- **Coverage:** deployment floor, not quality proof.