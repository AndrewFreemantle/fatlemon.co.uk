---
title: 'Advent of Code 2024 - Day 6'
permalink: /2024/12/advent-of-code-2024-day06/
excerpt: 'Advent of Code 2024 - Day 6 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day06/
tags:
  - Advent of Code
  - 2024
  - Day 6
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

 [Day 6](https://adventofcode.com/2024/day/6) - oh yes, I love a maze!

### Part 1 ⭐️
Pretty straightforward 🙂

### Part 2 ⭐️
Seemed straightforward, but after a wrong answer and much code review I was struggling to understand why my approach to adding obstacles wasn't working. I'd thought I could place an ostacle in the path then run from there, but my answer was too high.

I'm not loving mazes anymore.

I ruminated on it during the day and after discussing the problem with Julie, she suggested building a new map with each of the visited points as an obstacle and running each to completion. A quick rewrite after [completing day 7]({{ site.url }}/aoc-2024-day07) and we're back up to full marks 🙌

Out of curiosity, I used the `Parallel.ForEach(visited, visitedPoint => {})` function, and it took 37.7652257 seconds to complete.
