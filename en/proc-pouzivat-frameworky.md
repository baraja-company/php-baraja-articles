Why and how to use frameworks and libraries
===========================================

> id: '5ea6cdde-f67c-4f3f-8871-37b27ee8e23a'
> slug:
> 	cs: proc-pouzivat-frameworky
> 	en: why-and-how-to-use-frameworks-and-libraries
> 
> publicationDate: '2020-02-16 22:13:46'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '0926b7ec4f59904c008388431ff22eb5'

There's a famous joke that programmers start using frameworks only when they write their own and find that it goes nowhere. The funny thing about this is that it's true. I've experienced it myself. Twice, even.

However, the main page of <a href="https://nette.org">Nette</a> says:

> Real programmers **don't use frameworks**. They write web applications via the command line straight to the server. This is a tribute to them. For the rest of us, Nette will make our jobs a lot easier and more enjoyable.

The role of frameworks in application development
-----------------------------------

I'm sure you know this yourself:

You get an idea for a great project, so you start writing greenfield code directly in pure PHP... in a few hours you find that much of the work is still repetitive and you're solving basic system problems. Like how to connect to a database, how to render a form, how to validate data, how to send an email, and much more.

You'll probably write your own functions to call for these tasks. If you're writing several projects at once, you'll start sharing these functions between projects in one big messy file, and you'll continually tweak them as you need them.

And when the functions are at a certain stage of development that you start building a new project on top of them each time and develop straight away, then you can talk about having written your own framework, or at least a library.

What is and what does a framework
-------------------------

As we have already shown with a practical example from life, a framework is a collection of many functions (but ideally classes and objects) that save the programmer's work. When developing, he doesn't have to think about how to connect to a database, but he knows that it is already programmed somewhere and "it just works".

Finished frameworks are thus the collected smarts of hundreds of people who have developed thousands of applications over a long period of time to debug one solution and one ecosystem to tackle new projects.

With frameworks, you also get a set of **best practices**, which are ways to solve problems without having to think about it. If you run into a problem, someone has probably solved it before you, and it's always better to find a solution in the documentation than to have to work it out in a complicated way yourself.

Abstract programming and the encapsulation principle
---------------------------------------------

Well-designed frameworks like Nette allow you to program at a **very high level of abstraction**.

This way, the programmer (the framework user) doesn't have to understand what's going on internally and exactly how the components work internally, which saves a lot of time and effort. He can focus on solving the real problems of his application and get results very quickly.

David Grudl himself (the author of the Nette framework) once said that he designed Nette to be able to program a website for a client in a night, when he comes home late from the pub and has to launch the website in the morning. This is mainly due to the developer being completely removed from the technical stuff.

In order for this to work and for the developer to just use the finished framework as a whole, it is very important to use the <a href="/encapsulation">encapsulation principle</a> correctly.
