---
title: 'Advent of Code 2024 - Day 9'
permalink: /2024/12/advent-of-code-2024-day09/
excerpt: 'Advent of Code 2024 - Day 9 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day09/
tags:
  - Advent of Code
  - 2024
  - Day 9
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

It took a change of approach to solve [Day 9](https://adventofcode.com/2024/day/8)'s part 2, but I got there 🙂

### Part 1 ⭐️
Time travelling with the engineers and reading the problem statement made me think of [Quantum Leap](https://en.wikipedia.org/wiki/Quantum_Leap_(1989_TV_series)) this morning...

![](https://c.tenor.com/5e-uwFCsfosAAAAM/quantum-leap-oh-boy.gif)

Took a deep breath, started breaking down the problem into functions and before I knew it I had a working solution. It took 6 secs to compute mind!

### Part 2 ⭐️
Took another deep breath (!), and tweaked my Part 1. When the answer I got was too low for the example input I re-read the instructions and realised I'd completely misunderstood them 😔

After fixing my understanding and my code, I ran it against the full-fat input and when it hadn't resolved in 20 seconds I went for breakfast. 20 minutes later it still hadn't finished but I'd been thinking of using and manipulating a collection of `Gap` objects rather than repeatedly looping over the collection of `File` objects looking for gaps.

Another 20 minutes or so and with one wrong attempt (due to a susequently easily spotted bug), I had the second star. Compute was less than 150 ms too 🤓
