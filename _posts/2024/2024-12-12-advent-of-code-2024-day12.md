---
title: 'AoC# 2024 - Day 12: Garden Groups'
permalink: /2024/12/advent-of-code-2024-day12/
excerpt: 'Advent of Code 2024 in C# - Day 12 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day12/
tags:
  - Advent of Code
  - 2024
  - Day 12
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

[Day 12](https://adventofcode.com/2024/day/12) finds us pricing up some garden fencing...

### Part 1 ⭐️
Pretty straightforward, though I had to look up the [algorithm for a flood fill](https://en.wikipedia.org/wiki/Flood_fill#Moving_the_recursion_into_a_data_structure) - turns out it's not too dissimilar to the [path-walking we did two days ago]({{ site.url }}/2024/12/advent-of-code-2024-day10/) 🙂

### Part 2 ⭐️
After a little refactoring so the `Garden` objects I'd created in Part 1 were also available to my Part 2 function, I just had to implement a `Garden.Sides` property...

``` c#
public string Part1()
{
    return _gardens
        .Aggregate(0, (sum, garden) => sum += (garden.Area * garden.Perimeter))
        .ToString();
}

public string Part2()
{
    return _gardens
        .Aggregate(0, (sum, garden) => sum += (garden.Area * garden.Sides))
        .ToString();
}
```

My first attempt was a little naïve - a simple distinct of the top, bottom, left and right edge locations didn't factor that the edges mightn't be contiguous. A bit more thought and I had the example correct and my puzzle input answer correct too.
