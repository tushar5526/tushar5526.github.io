---
title: "How to Compare Two Directories on Linux"
date: 2021-11-03T16:24:37+01:00
tags:
- diff
- linux
- cli
---

While migrating a couple of Bazaar repositories to git,
I experienced some problems with one Bazaar repository,
or let's say the exporter did not work properly on one of its commit.

In order to find out what exactly happened,
I needed to compare both the tip of the git and the Bazaar repository.

Turns out, diff can not only compare files, but also directories... recursively!

```bash
$ diff -qr bzr/wadllib/ git/wadllib/
Only in bzr/wadllib/: .bzr
Only in git/wadllib/: .git
Only in bzr/wadllib/: MOVED_TO_GIT
Only in git/wadllib/src/wadllib: diff
Only in git/wadllib/src/wadllib/docs: __init__.py
Only in git/wadllib/src/wadllib: _utils
```
