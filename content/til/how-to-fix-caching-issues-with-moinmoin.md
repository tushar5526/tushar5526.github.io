---
title: "How to Fix Caching Issues With MoinMoin wiki"
date: 2021-10-05T08:00:41+02:00
tags:
- moinmoin
- cache
---

After making two changes to a [MoinMoin](https://moinmo.in/) powered page,
the first change showed up immediately,
the second one did not show up at all.

Though the changes were visible both in the **edit** view,
and in the history.

Turns out this was a server-side caching issue
which could be solved by chosing "More Actions" and then "Delete Cache".
