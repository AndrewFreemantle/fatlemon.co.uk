---
title: 'Advent of Code 2024 - Day 8'
permalink: /2024/12/advent-of-code-2024-day08/
excerpt: 'Advent of Code 2024 - Day 8 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day08/
tags:
  - Advent of Code
  - 2024
  - Day 8
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

I'm happy with my [Day 8](https://adventofcode.com/2024/day/8) solution 🙂

### Part 1 ⭐️
I started by creating a `Dictionary<char, List<Point>>` to group all of the antennae by type (`char`), then looped over each type in turn finding pairs and extrapolating the next location by applying the difference of their coordinates. A quick check that the new coordinates were within the map bounds and I had the answer.

### Part 2 ⭐️
Extending the difference of coordinates repeatedly until they're outside the map bounds called for a `while (IsWithinMap(potentialAntinode)) { ... }` loop.


### Calendar...
I love how the calendar [ASCII art](https://en.wikipedia.org/wiki/ASCII_art) follows the narative of the puzzle journey, could this year's be a milestone celebration... 🎂❓

![Advent of Code calendar for 2024, complete up to Day 8]({{ site.imageurl }}2024/advent-of-code/2024-calendar-day08.png)
