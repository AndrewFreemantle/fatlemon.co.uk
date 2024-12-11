---
title: 'Advent of Code 2024 - Day 11'
permalink: /2024/12/advent-of-code-2024-day11/
excerpt: 'Advent of Code 2024 - Day 11 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day11/
tags:
  - Advent of Code
  - 2024
  - Day 11
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

As I've attempted these challenges for a [few]({{ site.url }}/2022/02/advent-of-code-2021) [years]({{ site.url }}/2022/02/advent-of-code-2022) [now]({{ site.url }}/2022/02/advent-of-code-2023), I like to say that it teaches you how to recognise exponentially expensive problems (that is, those that brute force can't solve). [Day 11](https://adventofcode.com/2024/day/11) is one of those if we tried to maintain a string of number changing pebbles...

### Part 1 ⭐️
A simple list of the pebble numbers worked fine, albeit taking about 1 second to compute.

### Part 2 ⭐️
I wan't sure why the instruction "*[the pebble] **order is preserved***" was relevant so I thought it might be important for Part 2, but instead we were asked to loop over the other-worldly pebble mutation 3&times;.

I was, however, pretty sure my Part 1 solution wouldn't complete in any sensible amount of time[^1], so I thought a Dictionary of pebble numbers and counts *might* work. The result was correct and I'm still left wondering why the order was important[^2]... can't say I've seen a [red herring](https://en.wikipedia.org/wiki/Red_herring) in these challenges before 🤷‍♂️

---

[^1]: Certainly not longer than it took to implement the Dictionary method 😉
[^2]: Indeed - my non-order preserving Dictionary method gives the same result for Part 1, and in 4ms compared to 1sec.
