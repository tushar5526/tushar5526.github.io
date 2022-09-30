---
title: "How to Show Git Log Only for One File Type"
date: 2022-09-30T08:13:24+02:00
tags:
- git
---

Today I wanted to have a look how the `UPDATE` statement was used 
in already deleted SQL patch files

Usually I do a `git log -p` and then use the interactive search in **less** via `/`,
but in this case there were too many hits from Python files,
so I wanted to restrict my search to SQL files only.

Turns out you can pass in the file type via...

```
git grep "UPDATE" -- '*sql'
```
