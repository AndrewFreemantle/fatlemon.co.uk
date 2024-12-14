---
title: 'AoC# 2024 - Day 14: Restroom Redoubt'
permalink: /2024/12/advent-of-code-2024-day14/
excerpt: 'Advent of Code 2024 in C# - Day 14 ⭐️⭐️'
redirect_from:
  - /aoc-2024-day14/
tags:
  - Advent of Code
  - 2024
  - Day 14
  - dotnet
---

It's Advent of Code time 🎄

> [Advent of Code](https://adventofcode.com/2024/about) is an [Advent calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small programming puzzles for a variety of skill levels that can be solved in any programming language you like.

We've already [dodged guards]({{ site.url }}/2024/12/advent-of-code-2024-day06/) this year, and on [Day 14](https://adventofcode.com/2024/day/14) we're avoiding security robots!

### Part 1 ⭐️
Straightforward enough, once I'd fixed a couple of off-by-1 errors: I'd taken the tile space as given but used zero-indexing for the robot positions; and teleporting with a negative velocity to a position less than zero needed to account for zero being a space/move.

### Part 2 ⭐️
An unexpected twist...

<img width="75%" src="https://preview.redd.it/2024-day-14-did-not-see-that-one-coming-v0-vetnut3tar6e1.png?auto=webp&s=adbeb4f8eaa40b55094d96363151c86ed501b531" />
<figcaption>credit: <a href="https://www.reddit.com/r/adventofcode/comments/1hdwpak/2024_day_14_did_not_see_that_one_coming/">fit_femboy_</a></figcaption>

But how's this for a vague requirement:

> most of the robots should arrange themselves into **a picture of a Christmas tree**.
>
> **What is the fewest number of seconds that must elapse for the robots to display the Easter egg?**


What, *exactly*, are we looking for?! No hint of where in the space or what size this tree could be[^1] meant that my first thought was to compare the left half of the space mirrored the right[^2], but the "most of the robots" gave me doubt this would work.

My next thought was to eyeball the output... it was easy to create a simple console output map of the robot's locations but it was hard to track, so I switched to rendering ~10,000[^3] images as they'd be easier to page through in the Finder:

![image of a folder full of image thumbnails, one of which has a small box containing a christmas tree in it]({{ site.imageurl }}2024/advent-of-code/2024-day14-finder.png)
<figcaption>Ahhh! That's what we're looking for!</figcaption>

I wasn't the only one with this idea 😄

<img width="50%" src="https://preview.redd.it/2024-day-14-part-2-v0-ea28hb3o3r6e1.png?auto=webp&s=a97940493393b2ee7ec8d83c9c2c79f224d54305" />
<figcaption>credit: <a href="https://www.reddit.com/r/adventofcode/comments/1hdw2m1/2024_day_14_part_2/">piman51277</a></figcaption>

---

[^1]: There wasn't a hint about trees in the previous year's puzzle referenced in the introduction; it concerns keypad entry codes.
[^2]: Not a bad thought, given Part 1 has us counting robots in quadrants.
[^3]: 10,000 was chosen at random. I figured I could just keep generating blocks of images if need be. Turns out my answer was in the first block.
