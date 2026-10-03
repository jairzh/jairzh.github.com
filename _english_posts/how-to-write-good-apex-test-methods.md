---
title: "Apex Tests That Do More Than Meet Code Coverage Requirements"
date: 2016-05-22 15:25:29 +0800
last_modified_at: 2026-10-03 21:48:42 +0800
permalink: /en/blog/2016/05/22/how-to-write-good-apex-test-methods
translation_key: how-to-write-good-apex-test-methods
description: "Four goals, five practices, and six challenges for tests that protect real Apex behavior."
categories: ["apex"]
tags: ["apex", "test"]
---

Apex tests are part of deploying to Salesforce, but their value should extend well beyond the coverage requirement. A test that executes a line without checking its result can satisfy a metric while missing the very defect it was supposed to catch.

My original article organized useful tests around four goals, five practices, and six challenges. That structure still provides a practical checklist.

## Four goals

### 1. Test positive behavior

Start with the intended business outcome. Create valid inputs, execute the operation, and check that it completes as expected. If a method has branches, cover the meaningful successful paths rather than only the easiest route through it.

### 2. Verify the end state

Assertions are the part that makes a test useful. Check the returned value, the saved record, or the observable side effect that the requirement calls for.

```apex
System.assertEquals(
    'Closed',
    actualStatus,
    'The operation should close the request'
);
```

The assertion message should explain the expected behavior. A failure is much easier to investigate when the test tells you which promise was broken.

### 3. Test negative behavior

Try inputs that should be rejected or handled differently. A blank required value, an unsupported transition, or an unavailable related record can be just as important as the happy path.

Check the intended response: an error, a validation message, or an unchanged record. Merely catching an exception without asserting anything can hide a defect.

### 4. Test bulk behavior

Code that works for one record may fail when a batch arrives through an integration or data import. Test a representative collection—200 records is a useful trigger-batch scenario—and assert that all relevant records were processed correctly.

That helps expose SOQL or DML inside loops. It is not a guarantee that every governor-limit scenario is covered: multiple objects, repeated updates, trigger recursion, and asynchronous callers can produce different behavior.

## Five practices

### 1. Own your test data

Avoid relying on existing organization data through `SeeAllData=true`. Such dependencies can make the same test pass in one org and fail in another.

Create the data the test needs. Use `@testSetup` for shared setup where appropriate, or `Test.loadData` with a CSV static resource when fixtures are easier to express that way.

### 2. Centralize repeated setup

A test-data factory keeps common record creation in one place. If a new required field is introduced, a factory is easier to update than dozens of nearly identical setup blocks.

Keep scenario-specific choices visible in the test. A factory should reduce duplication without making the reader guess which conditions are being tested.

### 3. Use descriptive names

A class name such as `MyServiceTest` makes the relationship to the production class clear. Method names should describe behavior: `rejectsMissingAccount` is more informative than `testMethod2`.

### 4. Separate setup, execution, and verification

Create fixtures before `Test.startTest()`, execute the operation between `startTest()` and `stopTest()`, then assert the result afterward.

```apex
// Arrange: create the records and inputs for this scenario.
Test.startTest();
// Act: invoke the operation under test.
Test.stopTest();
// Assert: verify the resulting records or return value.
```

This makes the test boundary easy to understand and is important when testing supported asynchronous work.

### 5. Give each test a clear purpose

One method should not try to exercise the entire application. Separate scenarios so that a failing test points to a specific behavior. Shared setup is useful; shared ambiguity is not.

## Six common challenges

| Scenario | Useful approach |
|---|---|
| Visualforce controllers | Set a page with `Test.setCurrentPage()`, provide URL parameters, instantiate the controller, and assert navigation or state. |
| SOSL | Use `Test.setFixedSearchResults()` to control the records returned by the test. |
| Asynchronous Apex | Use the supported `startTest()` / `stopTest()` pattern and assert the resulting work. Do not assume an arbitrary chain of nested jobs all runs in the test. |
| HTTP callouts | Implement `HttpCalloutMock` and register it with `Test.setMock()`; test success and failure responses. |
| User access | Use `System.runAs()` for user and sharing scenarios, then explicitly test the security behavior implemented by your production code. |
| Private members | Use `@TestVisible` when necessary, while favoring observable public behavior when that is sufficient. |

User context, record sharing, object access, and field access are different concerns. `runAs()` alone should not be treated as proof that CRUD and field permissions are enforced. Queries and DML need the appropriate security behavior, such as user-mode operations, when the requirement calls for it. Salesforce explains these distinctions in [Secure Apex Classes](https://developer.salesforce.com/docs/platform/lwc/guide/apex-security).

## A test should protect a promise

Coverage answers whether code was executed. Useful tests answer whether the code kept its promise, rejected the wrong inputs, and behaved correctly under realistic workloads. Those are the tests that make the next change safer.

For the platform-specific tools mentioned here, see the [Apex Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/).

---

> Originally published on 2016-05-22. English edition revised on October 3, 2026. Updated terminology and clarified bulk testing, asynchronous execution, and permission-testing boundaries. [Read the Chinese original](/blog/2016/05/22/how-to-write-good-apex-test-methods).
