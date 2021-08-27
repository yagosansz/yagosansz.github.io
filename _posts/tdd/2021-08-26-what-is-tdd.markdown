---
layout: post
title:  "What is Test-Drive Development (TDD)?"
date:   2021-08-26 03:31:00 -0400
categories: tdd ruby rails
---

TDD (Test Driven Development) is a software development process where your code is driven by tests. It follows the red-green refactor cycle, which has the following standards:

1. Write test for a small chunk of your functionality
2. Watch it fail (<span style="color: red">red</span>)
3. Add code so the test will pass (<span style="color: green">green</span>)
4. Repeat 1 to 3, until your functionality is complete

Time spent writing tests for your application is always time well spent, because it gives you confidence that your code works like it should.

With a full suite of regression tests to back you up, you will know that both you and your team will be safe when refactoring or adding new features.