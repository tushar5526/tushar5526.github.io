---
title: "How to Bring Color Back Into Tox and Pytest"
date: 2021-06-11T15:48:12+02:00
tags:
- pytest
- tox
- tty
---

When I first tried the pre-release of the upcoming [tox rewrite](https://youtu.be/5zEn3Jta2Dg?t=388),
I noticed [tox](https://github.com/tox-dev/tox) itself has funky new colors - which I like a lot...

![tox4](/images/color-tox4.png)

But what happened with the green dots for `pytest's`output?

You may recall, running the old `tox` or `pytest` directly,
the dots would be green...

![tox3](/images/color-tox3.png)

## why has this changed

Honestly, I am not 100% sure.

But I know how to get the colors back.

All you need to do is changing this line `commands = pytest {posargs}` in your `tox.ini`,
to this line `commands = pytest {tty:--color=yes} {posargs}`.

This forces `pytest` to produce colorful output,
even when [invoked without a tty attached](https://github.com/tox-dev/tox/issues/2070#issuecomment-849503960).
And this is possible the `why` - `pytest` is called internally in a different way? Maybe?

Here we go...

![tox4 with color](/images/color-tox4-with-color-on.png)
