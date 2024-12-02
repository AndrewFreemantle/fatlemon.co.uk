---
title: 'Advent of Code 2024 - Day 01'
permalink: /2024/01/advent-of-code-2024-day01/
excerpt: '▶️ Advent of Code 2024 - Day 01 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day01/
tags:
  - Advent of Code
  - 2024
  - Day 01
  - dotnet
  - YouTube
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

Given I found [watching other developer's attempts](https://www.youtube.com/@jonathanpaulson5053/videos) helpful [last year]({{ site.url }}/2024/01/advent-of-code-2023/), I decided I'd try recording my own this year:

<iframe width="1280" height="720" src="https://www.youtube.com/embed/bSgjxKdqmv8" title="Advent of Code 2024 - Day 01 - C#" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

[The 1st day](https://adventofcode.com/2024/day/1) is usually straightforward, and gave me a chance to shakedown my recording, exporting and uploading process.

### Part 1 ⭐️
The only gotcha I could think of here was the subtraction of a larger number might give a negative value. I didn't check the input for this situation as taking the absolute of the result would mitigate it: `Math.Abs(smaller - larger)`

### Part 2 ⭐️
I thought there might have been a gotcha where duplicate left-hand values might need to be excluded, but the example explanation covered this situation and included them.
