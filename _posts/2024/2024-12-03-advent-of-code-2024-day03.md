---
title: 'Advent of Code 2024 - Day 3'
permalink: /2024/12/advent-of-code-2024-day03/
excerpt: 'Advent of Code 2024 - Day 3 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day03/
tags:
  - Advent of Code
  - 2024
  - Day 3
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

A delayed start to my attempt at [Day 3](https://adventofcode.com/2024/day/3) because I was catching up on much needed rest 😴

### Dude, where's the video?
I'm glad I tried making [the daily attempt videos this year](https://www.youtube.com/watch?v=SIs_JJswiEg&list=PLzLx2aDA4X7TLjqZ9zYwfJ6O--Bhz_RP0), but I've found that editing them takes a long time, and when I thought of my own preference as a viewer, I either want a [speed-run style](https://en.wikipedia.org/wiki/Speedrunning) (like [Jonathan's](https://www.youtube.com/@jonathanpaulson5053/videos)), or a longer [walk-through explanation](https://en.wikipedia.org/wiki/Video_game_walkthrough).

Of these, I feel I'm better skilled for creating the latter 😉


### Part 1 ⭐️
This looked to be best solved with regular expressions which I don't use often, so it took a little tweaking but was solved pretty quickly. The easy to remember [https://ihateregex.io](https://ihateregex.io) helped with its visualisation 😄

### Part 2 ⭐️
Initially I'd thought I could advance through the input with a current index, passing the substring between `don't()` and `do()` to the Part 1 RegEx, but this approach didn't work.

I then wondered about extending the RegEx to include the commands and then looping through all of the matches which would be in order - this worked a treat.

I think my solution could be improved by using RegEx's capture groups to pull out the `x` and `y` values.
