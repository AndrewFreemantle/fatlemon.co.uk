---
title: How far should your unit tests go?
permalink: /2008/02/how-far-should-your-unit-tests-go/
categories:
  - Uncategorized
tags:
  - Development
  - Unit Testing
---
I don't think it would be appropriate to talk about Unit Testing without first starting with the bigger picture of Agile Software Engineering Methodologies.

I started working to the Agile methodologies, as described by [Robert C. Martin](https://uk.bookshop.org/p/books/agile-software-development-principles-patterns-and-practices-pearson-new-international-edition-robert-martin/4676174?ean=9781292025940 "Robert C. Martin - Agile Software Development: Principles, Patterns and Practices") about 18 months ago. It was hard not to see it as a 'magic bullet' that enables the production of quality software because it just made so much sense to me. Here's [Steve McConnell's view of Agile development](http://forums.construx.com/blogs/stevemcc/archive/2007/10/08/5-questions-on-agile-development.aspx "5 Questions on Agile Development - 10x Software Development"):

> *Q4: What is the future of Agile?*
>
> *Agile has largely become a synonym for "modern software practices that work" so I think the future of Agile with a capital "A" is the same as the past of Object Oriented or Structured. We rarely talk about Object Oriented programming anymore; it's just programming. Similarly, I think Agile has worked its way into the mainstream such that __within the next few years we won't talk about Agile much anymore; we'll just talk about programming__, and it will be assumed that everyone means Agile whenever that's appropriate.*

I have emphasised Steve's sentence that beautifully sums up Agile for me - 'Agile' is my definition of programming.

Changing my way of thinking from initially "where do I start coding the solution?' to "how am I going to TEST the solution?' was definitely challenging, but I still remember sitting down to start actually writing test code and wondering "how far should my unit tests go?' - meaning, if I'm testing that an Object can persist, or save its state to a database, how do I test that it's working?

My current project incorporates the excellent [Rockford Lhotka's CSLA.Net framework](href="http://www.lhotka.net/cslanet/Default.aspx "Component-based, Scalable Logical Architecture (CSLA) - Homepage"), and my first attempts at testing for persistence did so by reading the database tables to verify the data written - in the same unit test..

![Unit Testing - first attempts]({{ site.imageurl }}2008/unit_testing_1.png)

In practice, these unit test cases were well over a screen full of code which, when testing different scenario's around the edit and save logic, meant I'd created an overhead that added a quite considerable amount of time to their maintenance when features were added and changes were inevitably made to the application code being tested.

I was already thinking about separating out the unit test code into it's component parts to make handling change less painful, when I saw the title of [Carl and Richard's audio talk show "Dot Net Rocks!"](http://www.dotnetrocks.com/ ".Net Rocks! The audio talk show for .Net developers") [#312 - "Andy Leonard on Unit Testing your Database](http://www.dotnetrocks.com/default.aspx?showNum=312 ".Net Rocks - Show 312 - Andy Leonard on Unit Testing your Database"). Being a Database guy, Andy's main reason for unit testing the Database is that we developers sometimes forget that [we write code there](http://en.wikipedia.org/wiki/Stored_procedure "Wikipedia - Stored Procedure"), and that **our Database code is as much a part of the application** - but I think his rationale also solves the maintenance issue with my single, large unit tests..

![Unit Testing - separating out the database layer]({{ site.imageurl }}2008/unit_testing_2.png)

**By separating out the unit tests to check the Database layer, it massively simplifies the readability and maintenance of the application layer unit tests.**

Moreover, if you locate the Database layer unit tests in their own library, then they can be executed before the application layer unit tests so they can halt further testing as it's highly likely that failure at the lower database level will manifest itself higher up. This will ultimately save us time locating and fixing failing code, as well as saving time modifying existing unit tests when changes occur.
