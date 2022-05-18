---
title: "Choosing Singular or Plural for Argument Names in Argparse"
date: 2022-05-18T15:16:32+02:00
tags:
- argparse
- singular
- plural
---

Naming is hard, right?

Today I was extending an
[argparse]((https://docs.python.org/3/library/argparse.html)) based CLI
application.

I wanted to add an optional argument so that I can pass in environment variables.

For this argparse has a special "action", called `append`.

From the [argparse documentation](https://docs.python.org/3/library/argparse.html#action):

```
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--foo', action='append')
>>> parser.parse_args('--foo 1 --foo 2'.split())
Namespace(foo=['1', '2'])
```

Awesome!

Now I can access all the provided values as `foo`.

## Wait a moment

Wait! This does not match. I do not access `foo` but `foos`!

The problem is more visible when I use a real argument name.

I pass in several environment variables with `--environment-variable` each,
and access them via `environment_variable` - this does not feel right.

Also it is not better to name the argument `--environment-variables`,
as I pass in only one variable at a time.

Nobody likes a confusing cli API.

## Colleague to the rescue

My colleague [Colin](https://www.chiark.greenend.org.uk/~cjwatson/blog/)
pointed out that argparse's `add_argument` method has a
[dest](https://docs.python.org/3/library/argparse.html#dest) keyword,
which determine the name under which the provided values are accessible.

## Final solution

Combining `append` with `dest` is the way to go:

```
parser.add_argument('--foo', action='append', dest='foos')
```

and

```
parser.add_argument(
    '--environment-variable', action='append', dest='environment_variables'
)
```
