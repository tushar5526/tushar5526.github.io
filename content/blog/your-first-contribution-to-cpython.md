---
title: "Your First Contribution to CPython"
date: 2023-01-18T15:35:10+01:00
tags:
- opensource
- python
- cpython
---

Do you love Python?

I certainly do.

Have you ever thought how cool it would be to contribute to it?

Sounds scary? It is not!

Python, or more specifically [CPython](https://github.com/python/cpython) as
the reference implementation of the Python programming language is called,
is an open source project like any other - nothing magical.

## Other resources

While there are many great resources out there to prepare you for the first contribution,
such as 
- [Python Developer's Guide](https://devguide.python.org/)
- [Start Contributing to Python: Your First Steps](https://realpython.com/start-contributing-python/)
- or even the fantastic book [CPython Internals](https://realpython.com/products/cpython-internals-book/)
they are pretty extensive,
and I think we can do without, at least for our first contribution.

## Where would I start





So, maybe I was wrong. There is something magical about contributing to CPython,
or at least seeing my changes merged and even backported to older versions still gives me goose bumps.



https://devguide.python.org/core-developers/experts/

####

❯ git worktree add -b specify-rglob-pattern ~/worktrees/cpython/specify-rglob-pattern
Preparing worktree (new branch 'specify-rglob-pattern')
Updating files: 100% (4453/4453), done.
HEAD is now at b84be8d9c0 Docs: improve sqlite3 placeholders example (#101092)

cpython 



   Patterns are the same as for :mod:`fnmatch`, with the addition of "``**``"
   which means "this directory and all subdirectories, recursively".  In other
   words, it enables recursive globbing::
   
   
   
   
      Patterns are the same as for :mod:`fnmatch`, with the addition of "``**``"
   which means "this directory and all subdirectories, recursively".  In other
   words, it enables recursive globbing::
   
   
   make -C Doc/
   
   
   ❯ make venv -C Doc/
   
   ❯ make -C Doc/ html
   
   
   The HTML pages are in build/html.
Writing glossary.json


❯ google-chrome Doc/build/html/index.html



   GH-101112: Specify type of pattern for Path.rglob

The documentation for `rglob` did not mention what `pattern` actually
is. Mention and link that `pattern` is the same as for `fnmatch`, which
shows helpful syntax in its documentation.


❯ git push fork

click on link

delete comments below

be sure to target main branch



   
   
   google-chrome go