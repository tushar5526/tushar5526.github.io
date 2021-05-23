---
title: "What Is the Difference Between Invoking `pytest` and `python -m pytest`"
date: 2020-12-03T09:16:51+02:00
tags:
- python
- pytest
---

Yesterday, I was recommended to have a look at [**Shopyo - Open inventory management and Point of sales**](https://github.com/Abdur-rahmaanJ/shopyo).

As I am passionate about testing and CI,
I always have a look at configuration files for newly discovered projects.

After I cloned the repository, I tried to run the tests.

So, without having a look into the documentation ( :-/ ), I
- created a virtual env
- installed the dependencies
- ran `pytest` from the root of the project
- ... and got a `ModuleNotFoundError`

The friendly maintainers of the projects pointed me in the right direction on how to run tests for this project:

```bash
cd shopyo
python -m pytest
```

And indeed, it worked.

The project does not follow the `src` "convention",
so I roughly knew what was going on,
but I missed the exact reason.

[Florian Bruhin](https://twitter.com/the_compiler) gave me the answer on [Twitter](https://twitter.com/the_compiler/status/1334446761511952384):

> Does it work with `PYTHONPATH=. pytest`? The main difference is that `python -m` adds the current directory to `sys.path`, i.e. lets you import modules from there.

So, that's it - `python -m pytest` adds the current directory to `sys.path`!
This makes a lot of sense!

Later, [Paul Ganssle](https://twitter.com/pganssle) linked to his excellent blog post
["Testing your python package as installed"](https://blog.ganssle.io/articles/2019/08/test-as-installed.html),
where he explained this aspect and the whole `to src or not to src` story in detail.
