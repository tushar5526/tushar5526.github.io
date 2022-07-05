---
title: "Bite My Shiny Type Annotated Library"
date: 2021-05-09T15:40:11+02:00
---

How do you make type annotations available to the users of your library?

Well, you just type annotate your library, right?

No!

But let's step back for a moment.

## Flask 2.0 goes full type annotations

This morning I read David Lord's [announcement](https://twitter.com/davidism/status/1391130343286001664) that Flask, Jinja, Click, Werkzeug, MarkupSafe,
and ItsDangerous are now fully type annotated,
and new releases will be available next week.

Ok, as I typed [Flask-Reuploaded](https://github.com/jugmac00/flask-reuploaded) almost a year ago,
I certainly noticed that Flask was not typed back then,
but external type information was provided via [typeshed](https://github.com/python/typeshed),
which I remember lively, as I had to add a missing type annotation for [Werkzeug](https://github.com/python/typeshed/pull/4308).

## I wonder how...

I immediately wondered,
how the type checkers then would know whether to use the inline type information or the type stubs from **typeshed**.

Anthony Sottile responded on the Twitter thread,
that [PEP 561](https://www.python.org/dev/peps/pep-0561/) handles this and he linked to one of his [videos](https://www.youtube.com/watch?v=n4GJ8rp6DpE).

In short - you need to include an empty `py.typed` in your repository/package.

What?

So, this basically means the applied type annotations have been in vain - for almost an entire year.

I am not the only one bitten by that.
I am in [very good company](https://github.com/encode/httpx/issues/193).

By the way... `py.typed` has to be in your package root, not necessarily in your git root!

## Make mypy and co aware of your type annotations

So, we know what to do, but how?

This depends on your build backend... and some more things,
like whether you prefer `setup.py` or `setup.cfg`,
whether you prefer to use `package_data` or rather `include_package_data` and use a `MANIFEST.in`...

Nobody claimed Python packaging is easy!

### MANIFEST.in

After adding `py.typed` to my repository,
the indispensable [check-manifest](https://pypi.org/project/check-manifest/) told me what to do:

```bash
❯ check-manifest 
lists of files in version control and sdist do not match!
missing from sdist:
  py.typed
suggested MANIFEST.in rules:
  include *.typed
```

or simply add e.g. ...

```ini
include src/your_package/py.typed
```

P.S.: Do not forget to add the `include_package_data=True` directive to your `setup.py`,
otherwise `py.typed` will be included in the sdist, but not in the wheel.

Sounds logical? Right... :-/

### setup.py

If you do not use a `MANIFEST.in`, but `setuptools` with a `setup.py`...


```python
setup(
    package_data={"your_package": ["py.typed"]},
)
```

While we are at it... take care that `py.typed` is not matched by `exclude_package_data`.

Got it? Almost :-)

You also need to make sure you have the `zip_safe=False` directive set.

### setup.cfg

If you prefer a `setup.cfg` over a `setup.py`...

```ini
[options.package_data]
your_package = py.typed
```

### poetry

If you are into `poetry`, there is great news.

Poetry actually includes everything that’s part of the package source tree in your distribution,
so `py.typed` is included out-of-the-box, no configuration needed.

### flit

The same is true for `flit`!
Just create `py.typed` and add it to your git repository.

## Conclusion

I want to end this journey into the depths of Python packaging with the famous words of a colleague on mine:

"Kaum macht man's richtig, schon geht's."

(Translation: When you start doing it the right way, it will eventually work out.)

## Updates

**2020.05.12**
- [David Lukeš](https://dlukes.github.io/) reported a problem with the `poetry` section.
  Previously, it included an even wrong configuration snippet.
  Good news!
  Poetry automatically packages the files in the source tree of the package.
  Thanks so much for your feedback!
- After David reported how `poetry` packages files,
  I really wanted to know how `flit` is doing this.
  So I created a package with `flit` and updated the blog post with my findings.
