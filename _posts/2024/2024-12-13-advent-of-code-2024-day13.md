---
title: 'AoC# 2024 - Day 13: Claw Contraption'
permalink: /2024/12/advent-of-code-2024-day13/
excerpt: 'Advent of Code 2024 in C# - Day 13 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day13/
tags:
  - Advent of Code
  - 2024
  - Day 13
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

[Day 13](https://adventofcode.com/2024/day/12) finds us in worship of the claw!

<img width="50%" src="https://wallpapers.com/images/hd/toy-story-alien-with-the-claw-wbctqt7tw6xrggaf.jpg"/>

### Part 1 ⭐️
Parsing the input into `ClawMachine` objects was as enjoyable as solving the first puzzle 🙂

Given the hint:

> You estimate that each button would need to be pressed **no more than 100 times** to win a prize.

I was sure there was a better way than brute force looping from 0 to 100 for each button; recording the presses and token scores when a prize is won; and returning the smallest prize-winning number; but at ~100ms the brute force was sufficient if inneficient 🙃

### Part 2 ⭐️
> Unfortunately, it will take **many more than 100 presses** [to win the prize]

Moving the ~~cheese~~ prize so that brute force isn't viable for Part 2 is a staple of these Advent of Code challenges 🙂

Not sure of a suitable approach to the problem, I explained today's problem to Julie with the [hope I might come up with an answer myself](https://en.wikipedia.org/wiki/Rubber_duck_debugging), but she also loves a challenge (especially a maths one) and within 10 minutes had written out a solution by hand. It didn't take long to code it up and we had the second star[^1].

After explaining the solution to me, I spent an hour learning how to solve simultaneous equations 🤓

[^1]: Yes, I fully conceed that Julie won today's second star ⭐️
