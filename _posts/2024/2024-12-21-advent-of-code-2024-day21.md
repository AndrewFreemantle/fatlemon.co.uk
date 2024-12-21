---
title: 'AoC# 2024 - Day 21: Keypad Conundrum'
permalink: /2024/12/advent-of-code-2024-day20/
excerpt: 'Advent of Code 2024 in C# - Day 21'
redirect_from:
  - /aoc-2024-day21/
tags:
  - Advent of Code
  - 2024
  - Day 21
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

[Day 21](https://adventofcode.com/2024/day/21) is a conundrum alright...

### Part 1 ❌
This looked tricky from the outset. Daunted, I started breaking down the problem into chunks and an approach quickly appeared - I figured storing the keypads as `Dictionary<char, Point>`[^1] meant quick look-ups:

``` c#
private readonly Dictionary<char, Point> _numericKeypad = new()
{
    {'7', new Point(0, 0)}, {'8', new Point(1, 0)}, {'9', new Point(2, 0)},
    {'4', new Point(0, 1)}, {'5', new Point(1, 1)}, {'6', new Point(2, 1)},
    {'1', new Point(0, 2)}, {'2', new Point(1, 2)}, {'3', new Point(2, 2)},
    /* gap at       0, 3 */ {'0', new Point(1, 3)}, {'A', new Point(2, 3)},
};

private readonly Dictionary<char, Point> _directionalKeypad = new()
{
    /* gap at       0, 0 */ {'^', new Point(1, 0)}, {'A', new Point(2, 0)},
    {'<', new Point(0, 1)}, {'v', new Point(1, 1)}, {'>', new Point(2, 1)},
};
```

Then it was a matter of working out the X,Y difference between the `char` that is the current/last pressed button and the next.

Writing that function it became clear the logic was the same for both keypads, so I refactored it to accept the keypad, then the caller passes in the numeric keypad followed by the directional one twice, like so:

``` c#
public string Part1()
{
    var result = 0;

    foreach (var code in _codes)
    {
        var keypadSequence = GetKeyPadSequence(code, _numericKeypad);

        keypadSequence = GetKeyPadSequence(keypadSequence, _directionalKeypad);

        keypadSequence = GetKeyPadSequence(keypadSequence, _directionalKeypad);

        Console.WriteLine($"{code}: {keypadSequence}");  // for debugging, keep reading...

        result += keypadSequence.Length * int.Parse(code.Replace("A", ""));
    }

    return result.ToString();
}
```

Which works for the example input but gives the wrong answer for the actual puzzle input 😕

I'm sure the issue is with how `GetKeyPadSequence()` is generating the sequences and I have checks to ensure the gaps are avoided, but after tinkering on and off I've run out of day 🙁

There's still time pick up at least one of the remaining 8 stars to get a new Personal Best this year... 🤞

### Part 2 ❌

*Can't get to Part 2 without Part 1*


---

[^1]: `Point()` is a simple [POCO](https://en.wikipedia.org/wiki/Plain_old_CLR_object) coordinate class from my [`AdventOfCode.Common`](https://github.com/AndrewFreemantle/AdventOfCode.Common) library, which is [available as a NuGet package](https://www.nuget.org/packages/AndrewFreemantle.AdventOfCode.Common)