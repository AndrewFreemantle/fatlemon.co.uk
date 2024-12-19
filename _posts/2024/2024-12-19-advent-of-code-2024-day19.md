---
title: 'AoC# 2024 - Day 19: Linen Layout'
permalink: /2024/12/advent-of-code-2024-day19/
excerpt: 'Advent of Code 2024 in C# - Day 19 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day19/
tags:
  - Advent of Code
  - 2024
  - Day 19
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

I was hoping for a straightforward [Day 19](https://adventofcode.com/2024/day/19) as I'd been out the night before. It was a struggle to get up at 4:45am, so with less sleep and a slightly fuzzy head I think I did OK...

### Part 1 ⭐️
This looked like a simple(!) recursion / dynamic programming problem and I had an implementation working pretty quickly for the example puzzle input, taking milliseconds. Set it away with the full puzzle input and when it hadn't returned after 5 minutes I went back to bed for 3 hours.

Came back to a message I haven't seen before:

![]({{ site.imageurl }}2024/advent-of-code/2024-day19-errormessage.png)
<figcaption>Poor Rider - I guess my <a href="https://en.wikipedia.org/wiki/Integrated_development_environment">IDE</a> was as tired as I was</figcaption>

Feeling better after the rest, and wondering if my approach was suitable, I headed over the Reddit forum for a hint. The first meme was about caching and two or three others called out [memoisation](https://en.wikipedia.org/wiki/Memoization). I will admit I was a little skeptical that just caching the function calls would be that much faster, but after finding a suitable [NuGet function-cachine package](https://github.com/Gaweph/Memoizer), I was surprised it worked, and in about ~100ms too!

### Part 2 ⭐️
A small tweak to the now `[Cache]`'d Part 1 function to return a number after trying all combinations rather than returning success on the first working combination was all it took.

Well, almost. I'd used an `int` and that wasn't big enough as the first result was negative 🤭

Swapped the `int`s for `long`s and my answer was *much* bigger, but still not big enough aparently!

I tweaked the matching logic in my recursive function from this:[^1]
``` c#
[Cache]
private long IsDesignPossiblePart2(string design)
{
    // design is the remainder of the design to check
    long result = 0;

    foreach (var towel in _towels)
    {
        if (design.Equals(towel))
            return result + 1;

        if (design.StartsWith(towel))
            result += IsDesignPossible(design.Substring(towel.Length));
    }

    return result;
}
```
to this:
``` c#
[Cache]
private long IsDesignPossiblePart2(string design)
{
    // design is still the remainder of the design to check
    if (string.IsNullOrEmpty(design))
        return 1;

    long result = 0;
    foreach (var towel in _towels)
    {
        if (design.StartsWith(towel))
            result += IsDesignPossiblePart2(design.Substring(towel.Length));
    }

    return result;
}
```

... and found an additional 270,956,484,210,700 combinations 😮

---

[^1]: In writing up this post, I've spotted the bug in my first Part 2 function attempt: it returns too early; even if the `design.Equals(towel)`, there could be subsequent towels that the design also equals.
