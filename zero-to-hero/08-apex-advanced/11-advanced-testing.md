[Home](../index.md) / [08 · Apex Advanced](index.md) / **Advanced Testing**

# Advanced Testing

7 topics · Series 8: Apex Advanced

**Topics on this page**

- [HttpCalloutMock](#httpcalloutmock)
- [WebServiceMock](#webservicemock)
- [Stub API](#stub-api)
- [Queueable Testing](#queueable-testing)
- [Batch Testing](#batch-testing)
- [Scheduled Job Testing](#scheduled-job-testing)
- [Platform Event Testing](#platform-event-testing)

## HttpCalloutMock

*Simulating HTTP callout responses in unit tests.*

### 🌱 Simple

*Beginner - plain language*

**HttpCalloutMock** lets tests **fake HTTP responses** — real callouts aren't allowed in tests. Implement the interface's `respond(req)`, register with `Test.setMock(HttpCalloutMock.class, mock)`, then run the callout.

### 📏 Limits

*Governor & platform limits*

- Must be set with `Test.setMock()` before the callout executes.
- Only one mock is active at a time - use a router mock for multiple endpoints.
- Real callouts are never made in tests; an unmocked callout throws.
- Mocked responses still count toward callout limits in the test transaction.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## WebServiceMock

*Mocking SOAP callouts in unit tests.*

### 🌱 Simple

*Beginner - plain language*

**WebServiceMock** is the SOAP equivalent of HttpCalloutMock — it fakes responses for **WSDL-generated SOAP callouts** in tests via `Test.setMock(WebServiceMock.class, mock)`.

### 📏 Limits

*Governor & platform limits*

- Required for WSDL2Apex-generated SOAP stubs; `HttpCalloutMock` does not work for them.
- Set with `Test.setMock(WebServiceMock.class, ...)`.
- The mock must populate the generated response object by reference.
- Generated stub classes are excluded from coverage requirements but still deploy.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Stub API

*Dynamically generating mock implementations for any interface/class.*

### 🌱 Simple

*Beginner - plain language*

The **Stub API** (`Test.createStub` + `System.StubProvider`) generates a **mock object** at runtime for an interface/class — returning canned values for method calls in tests, without a real implementation.

### 📏 Limits

*Governor & platform limits*

- Works only in test context - `Test.createStub()` throws outside tests.
- Cannot stub static methods, inner classes in some cases, or system types.
- The stubbed type must be a non-final class or interface.
- Stub invocations still consume CPU in the test transaction.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Queueable Testing

*Executing and asserting Queueable jobs within tests.*

### 🌱 Simple

*Beginner - plain language*

To test **Queueable Apex**, enqueue the job between `Test.startTest()` and `Test.stopTest()` — `stopTest()` forces the async job to run synchronously, then you assert the results.

### 📏 Limits

*Governor & platform limits*

- Only the first job in a chain runs between `startTest()` and `stopTest()`.
- Guard chained enqueues with `Test.isRunningTest()` or the test fails.
- Finalizers do execute in tests and can be asserted.
- Chain depth is capped at 5 in Developer and scratch orgs.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Batch Testing

*Running and asserting Batch Apex jobs in tests.*

### 🌱 Simple

*Beginner - plain language*

To test **Batch Apex**, call `Database.executeBatch(job)` between `Test.startTest()` and `Test.stopTest()` — the batch runs at `stopTest()`, then you assert the results.

### 📏 Limits

*Governor & platform limits*

- Only one `execute()` chunk runs in a test, regardless of data volume.
- Create at most one scope worth of test data - more is never processed.
- `Database.executeBatch` must be inside `startTest()/stopTest()`.
- Concurrency and Flex Queue behaviour cannot be tested.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Scheduled Job Testing

*Testing Schedulable Apex via System.schedule in tests.*

### 🌱 Simple

*Beginner - plain language*

To test **Schedulable Apex**, call `System.schedule(name, cron, job)` inside `Test.startTest()/stopTest()` — the scheduled job's `execute` runs at `stopTest()`, then assert the results.

### 📏 Limits

*Governor & platform limits*

- `System.schedule()` inside a test runs the job at `stopTest()`, not on the cron schedule.
- Cron expression validity is checked but timing behaviour is not exercised.
- The 100-scheduled-job limit applies in the test org too.
- Scheduled jobs created in tests are rolled back.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Platform Event Testing

*Publishing and asserting Platform Event handling in tests.*

### 🌱 Simple

*Beginner - plain language*

To test **Platform Events**, publish the event inside `Test.startTest()/stopTest()` — `stopTest()` delivers it to the subscriber (e.g., an Apex trigger), then you assert the subscriber's effects.

### 📏 Limits

*Governor & platform limits*

- Events published in a test are delivered at `Test.stopTest()`.
- Publish After Commit events are not delivered if the test transaction is rolled back before stopTest.
- Replay Id behaviour and retention cannot be tested.
- Subscriber trigger retries are not exercised in test context.

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
