---
title: 'AoC# 2024 - Day 23: LAN Party'
permalink: /2024/12/advent-of-code-2024-day23/
excerpt: 'Advent of Code 2024 in C# - Day 23 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day23/
tags:
  - Advent of Code
  - 2024
  - Day 23
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

[Day 23](https://adventofcode.com/2024/day/23) is the start of the network or graph theory puzzles which is an area of computing that I've only touched on via these puzzles. Today was a learning day 👨‍🏫

### Part 1 ⭐️
Parsing the input into a `Dictionary<string, HashSet<string>>` seemed like a good starting move, then for each of the linked computers in the LAN Party[^1] (the `HashSet<string>`), check the computers they were linked to, looking for pairs that were also linked (i.e. finding triangles of linked computers).

My implementation worked for the example puzzle input but gave too low an answer for the full-fat puzzle input. Reviewing my code I realised I wasn't comparing all pairs of linked computers... I was sure there was an elegant way to get a collection of all pairs in a list (rather than a loop within a loop), and [ChatGPT](https://chatgpt.com/) suggested the following:

``` c#
private List<Tuple<string, string>> GetPairs(HashSet<string> list)
{
    return list
        .SelectMany((x, i) => list.Skip(i + 1)
                                  .Select(y => new Tuple<string, string>(x, y)))
        .ToList();
}
```

It's very likely that it's loops within loops underneath `.SelectMany()` anyway[^2], but this reads clearly to me and more importantly, it worked!

### Part 2 ⭐️
When I first [started Advent of Code]({{ site.url}}/2022/02/advent-of-code-2021/), solving mazes was new to me and I enjoyed learning about and implementing maze solving algorithms. Today, the way to learn has improved; instead of crafting search terms and scanning results trying to determine if they're relevant, we can simply ask a [Large Language Model](https://en.wikipedia.org/wiki/Large_language_model) for hints or pointers:

> Given a list of connected nodes represented by "node1-node2", how can I find the largest set of nodes that are all connected to each other. Without giving me the answer or an implementation, could you give me a hint about how I could think about solving this please?

Reading through the response that ChatGPT 4o gave me[^3], it suggested I consider one of two efficient algorithms:
* Bron-Kerbosch
* Heuristics (if the graph is too large for exact solutions).

Cue some reading up about [Bron-Kerbosch](https://en.wikipedia.org/wiki/Bron–Kerbosch_algorithm); then an implemenation of the pseudocode with a tweak to return the list of cliques found, and I had my 41st ⭐️

``` c#
public string Part2()
{
    var result = BronKerbosch(
        new HashSet<string>(),
        _linksPerComputer.Keys.ToHashSet(),
        new HashSet<string>(),
        _linksPerComputer);

    return string.Join(',', result.MaxBy(r => r.Count));
}
```

I think I'll add my new `BronKerbosch()` function to the [`AdventOfCode.Common`](https://github.com/AndrewFreemantle/AdventOfCode.Common) toolbox library 🧰

---

[^1]: Yes, I'm old enough to remember LAN Parties, I hosted a few back in the day too 😉
[^2]: As the [.NET Core languages and libraries are open-source](https://github.com/dotnet/runtime), a quick search found [this `foreach()` loop](https://github.com/dotnet/runtime/blob/31af68bcfe08911c4bff57b24dc12774d19c2dc9/src/libraries/System.Linq/src/System/Linq/SelectMany.cs#L96) in the `SelectMany()` implementation.
[^3]: After reading the Wikipedia page on the [Bron-Kerbosch algorithm](https://en.wikipedia.org/wiki/Bron–Kerbosch_algorithm), I asked ChatGPT to explain it to me. It helpfully included an example but an incorrect one:<br><img src="{{ site.imageurl }}2024/advent-of-code/2024-day-23-chatgpt-mistake.png" width="75%"><br>This was a good test to make sure I was paying attention and spotting it proved I understood what I'd researched.
