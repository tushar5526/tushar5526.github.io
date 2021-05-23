---
title: "How to Delete Complete Lines With Search Replace"
date: 2020-11-18T13:21:11+02:00
tags:
- vscode
---

For many years now,
I had been using Jetbrain's IntelliJ Idea Ultimate happily also for my Python development.

Due to "legacy issues", I had to apply many `# noinspection` annotations to my source code.

Now, that I switched to VS Code,
I want to get rid of actually 821 `# noinspection` annotations :-)

A simple search for `# noinspection.*` and a replacement with basically *nothing* would work,
but that would leave behind many blank lines.
And then `flake8` would complain.

## simple solution

search for `^# noinspection.*$\n`,
ie expand the match with the newline character,
and then replace with *nothing*


## thank you

https://stackoverflow.com/a/53287432/672833
