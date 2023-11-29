---
title: Writing useful comments
permalink: /2008/01/writing-useful-comments/
tags:
  - Comments
  - Development
---

It was drummed into me throughout my software engineering training years - the essential internal documentation of software - [the comment](http://en.wikipedia.org/wiki/Comment_%28computer_programming%29 "Wikipedia - Comment (Computer Programming)").

After a few years actually engineering software, I've found that I can categorise most comments into 4 basic types:

  1. The obsolete code block, often with an accompanying note about why it was removed or replaced
  2. The overly-verbose or overly-descriptive, highlighting the obvious
  3. The reference; a Bug tracking reference number or URL link to where the block of code originated
  4. The one or two-liner descriptive, concise explanation

And having done so, **I have found that the first 3 are actually detrimental to the process of maintaining or otherwise changing software**. Let's look at them in closer detail to find out why..

1. If we're using any [decent form of source code version control](http://www.codinghorror.com/blog/archives/000660.html "Coding Horror - Source Control: Anything But SourceSafe"), then there is no benefit whatsoever to leaving commented out, obsolete code blocks in the software - in fact, I'd argue that when following a thread of logic through function to function, **having to skip over comments that look like code is actually detrimental to understanding why the code does what it does**. Consider that source control makes it a trivial task to compare or replace the current code with any previous checked-in version of the code, and there simply isn't a sensible reason to leave old code in place.

2. I take this to be an indication that there is no accompanying documentation, or it's going to be sparse at best. If it's a block comment about why a particular feature exists then a better place for it is in some form of requirements documentation; if it's a block comment about a design decision then it would be better placed in the design documentation. **Any other documentation format easily wins over plain-text descriptions**, and designs are better illustrated, well, with illustrations.

3. I've worked out that both of the above are bad, yet I still catch myself thinking that it's somehow beneficial to the maintenance of the code if I include a Bug tracking reference number next to the code I've changed. If I'm coming back to the code because of a problem with the fix I've made, then **it's much easier to search through the source code version control check-in comments for the Bug reference number than through the source code itself** - if only because it'll be a hell of a lot faster. If I'm in the code for another reason - say I'm adding something new - finding a Bug tracking number isn't going to make me head off to the Bug System and look up it's history.. I know what the code is doing because I'm in there reading it and stepping through it. URLs serve as a similar distraction from the actual workings of the software. That said, I think URLs are useful in referencing the origin of an entire Class, algorithm or Library.

4. The most helpful comments I've found are the concise, single-line description of what's going on that help keep the reader on track - such as what we're looping through or why we're looping when we're <a title="Wikipedia - Array" href="http://en.wikipedia.org/wiki/Array" target="_blank">2 or 3 Arrays</a> deep. **Like breadcrumbs, they assist you and your maintainer in un-ravelling the mystery of your software**.

I've found that putting as much thought into the content of my comments, as I do into the content of my code, makes it much easier to maintain my software.
