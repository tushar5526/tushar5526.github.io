---
title: "How to Properly Configure Isort and Black"
date: 2021-11-10T17:32:30+01:00
tags:
- black
- isort
- compatibility
---

When you both use [black](https://pypi.org/project/black/) and [isort](https://pypi.org/project/isort/),
which you probably should,
you will notice that they both touch the import statements.

When you now use both tools e.g. via [pre-commit](https://pre-commit.com/),
it could happen that both tools play ping-pong with each other,
as both tools modify the source code,
and then report a failure.

This is especially "funny",
when both tools report modified source and a failure,
but basically at the end you have a git repository with no modifications,
as the one tools fixes the other's changes.

Anyway, this is well known.
You can configure **isort** to work together with **black** by using the following configuration (e.g. in pyproject.toml):

```toml
[tool.isort]
profile = "black"
multi_line_output = 3
```

This is also well documented in [isort's documentation](https://pycqa.github.io/isort/docs/configuration/black_compatibility.html).


Now, you can imagine my surprised face when - although I applied this configuration - both tools still kept playing ping-pong.

After some time spending with GitHub issues, I finally found the solution:

When you configure **black** to use a custom line length,
you need to do the same for **isort**!

So, the complete configuration needs to look like...

```toml
[tool.black]
line-length = 79

[tool.isort]
profile = "black"
multi_line_output = 3
line_length = 79
```

And well,
at the end I saw [isort's documentation](https://pycqa.github.io/isort/docs/configuration/black_compatibility.html)
is mentioning this already at the top of the page in the **Tip** box :-)
