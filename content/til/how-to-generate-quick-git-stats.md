---
title: "How to Generate Quick Git Stats"
date: 2021-06-21T14:15:50+02:00
tag:
- git
- stats
---

Ever wondered how many lines of code you have added or deleted,
within a day or a week?

Here we go...

```bash
❯ git diff --shortstat "@{1 day ago}"
 36 files changed, 35 insertions(+), 1263 deletions(-)
```

```bash
❯ git diff --shortstat "@{1 week ago}"
 40 files changed, 821 insertions(+), 1270 deletions(-)
```

How do you generate stats from `git`?

Drop me a line on [Twitter](https://twitter.com/jugmac00)
or via [e-mail](https://jugmac00.github.io/).
