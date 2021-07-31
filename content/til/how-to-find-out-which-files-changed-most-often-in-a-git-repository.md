---
title: "How to Find Out Which Files Changed Most Often in a Git Repository"
date: 2021-07-31T14:34:02+02:00
tags:
- git
- awk
---

I found this helpful command while cleaning up my Desktop - I cannot recall where I got it from :-/

```bash
git log --since 6.months.ago --numstat |
  awk '/^[0-9-]+/{ print $NF}' |
  sort |
  uniq -c |
  sort -nr |
  head
```

This will list the top 10 most often changed files in your git repository.

Why would you want to know this?

Well, several things come to my mind:
- you get a quick overview what was changed in the last couple of months
- this could indicate problems such as a *god class*,
when a file shows up you did not expect

What do you think? Let me know!
