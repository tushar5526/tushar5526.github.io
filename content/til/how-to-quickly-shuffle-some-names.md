---
title: "How to Quickly Shuffle Some Names"
date: 2021-10-26T14:47:22+02:00
tags:
- bash
---

For the daily standup,
we have a random order who starts, who follows next, and so on.

## How to shuffle a couple of names?

Well, obviously, you could fire up a Python repl and use the random library, e.g.

```
>>> from random import shuffle
>>> members = ["Me", "TeamMate1", "TeamMate2", "TeamMate3", "TeamMate4"]
>>> shuffle(members)
>>> members
['TeamMate3', 'TeamMate4', 'TeamMate1', 'TeamMate2', 'Me']
```

While this works,
it is a lot of typing, and a lot of quotes :-)

## There must be a better way

As a matter of fact,
once again Bash (Coreutils) has a solution:

```
shuf
```

Never heard before?
Me neither! I just got to know the command today - thanks to my teammate Colin Watson.

```
$ shuf -e Me TeamMate1 TeamMate2 TeamMate3 TeamMate4
TeamMate4
TeamMate2
Me
TeamMate1
TeamMate3
```
