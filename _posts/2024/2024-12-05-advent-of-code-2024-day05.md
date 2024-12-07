---
title: 'Advent of Code 2024 - Day 05'
permalink: /2024/01/advent-of-code-2024-day05/
excerpt: 'Advent of Code 2024 - Day 05 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day05/
tags:
  - Advent of Code
  - 2024
  - Day 05
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

Quite the explanation for [Day 5](https://adventofcode.com/2024/day/5) - there was a lot to read!

### Part 1 ⭐️
I started by breaking down the problem into small functions: separating the input into rules and data, checking a rule for validity, and another to return the value of the middle page. Then I used [Linq](https://en.wikipedia.org/wiki/Language_Integrated_Query) to combine them:

``` c#
public string Part1()
{
    return _updates
        .Where(u => IsUpdateValid(u))
        .Sum(u => MiddlePageNumber(u))
        .ToString();
}
```

### Part 2 ⭐️
Initially this seemed daunting, then I realised this looked like a [bubble sort](https://en.wikipedia.org/wiki/Bubble_sort) problem. The separate functions from Part 1 were resuable and I ended up with a correct answer pretty swiftly.

``` c#
public string Part2()
{
    return _updates
        .Where(u => !IsUpdateValid(u))
        .Sum(u => MiddlePageNumber(FixUpdate(u)))
        .ToString();
}
```
