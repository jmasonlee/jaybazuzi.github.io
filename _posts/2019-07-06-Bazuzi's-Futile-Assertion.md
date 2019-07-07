---
layout: post
title: Bazuzi's Futile Assertion
---

In conversations about refactoring, it's common to hear something like "of course, you must have good tests to catch any mistakes you make while refactoring". I've written before that tests are [neither necessary nor sufficient] to refactor safely; here's a slightly different take:

# Bazuzi's Futile Assertion

- If you didn't strictly follow the Three Rules of TDD, your tests are not sufficient.

- Once code is in production, your tests are not sufficient, even if you followed the 3 rules.

# Understanding the assertion

## Sufficient

Tests are "sufficient" if I can make a change and tests will tell me if I break something that matters.

## The Three Rules

The Three Rules guarantee that you have a test for everything you want your code to do: if you want your code to do something, you write a test for that thing, then make it pass with minimal new code, therefore every behavior you have added to your code is verified. 

## Production

Once someone else uses your code, they'll use it in a way you didn't anticipate, and therefore don't have tests for. But now it's a feature so you better not break it.

# The Name

Three ways to understand it:

1. The futility of tests as a safety net for refactoring.

2. One way of understanding why tests are insufficient for refactoring is that they only test what they _*assert*_, which is a finite, while the set of details that are not asserted is effectively infinite. It is futile to try to assert all the things that matter.

3. I assert that this is true, but I suspect it is futile that this idea will be broadly accepted.
