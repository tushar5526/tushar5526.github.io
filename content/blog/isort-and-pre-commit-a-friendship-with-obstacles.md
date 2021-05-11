---
title: "isort and pre-commit - a friendship with obstacles"
date: 2020-02-23T14:06:45+02:00
---

## Note

This post is not intended to be an exhaustive introduction to `pre-commit` and its hooks.
Please visit [pre-commit.com](https://pre-commit.com/) for a complete documentation.

In a nutshell...

## pre-commit

From its [website](https://pre-commit.com/):

> A framework for managing and maintaining multi-language pre-commit hooks.

Very simplified this means, whenever you try to commit changes in your project (e.g. entering `git commit`),
`pre-commit` runs all configured tools (e.g. linter, formatter, ...),
and only if they run successfully, your commit will be executed - otherwise your commit will fail.

This is really great, as you will never ever forget to run your linter or other tools.

## isort

One of those tools, which pre-commit can execute for you, is `isort` - short for "sort imports".

As the name suggests, this little helper sorts your import statements,
both alphabetical and also PEP 8 compliant,
means e.g. standard library imports are separated from the ones of third party tools.
And I like this a lot!

A sample configuration for `pre-commit` to use `isort` could look like this - by the way, this is YAML:

```yaml
-   repo: https://github.com/timothycrosley/isort
    rev: 4.3.21
    hooks:
    -   id: isort
```

Also, you can customize `isort` to our liking, ie. put every import on one line.
This is just one of many configuration options.

And with the configuration options there comes the first problem.

## don't touch my migrations

Whenever you run an application with a relational database,
you probably also need to keep track of `migrations`,
which usually consists of files with the diffs between two database schemes.

For my `Flask` app, those are stored in a subfolder called `migrations`.

No problem at all!
Let's use `isort's` `skip` / `skip-glob` option
(see `isort's` [wiki]( https://github.com/timothycrosley/isort/wiki/isort-Settings)).

Looks easy enough.

After some [googling]( https://github.com/timothycrosley/isort/issues/282 ),
my first try would be something like this:

```ini
[isort]
skip = migrations
```

This does not work. Hm, maybe I need to use globbing (wildcards)?

## Note

I put `isort`'s configuration into my `tox.ini` file.
You can certainly also use a dedicated `.isort.cfg` file.
You'd have to replace the section `isort` with `settings` then.

After some more googling I came up with this:

```ini
[isort]
skip_glob = */migrations/*.py
```

Guess what? This also does not work. All my migration files still get sorted by `isort`.

After some more investigation,
I found  https://github.com/timothycrosley/isort/issues/885
(as of 2021, this link no more works, as the repository was moved to another GitHub organization):

There is a new "never skip when passed a filename" behavior - which is... new.
So, even if you put a file or a pattern into a `skip` option,
it does not get skipped when passed to `isort` directly.
And this is what I assume `pre-commit` is doing.

Ok, in the same issue a workaround was mentioned:
Use `--filter-files` on the command line,
which by the way, I do not use, as I use `pre-commit`,
but fortunately you can add it to your `pre-commit` hook configuration:

```yaml
  - repo: https://github.com/timothycrosley/isort
    rev: 4.3.21
    hooks:
    - id: isort
      args: [--filter-files]
```

It works!

## Note

As an [alternative](https://www.gitmemory.com/issue/pre-commit/mirrors-isort/9/511434587),
you could also add an `exclude: migrations/` statement in your pre-commit-config.yaml.

Let me rather say... it worked, until I updated from `4.3.21` to `4.3.21-2` - there are no changelog entries available for this update,
but there were tons of changes,
including different behavior which files should be scanned.

With this update, all files gets processed by `isort`, including `.po`, `.rst`, `.json` and many more.

Ok, what can we do about it?

First, file an issue (https://github.com/timothycrosley/isort/issues/1154).

Reading the excellent `pre-commit` documentation,
it seems there are several options,
but [`filter by files`](https://pre-commit.com/#creating-new-hooks) looks very promising.

The `files` statement supports regular expressions, so the new hook looks like this:

```yaml
  - repo: https://github.com/timothycrosley/isort
    rev: 4.3.21-2
    hooks:
    - id: isort
      args: [--filter-files]
      files: \.py$
```

And here we go... it finally works as intended.


## Update

I could track down the faulty behavior (`isort` also touches files which are not Python files) to a regression/packaging bug - `.pre-commit-hook.yaml` is broken for 4.3.21-2 - see the issue at `isort`'s [bug tracker](https://github.com/timothycrosley/isort/issues/1154#issuecomment-587517806).

I removed the section about `seed-isort-config`,
using this package is no longer necessary with the latest `isort` versions.
