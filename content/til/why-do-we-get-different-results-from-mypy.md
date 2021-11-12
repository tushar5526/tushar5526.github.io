---
title: "Why Do We Get Different Results From Mypy"
date: 2021-11-12T16:12:43+01:00
tags:
- mypy
- cache
---

My colleague added [mypy](https://github.com/python/mypy) as a test environment for our [tox](https://tox.wiki/en/latest/index.html) setup.

When I checked out his branch and ran it on my machine, it failed.

The error does not matter,
but fwiw it was about that one upstream package has [forgotten](https://github.com/canonical/craft-cli/pull/35) to include a `py.typed` file,
a topic which I dedicated [a whole blog post](https://jugmac00.github.io/blog/bite-my-shiny-type-annotated-library/).

The interesting part was...
why did it work on his machine, but not on mine?

I compared the output of both our `tox -rvv -e mypy` runs...
Almost identical, except for the path and a difference in the patch version of Python.

A couple of minutes later my colleague come up with the solution.

Mypy accessed its cache from earlier runs.

Once he deleted the cache, it worked.

In order to prevent future problems,
especially which do not resolve when running `tox -r`,
he updated the `tox` configuration to put the cache directory inside the `tox` environment so it gets deleted on recreation.

```
commands =
    mypy --cache-dir="{envdir}/mypy_cache" --strict {posargs:lpcraft}
```
