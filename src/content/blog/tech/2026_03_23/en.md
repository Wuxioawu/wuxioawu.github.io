---
title: "Types of Developer's Tests"
pubDate: 2026-03-23
description: types of developer's tests
draft: false
slugId: tech/260323
---

## Three Types of Tests

The text introduces three essential types of testing:
- **Unit Tests** → test individual components (main focus)
- **Integration Tests** → test interaction between components
- **End-to-End Tests** → test the whole system from user perspective

![](./20260323113048.png "")

## Unit Test

A **unit test** focuses on testing a single class (or the smallest testable unit of code) in isolation.  
Its primary goal is to ensure that the code behaves correctly under controlled conditions.

## Integration Tests
_Integration tests focus on the proper integration of different modules of your code, including and this is especially valuable - with code over which you have no control.

## End-to-End Tests

End-to-end tests exist to verify that your code works from the client’s point of view. They put the system as a whole to the test, mimicking the way the user would use it.

## Example

SUT: System Under Test, we understand the part of the systems being tested.
DOC: Depended On Component, is any entity that is required by an SUT to fulfill it duties.

![](./20260323114825.png "")
