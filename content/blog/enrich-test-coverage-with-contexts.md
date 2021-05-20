---
title: "Enrich Test Coverage With Contexts"
date: 2021-05-20T14:22:51+02:00
---

Hold on tight, now I expect a lot of us:

We all
- test our code
- use coverage
- strive for 100% test coverage

So, what's missing?

## I miss some context

100% test coverage is great, but sometimes we need more information.

e.g. which test covers line XXX?

In sufficiently complex code bases,
with mixed unit and integration tests,
this is no longer easy to find out.

Sure, you can set a breakpoint and run your tests
and wait until the breakpoint has been hit.

But this is pretty cumbersome.

## Why?

Before we try to find a better way,
let's pause for a second, and think about,
why would we even want to know which test covers a certain line?

I have a couple of reasons.

First, when working in a complex code base,
especially in open source,
when it is not my day job,
and I want to add a test,
I look for a similar test first,
so it is easier for me to create a new one.

Second, it is good to know how many tests cover a certain line.
In an ideal world, strictly using unit tests only,
every line should be hit as infrequent as possible,
so you can change an implementation without having to change many, many tests.

## There must be a better way

So, instead of setting a breakpoint to find the tests which cover a certain line,
there is indeed a better way.

Since [version 5](https://coverage.readthedocs.io/en/latest/changes.html#version-5-0-2019-12-14)
of [Coverage.py](https://coverage.readthedocs.io/en/v4.5.x/index.html) you can record and later show so-called [contexts](https://coverage.readthedocs.io/en/coverage-5.5/contexts.html).

## Activate contexts for pytest-cov

Mostly, I use `pytest-cov`, which is an integration of `coverage` and `pytest`.

In order to activate `contexts` I have to
- add a directive to record contexts
- add a directive to show contexts

In my configuration file, e.g. `tox.ini`, I add

```ini
[coverage:html]
show_contexts = true
```

and for the test command, I add

```ini
--cov-context=test
```

so my `testenv`, again in `tox.ini`, looks like

```ini
[testenv:coverage]
commands =
    pytest --cov tests --cov flask_uploads --cov-report term-missing --cov-report html --cov-context=test --cov-fail-under=100 {posargs}
```

If you use plain `Coverage.py`,
the [official documentation](https://coverage.readthedocs.io/en/coverage-5.5/contexts.html) tells you what you need to do.

## The result

And what do we get from our updated test and coverage configuration?

This beauty...

![xxx](/images/coverage-with-contexts.png)

Every line is annotated by how many tests it is covered.
Also you can click on the little triangle to get a list of all relevant test functions.


## Conclusion

Today, we learned how to get more information out of the coverage report.

Do you already use this feature? What are your reasons?
Or do you have anything else to comment on this blog post?

Drop me a line!
You can find my contact details at https://jugmac00.github.io/.

## Thank you

Thanks a lot, Ned Batchelder for maintaining this indispensable library in the Python test universe!

If you or especially your company relies on `Coverage.py`,
please consider [sponsoring Ned's work](https://github.com/nedbat/coveragepy).

## Further reading

- Coverapge.py: [Measurement contexts](https://coverage.readthedocs.io/en/coverage-5.5/contexts.html)
- pytest-cov: [Contexts](https://pytest-cov.readthedocs.io/en/latest/contexts.html)
