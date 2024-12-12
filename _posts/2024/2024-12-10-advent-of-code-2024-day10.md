---
title: 'AoC# 2024 - Day 10: Hoof It'
permalink: /2024/12/advent-of-code-2024-day10/
excerpt: 'Advent of Code 2024 in C# - Day 10 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day10/
tags:
  - Advent of Code
  - 2024
  - Day 10
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

[Day 10](https://adventofcode.com/2024/day/10) looked like a [maze-type solving](https://en.wikipedia.org/wiki/Maze-solving_algorithm) problem: we need some code to walk through it...

### Part 1 ⭐️
After a brief refresher of maze solving and then [tree traversing algorithms](https://en.wikipedia.org/wiki/Tree_traversal), I realised the puzzle input is a tree: we have a starting node, and any number of routes or dead-ends to an ending node.

I thought a [breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search) should do the trick, and having built a list of all possible trails it was straightforward to get the answer:

``` c#
// trailheads is a list of all complete (0-9) trails from a single starting point
result += trailheads.DistinctBy(t => t.End).Count();
```

### Part 2 ⭐️
It's really satisfying when you just need a tweak to Part 1 to get the answer to Part 2:

``` c#
// trailheads is the same list of all complete (0-9) trails from a single starting point
result += trailheads
    .AggregateBy(t => t.End, 0, (sum, t) => sum + 1)
    .Sum(agg => agg.Value);
```

Lovely 🥹
