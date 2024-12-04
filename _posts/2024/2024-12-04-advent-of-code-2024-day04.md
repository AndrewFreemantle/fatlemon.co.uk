---
title: 'Advent of Code 2024 - Day 04'
permalink: /2024/01/advent-of-code-2024-day04/
excerpt: 'Advent of Code 2024 - Day 04 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day04/
tags:
  - Advent of Code
  - 2024
  - Day 04
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

[Day 4](https://adventofcode.com/2024/day/4) sees the return of the 5am start 😜

### Part 1 ⭐️
I decided on a generic approach, passing in the word we needed to find then creating methods that looked in each of the [Cardinal and Ordinal directions](https://en.wikipedia.org/wiki/Cardinal_direction), for example:

``` c#
/// <summary>
/// X ++ && Y --
/// </summary>
private bool WordIsUpForward(string word, int x, int y)
{
    for (var i = 0; i < word.Length; i++)
    {
        // bounds check - can we keep looking in is this direction?
        if (x + i >= _input[y].Length || y - i < 0)
            return false;

        // word/character check - is this our word?
        if (_input[y - i][x + i] != word[i])
            return false;
    }

    return true;
}
```

### Part 2 ⭐️
After a little thought, I figured that finding the middle character of the word as my starting point, then re-using 2 of the ordinal direction methods in combination with reversing the word ought to work. Pleasingly, it did.
