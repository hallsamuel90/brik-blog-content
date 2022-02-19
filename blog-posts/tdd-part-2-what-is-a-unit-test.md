---
title: "TDD Part 2: What is a Unit Test?"
date: "2021-01-22"
---

# Introduction
In [part 1](https://dev.to/hallsamuel90/tdd-part-1-why-should-i-4a0e) of the series, I shared my personal experience with TDD and how my practice with it has evolved. 

Before we dive too deep into it let's first answer what is a test, how to set up a test, and what defines some good characteristics of a unit test.

#What is a Unit Test? 🤔
What is a unit test? For this, I like Elliotte Rusty Harrold's definition:

 >"A Unit Test is a piece of code that verifies that a known, fixed input produces a known fixed output."

Okay now that we got that out of the way, what the hell do we do now!? I think this is probably best explained by the commonly known acronym AAA - no not the people you call when your car breaks down on the side of the road, what we are talking about here is Arrange Act Assert aka Given When Then.

First, we need to arrange our known input. Then we feed that input into our method under test. Finally, we verify that the code did what we expected. Here are a couple of example tests of a calculator that takes an input of comma-separated strings.

```typescript
it('return 0 if the input is empty', () => {
  const emptyInput = ''; // arrange

  const result = calculator.add(emptyInput); // act

  assert.equal(result, 0) //assert
});
```

This is simple enough that we can do this in one-line but under more complex circumstances (or if you are new to this) it is very helpful to break it out in this manner.

```typescript
it('return input if there is only one number', () => {
  assert.equal(calculator.add('2'), 2)
});

it('return sum of two numbers', () => {
  assert.equal(calculator.add('1,2'), 3)
});
```

# FIRST Principles 🥇
Although this is a very simple example, let's take a second to make a few observations about these tests. 
1. They are fast, there are no external dependencies. I'm not reading from the file system or database (we will cover this soon). This is good because I want to keep my feedback loop as tight as possible and I can keep running the tests between every change.
2. They are isolated. I am only testing one thing in each test. If one of those breaks, I know exactly what broke and where to look.
3. They are repeatable. It doesn't matter which order I run these in, what environment, they will always produce the same result.
4. They are self-validating. I do not have to check manually if one of these failed. I will know on the run if something broke.
5. They are thorough (at least somewhat here). Ideally you want to test your edge cases, error cases, and happy path. You need to know how you are going to handle everything.

These are known as the FIRST principles of unit testing. Sometimes I feel that these are almost self-evident once you start writing tests, but I think it bears repeating and bolsters the definition of a unit test.

# Wrapping Up
Alright, that's lightweight stuff, what about functions and methods that don't return anything? What about if I need to talk to a database? You still haven't shown how to actually do test-driven development. 

Whoah.... cool the jets we will get there I promise. 

In the next article, we will start actually doing some TDD with an awesome way to learn and practice TDD - Code Katas.

Thank you for reading and stay tuned! 🙏