---
title: "Combine Coverage for Different Python Versions"
date: 2021-11-21T15:34:38+01:00
tags:
- coverage
- tox
---

Is it enough to run code coverage for a single Python version?

Probably, but not necessarily.

Especially when you still need to support Python 2.7 (sigh),
there could be quite some different code paths for Python 2 and Python 3.

A simple example...

```python
try:
    import Queue  # Python 2
except ImportError:
    import queue as Queue  # Python 3
```

But also the different Python 3 versions may require that you not only test your code for each interpreter,
but also you need to assess code coverage for the different versions,
and certainly you want to make sure you got all paths covered.

When I speak of coverage,
I certainly speak of [Ned Batchelder's](https://twitter.com/nedbat) [coverage.py package](https://coverage.readthedocs.io/en/latest/).

> I wrote previously about how to [Enrich Test Coverage With Contexts](https://jugmac00.github.io/blog/enrich-test-coverage-with-contexts/).


## The Problem

Let's assume you have a simple `tox.ini` - you use [tox](https://tox.wiki/en/latest/) for testing, right?

```ini
[tox]
envlist =
    py27
    py38
    py39
    coverage

[testenv]
deps = 
    pytest
commands = 
    pytest

[testenv:coverage]
basepython =
     python3
deps =
    coverage run -m pytest
    coverage report -m --fail-under=100
```

Given your code has some branches which are Python 2 specific,
coverage would not reach 100% and fail.

The same would apply when your code base has some e.g. Python 3.9 specific code paths.

## How to combine coverage?

Obviously, with `coverage combine` :-)

You need to run your tests via coverage, and then combine the results.

```ini
[tox]
envlist =
    py27
    py38
    py39
    coverage

[testenv]
deps =
    coverage
    pytest
commands = 
    coverage -m pytest

[testenv:coverage]
basepython =
    python3
deps =
    coverage combine
    coverage report -m --fail-under=100
```

Not so fast - there is one problem:

Coverage saves the data in `.coverage`,
and so the file gets created by one env run, and overwritten by the next one.

You could certainly manipulate the coverage runs to save the coverage files into separate files,
but `coverage.py`,
which is the official name of the plugin,
already got you covered.

You just need to configure `coverage/run` to use the [parallel mode](https://coverage.readthedocs.io/en/latest/cmd.html?highlight=parallel#combining-data-files-coverage-combine).

## The Complete tox Configuration

```ini
[tox]
envlist =
    py27
    py38
    py39
    coverage

[testenv]
deps =
    coverage
    pytest
commands = 
    coverage -m pytest

[testenv:coverage]
basepython =
    python3
deps =
    coverage combine
    coverage report -m --fail-under=100

[coverage:run]
parallel =
    true
```

Now you can run your tests and get a combined coverage report by...

```bash
tox -e py27,py38,py39,coverage
# or just
tox
```

## Bonus

What happens when you run `tox` and your envlist is sorted in a way that `coverage` comes first?

Or you run just `tox -e coverage`?

You will see ... `ERROR:   coverage: commands failed`.

The Python environments must be run before the coverage env!

`tox` has a [depends](https://tox.wiki/en/latest/config.html#conf-depends) setting for that.

Please note - this will indeed run the environments in the correct order,
but it will not pull in the dependencies,
if you have not specified them!

=> So `tox -e coverage` will still not work, but `tox -e coverage,py27,py38,py39` will.

> Please note: There is an [open issue](https://github.com/tox-dev/tox/issues/238) on `tox`' issue tracker to implement **group environments**,
which would allow to specify to run a group of environments with a single command.

## Final tox Configuration - This Time For Sure

```ini
[tox]
envlist =
    py27
    py38
    py39
    coverage

[testenv]
deps =
    coverage
    pytest
commands = 
    coverage -m pytest

[testenv:coverage]
basepython =
    python3
skip_install =
    true
deps =
    coverage combine
    coverage report -m --fail-under=100
depends =
    py27
    py38
    py39

[coverage:run]
parallel =
    true
```
