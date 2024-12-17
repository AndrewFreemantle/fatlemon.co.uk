---
title: 'AoC# 2024 - Day 17: Chronospatial Computer'
permalink: /2024/12/advent-of-code-2024-day17/
excerpt: 'Advent of Code 2024 in C# - Day 17 ⭐️'
redirect_from:
  - /aoc-2024-day17/
tags:
  - Advent of Code
  - 2024
  - Day 17
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

The single-star days continue on [Day 17](https://adventofcode.com/2024/day/17)...

### Part 1 ⭐️
Very happy with my implementation as it's clear and readable, making use of a simple `Instruction` class:
``` c#
public class Instruction
{
    public int Opcode { get; set; }
    public int Operand { get; set; }
};
```

And a `switch` statement on the current `Opcode` for the Computer's workings.

### Part 2 ❌
I had full-day plans today so my time this morning was limited - I figured that if I could modify the Computer loop in Part1 to return early if it's not going well then I *might* be able to brute force an answer[^1] if I left it running all day:

``` c#
do
{
    // Computer implementation here (omitted)

    if (breakEarly && output.Count > 0 && !ProgramInput.StartsWith(string.Join(',', output)))
        return string.Join(',', output);

} while (instructionPointer < Program.Count);

return string.Join(',', output);
```

... hours later it hadn't completed so this is demonstrably **not** the right way to solve this!

---

[^1]: Rarely is brute forcing Part2 possible - [Eric Wastl](https://was.tl/) tends to make sure of it 🤓
