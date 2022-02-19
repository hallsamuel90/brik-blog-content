---
title: "TDD Part 1: Why Should I?"
date: "2020-12-18"
---

In the first part of this series I want to share my personal experience with test-driven development(TDD), how I got there, and how it makes the development process much simpler with an end result of higher quality code.

# Bad Tests 👎
I once worked on a project using a micro-services architecture that had a contractual obligation for 80% code coverage. Before this most of us had not written unit tests, let alone practiced TDD.

In the beginning, I developed the way I normally did at the time, that is slam out a bunch of code, run the application, wonder why it wasn't working, run the debugger, fix the bug, rinse and repeat(also known as bug driven development). 

I would write the unit tests after I finished the code. Sometimes I would even create another task to do this work under after I had already committed the production code(no bueno). It was deeply frustrating and oftentimes took longer to write the tests than it did the feature (the tests were also garbage).

# Starting to Write Better Tests 🌱
As the system grew and more services were added, it became increasingly more difficult to develop this way. Running the entire system in a local environment was not always feasible and the feedback loop was far too long to do anything efficiently.

As my frustration grew I started looking at how to write better unit tests and how to write testable code. I read [Clean Code](https://amzn.to/34kTzQ9) which is closely related but not quite directed at TDD, discovered the [SOLID design principles](https://www.youtube.com/watch?v=rtmFCcjEgEw&list=PLGbeNLNjS1rHkib_aXeIUrV7YhSbQPz2g&index=36&ab_channel=LaraconEU), and absorbed as much as I could from the internet. 

So I started writing a little bit of code and then I would write the unit tests at the same time. This kept my code clean, and I could develop entire features without ever firing up the application. The tests were all of the feedback I needed and then I could verify that the system was running as expected less frequently. The issues that did occur where usually plumbing related or not understanding the problem correctly.

# Going full TDD 💡 
I then came across this [talk by Elliotte Rusty Harold](https://www.youtube.com/watch?v=fr1E9aVnBxw&ab_channel=Devoxx), and one of the audience members basically described how I developed and asked why he should write his tests first. Rusty's response was to the effect of 'so you are writing a little code, writing a little test, writing a little code, etc... I'm just asking that you cut out the first step and write a little test, write a little code.' 

A light bulb went off and that's what I started doing. Before I wrote any code, I wrote a test first. Suddenly writing complex features became not only easy but fun! I knew what the system had to do, so I would write down a list of tests, and one by one I would write the test, then write the code that made it pass. This let me build up the feature in tiny baby steps and I got immediate feedback, kept the code simple and clean, and gave me confidence in the code that I had not experienced in any other way. 

# Conclusion 🔚
There are many other benefits to TDD documented elsewhere, but I wanted to explain my evolution of how I got there and how it affected me practically. 

I would say that writing the tests as you write the code gives you about 80% of the benefits but going full TDD will take all unneeded complexity out of the design and leaves you with higher quality than you might have had otherwise. 

The bottom line is, is that it makes coding easier and with the added bonus that your code will be by default high quality.

Throughout the rest of this series, I'll be going over the basic anatomy of tests, test doubles (mocks), how to write tests for legacy code, and how to practice TDD.

I hope you'll join me!