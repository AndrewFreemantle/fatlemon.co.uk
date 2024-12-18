---
title: 'AoC# 2024 - Day 18: RAM Run'
permalink: /2024/12/advent-of-code-2024-day18/
excerpt: 'Advent of Code 2024 in C# - Day 18 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day18/
tags:
  - Advent of Code
  - 2024
  - Day 18
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

 [Day 18](https://adventofcode.com/2024/day/18) is a return to previous form - two stars again!

### Part 1 ⭐️
For solving the shortest path I chose the [A* algorithm](https://en.wikipedia.org/wiki/A*_search_algorithm) and implemented it from the psuedocode, which worked first time 🙂

### Part 2 ⭐️
After a quick peek at the input data, some 3,450 coordinates, minus the starting position of 1,024 would mean running Part 1 a maximum of 2,426 times to find the answer if we brute forced it[^1]...

Before trying that though, I thought about doing a flood fill from the end location and all bytes dropped (i.e. the full input) - if the fill doesn't contain the start then work backwards through the input, removing a byte and restarting the flood fill from the removed byte until the start is included: the last removed byte would be the answer.

However, the brute force was worth a go as the changes needed were small; passing through the number of bytes to drop into the maze to Part 1, and Part 2 was a simple loop:

``` c#
public string Part2()
{
    // can we brute force Part1 (didn't work yesterday!)
    //  edit: works today!

    for (int i = _byteLocations.Count - 1; i >= _bytes; i--)
    {
        var result = Part1(i);
        if (result != string.Empty)
            return $"{_byteLocations[i].X},{_byteLocations[i].Y}";
    }

    return string.Empty;
}
```

It finished in 2.2 seconds 🤓

---

[^1]: Yes, I'm well aware of [yesterday's comment and failure]({{ site.url }}2024/12/advent-of-code-2024-day17/#fn:1), but Part 1 finished in 200ms, so it would take at most about ~8min to compute all permutations.
