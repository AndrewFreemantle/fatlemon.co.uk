---
title: Why are software developers so bad at estimating time?
author: Andrew Freemantle
layout: post
permalink: /2009/01/why-are-software-developers-so-bad-at-estimating-time/
tags:
  - Development
  - Estimating
  - Productivity
---
In order to answer this question, we need to understand the term "flow". From [Peopleware](http://www.amazon.co.uk/gp/product/0932633439?ie=UTF8&tag=fatl-21&linkCode=as2&camp=1634&creative=19450&creativeASIN=0932633439 "Peopleware: Productive People and Teams - Amazon.co.uk"):

> During single-minded work time, people are ideally in a state that psychologists call *flow*. Flow is a condition of deep, nearly meditative involvement. In this state, there is a gentle sense of euphoria, and one is largely unaware of the passage of time: "I began to work. I looked up, and three hours had passed."  There is no consciousness of effort; the work just seems to, well, flow. You've been in this state often, so we don't have to describe it to you.
> 
> Not all work roles require that you attain a state of flow in order to be productive, but for anyone involved in engineering, design, development, writing, or like tasks, flow is a must. These are high-momentum tasks. It's only when you're in flow that the work goes well.

To paraphrase, "It's only when you're in flow that you're at your most productive".

The quotation is taken from chapter 10, page 65. I know that because after reading those short paragraphs, I realised something that made me stop and put the book down..

**When a software developer thinks up an estimated time to complete a development task - they're thinking solely in "flow" time.**

That realisation is profound.

Sure, more experienced software developers will *then* add time to this based on their experience before giving their answer, but their first thought is based on *flow-time*, which is their best-case, uninterrupted development time.

**Why is this?**

I think it's a combination of 2 things;

  1. The question itself.. "how long would it take you to do / fix / add / change x?".  
    A developers starting point is their experience of development to date, and that development is at it's most productive in flow-time. So they're going to answer you based on them being in productive flow-time. However, this means they're starting their estimate based on an unaware assumption that they're quoting flow-time.  
    For example, if a developer responds with "it'll take me half a day", what they're really saying is "with a half-day of flow-time, I can do / fix /add / change x".
  2. If you didn't already know, [developers are optimistic](http://www.codinghorror.com/blog/archives/000284.html "Defeating optimism - CodingHorror.com"). They're optimistic about how much they can get done, and they're optimistic about how much flow-time they're going to have.

**So what is the answer?!**

To quote more from [Peopleware](http://www.amazon.co.uk/gp/product/0932633439?ie=UTF8&tag=fatl-21&linkCode=as2&camp=1634&creative=19450&creativeASIN=0932633439 "Peopleware: Productive People and Teams - Amazon.co.uk"):

> Chances are, your company's present time accounting system is based on a conventional model. It assumes that work accomplished is proportional to the number of hours put in. When workers fill out their time sheets in this scheme, they make no distinction between doing meaningful work and hours of pure frustration. So they're reporting body time rather than brain time.
> 
> ...
> 
> The phenomena of flow and immersion give us a more realistic way to model how time is applied to a development task. What matters is not the amount of time you're *present*, but the amount of time that you're *working at full potential*. An hour in flow really accomplishes something, but ten six-minute work periods sandwiched between eleven interruptions won't accomplish anything.

The answer, then, is to take the developers estimate with a pinch of salt (the more 'seasoned' the developer, the less salt required), remember that their estimate **is an [*estimate*](http://dictionary.reference.com/browse/estimate "Estimate (noun): an approximate judgment or calculation - dictionary.com")**, and use it with [evidence based scheduling](http://www.joelonsoftware.com/items/2007/10/26.html "Evidence Based Scheduling - JoelOnSoftware.com") (if you aren't already).

The next question you may be asking of course, is how do I reduce the difference between the estimate and the actual - the 'slip'.

That's a topic for another day. I'll let you know when I've finished [Peopleware](http://www.amazon.co.uk/gp/product/0932633439?ie=UTF8&tag=fatl-21&linkCode=as2&camp=1634&creative=19450&creativeASIN=0932633439 "Peopleware: Productive People and Teams - Amazon.co.uk"), or you can grab a copy for yourself  <i class="fa fa-smile-o"></i>
