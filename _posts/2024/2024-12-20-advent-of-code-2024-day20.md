---
title: 'AoC# 2024 - Day 20: Race Condition'
permalink: /2024/12/advent-of-code-2024-day20/
excerpt: 'Advent of Code 2024 in C# - Day 20 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day20/
tags:
  - Advent of Code
  - 2024
  - Day 20
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

[Day 20](https://adventofcode.com/2024/day/20) and this year is shaping up to be the year of the maze!

### Part 1 ⭐️
Well, the first thing I needed was to solve the maze. This time I thought I'd try [A* search algorithm](https://en.wikipedia.org/wiki/A*_search_algorithm), adding the `cameFrom` array return so I had the route travelled.

Finding cheats through the maze... my first thought was to re-use my `GetNeighbours(Tile current)` function and for each neighbouring wall tile, check the next tile in the same direction and if *that tile* was on the route (and had a route index > than the current tile, i.e. forwards through the route rather than backwards), work out the route saving using the indexes of the tiles in the route.

This worked well and I got the first star 🤓

### Part 2 ⭐️
Now we have to check *up to 20* cheating spaces?!

Clearly Part 1's approach wasn't going to work, so after a bit of thought and some breakfast I realised we can just use the X,Y difference between all of the points along the route, then the saving is as in Part 1 (the indexes).

And if I passed in the maximum distance into this `CheatScoreBetweenTiles()` function, it'd work for Part 1 too.

``` c#
public string Part2()
{
    var routeScore = _route.Count;  // _route is a List<Tile> that is the route
                                    // through the input, using the A* solving algorithm

    var cheatScores = new List<int>();
    for (int i = 0; i < _route.Count; i++)
    {
        for (int j = i; j < _route.Count; j++)
        {
            if (i == j)
                continue;

            var cheatScore = CheatScoreBetweenTiles(i, j, 20);
            if (cheatScore <= routeScore - _cheatSaving)
                cheatScores.Add(cheatScore);
        }
    }

    return cheatScores.Count.ToString();
}
```

It gave the same correct answer for Part 1, but wasn't correct for Part 2... about an hour of debugging later I'd found the issue... my route didn't contain the end tile!

Adding it and re-running gave me the second star and I'm now [on par with last year]({{ site.url }}/2024/01/advent-of-code-2023/) and my Personal Best 💪
