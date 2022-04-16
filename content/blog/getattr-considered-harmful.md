---
title: "getattr() Considered Harmful"
date: 2022-04-16T15:40:58+02:00
tags:
- python
- coverage
---

While Hynek already thought in 2016 that "Considered Harmful" was getting old,
and so he named this blog post [hasattr() – A Dangerous Misnomer](https://hynek.me/articles/hasattr/),
instead of *hasattr() considered harmful*, 
meanwhile it is 2022 and I still like that phrase.

So here we go...

## `getattr()` considered harmful

The setting is a CLI application with 100% test coverage,
and even [branch coverage](https://coverage.readthedocs.io/en/6.3.2/branch.html) is activated.

## coverage 101

I assume you know what coverage is.
100% test coverage means that the test suite covers all lines of code of your
library or application.
That does not mean they are functionally correct,
but this means they are all run.
This eliminates a couple of error classes, e.g. syntax errors.

But we still have a problem with e.g. the following code:

```python
if function_call():
    do_something()
```

When in our test suite `function_call` is true,
both lines will be executed and a code coverage of 100% is shown.

But we may have never tested the case when `function_call` is false.

## hello branch coverage

When you activate [branch coverage](https://coverage.readthedocs.io/en/6.3.2/branch.html),
[coverage.py](https://coverage.readthedocs.io/) will show an error for above
code example:

> line x didn't jump to line y, because the condition on line x was never false

Excellent! We are safe... right?

## hello `getattr()`

Back to our CLI application.

A couple of weeks ago I renamed a flag for the application's `run` command from
`--output` to `--output-directory`.

Easy change, clear improvement, no fear as we have 100% test coverage.

## hello regression

Fast forward a couple of weeks.

My colleague created another merge proposal to fix a regression
I introduced with the above renaming.

I forgot to also update the flag for the `run_one` command.

How was this possible when we have 100% test coverage?

Let's have a look at the following code...

```python
_run_job(args.job, job, provider, getattr(args, "output", None))
```

If you have a close look at above line,
you'll notice that there is a hidden branch,
ie the `getattr` covers two different outcomes:

- `args` has an attribute `output`
- `args` has no attribute `output`

This could have been written as:

```python
if hasattr(args, "output"):
    _run_job(args.job, job, provider, args.output)
_run_job(args.job, job, provider, None))
```

Certainly, then coverage, at least with activated branch coverage,
would have raised the alarm!

## more gotchas

Please note that there are more idioms in Python which impede coverage from being reliable:

```python
x = {"a": 1, "b": 2}.get("z", 99)
```

```python
x = condition and 0 or 1
```

```python
x = 0 if condition else 1
```

I won't list multiple statements on one line, combined with a semicolon,
as [flake8](https://flake8.pycqa.org/en/latest/),
which you hopefully use,
will prevent this.

And there might be more.

Do you know more examples? Please contact me https://jugmac00.github.io/

## what to do

Well, you could avoid using the above mentioned idioms. :-/

You could use [mutation testing](https://en.wikipedia.org/wiki/Mutation_testing),
which mutates the code and raises alarm if this does not cause a test failure.
But that would be another topic.

At very least - be aware of this "invisible" test or coverage gap!
