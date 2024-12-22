---
title: 'AoC# 2024 - Day 22: Monkey Market'
permalink: /2024/12/advent-of-code-2024-day22/
excerpt: 'Advent of Code 2024 in C# - Day 22 ⭐️'
redirect_from:
  - /aoc-2024-day22/
tags:
  - Advent of Code
  - 2024
  - Day 22
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

[Day 22](https://adventofcode.com/2024/day/22) marks a Personal Best: 39⭐️'s and counting...

### Part 1 ⭐️
Straightforward enough. Seeing that each step was followed by mix and prune operations, I created a `MixAndPrune()` function and was pleasantly surprised given the 2,000 iterations over the 1,730 input values that it ran in ~100ms.

### Part 2 ❌
Again, this seemed straightforward enough to follow and implement, but after correctly solving the example puzzle input I couldn't do the same for the actual puzzle input. The message says I'm close, but my approach took ~18min to compute (using the parallel extensions too) so there are two problems:
1. I've missed something in my implementation
2. There's clearly a faster way to solve today's Part 2
