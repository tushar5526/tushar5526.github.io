---
title: "How to Install a Python Package Directly From a Git Branch From GitHub"
date: 2021-01-11T08:55:54+02:00
tags:
- git
- python
- github
- packaging
---

[Bernát Gábor](https://www.bernat.tech/) is currently working on a complete rewrite for [tox](https://github.com/tox-dev/tox).

I alpha-tested the latest release (4.0.0a2),
and reported a couple of problems.

Within a day Bernát published some fixes on the `rewrite` branch, 
and asked me to test it.

That means there is no new package on `PyPI`, yet.

So, I had to install `tox` directly from GitHub,
to be exact, from the `rewrite` branch:

```bash
pip install git+https://github.com/tox-dev/tox@rewrite
```

or more general

```bash
pip install git+https://github.com/username/repository@branch
```


## Update

Ok, installing from a branch works,
but what to do when you want to install from a [commit](https://github.com/tox-dev/tox/issues/1782#issuecomment-758188906)?

### How to install a Python package from a commit ID?

This works the same way as described above.

```bash
pip install git+https://github.com/gaborbernat/tox@0da060f85e38356afedb240fa4207c4fc6de6276

```

## Thanks

Thanks a lot [Sviatoslav](https://twitter.com/webKnjaZ/status/1348636796008198145) for the hint!
