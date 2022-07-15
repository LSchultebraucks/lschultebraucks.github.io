---
layout: post
title: "End the day with a failing unit test"
author: "Lasse Schultebraucks"
categories:  [TDD]
comments: true
---

In the last days I tried to end the day with a failing unit test. This may sound wrong at first, but in the end it offers many advantages, which I would like to explain in the following.

I work according to Test Driven Development, a method that should be familiar to every professional software developer. With TDD, the actual programming is done in iterations:

1. You write a test that describes the new behavior. This test (probably) does not satisfy the previous implementation, so the test fails.
2. You write the minimal implementation to run this test and all previous tests correctly.
3. You refactor according to the common method: minimal steps to make the code cleaner, test run to check if all tests pass successfully.

It seems plausible that a final result that can be checked in and is deployable cannot come about after the first step. The build pipelines would fail because there is a test that does not pass successfully.

But the idea behind "writing the test first" is also among other things to describe the later functionality of the software. When you write the test you analyse the requirement of the software you will write later and figure out the details. I usually find it more difficult to write a good unit test than the later implementation to make the written unit test run successfully. This is not always true, but in my perception most of the time.

It obviously feels very good at the end of the day to complete a task, run all tests successfully, check in the code and finish the feature you were working on. But if this is not the case, then it can be a good idea to write a unit test to prepare for the next morning. This can be compared to putting out the fresh laundry the night before you go to bed or setting the table for breakfast the next day. All these activities can make the start of the day easier and more pleasant. The same is with the unit test that fails, which you write at the end of the day to prepare for the next morning.
