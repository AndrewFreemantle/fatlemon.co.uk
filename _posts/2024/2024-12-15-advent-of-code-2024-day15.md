---
title: 'AoC# 2024 - Day 15: Warehouse Woes'
permalink: /2024/12/advent-of-code-2024-day15/
excerpt: 'Advent of Code 2024 in C# - Day 15 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day15/
tags:
  - Advent of Code
  - 2024
  - Day 15
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

[Day 15](https://adventofcode.com/2024/day/15): Warehouse Woes indeed...

### Part 1 ⭐️
My box-moving robot implementation didn't really push the boxes around so much as throw the closest box into the first available free space. It worked, though I won't guarantee their contents 📦

### Part 2 ⭐️
This took me a *lot* of time to debug. As my implementation wasn't satisfying the example input, I started adding [guard clauses](https://en.wikipedia.org/wiki/Guard_(computer_science)) which helped me narrow down where my logic errors were. Predictably they were in the pushing up or down of boxes because of the potential overlaps of the boxes being twice as wide.

Overall, I'm pretty pleased with my approach and the resulting code implementation 🤓
