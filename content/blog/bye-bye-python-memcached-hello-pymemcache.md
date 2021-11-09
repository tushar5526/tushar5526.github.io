---
title: "Bye-Bye python-memcached, hello pymemcache"
date: 2021-11-09T11:40:26+01:00
tags:
- python
- memcached
- pymemcache
- unmaintained
---

For one app that I help maintaining I noticed that **python-memcached** was used, which has not been updated in several years.

There were some efforts [to transfer python-memcached](https://github.com/linsomniac/python-memcached/issues/95) to a new maintainer,
but at the end that did not work out.

So, [the project is dead](https://github.com/linsomniac/python-memcached/issues/177).

While an unmaintained project may currently work,
there are several things to consider:
- there may be bugs which do not get fixed
- there may be security issues which do not get fixed
- it may stop working for e.g. a new Python version

So something needed to be done!

Luckily,
with [pymemcache](https://pypi.org/project/pymemcache/) there is a successor,
even backed by a big company.

One of the contributors of the successor even put some effort into [making it largely compatible](https://github.com/linsomniac/python-memcached/issues/177#issuecomment-744345125).

So, as a first step I updated the app's dependencies from **python-memcached** to **pymemcache**.

In the build configuration (e.g. `setup.py`, `setup.cfg`, `pyproject.toml`...) I swapped the names,
and as this is an application, I also updated the `requirements.txt`.

## Are we there yet?

Not so fast.

First, I needed to figure out what needed to be done to the imports.

The package is all about creating a connection to a [memcached](https://memcached.org/) server,
so I had to have a look at the `Client` class.

For **python-memcached** the import statement is:

```python
from memcache import Client
```

> Have you noticed?
> It is not called `from python_memcached import Client`!
> While the package name in e.g. setup.py (and thus for PyPI)
and the package name from the source code are usually the same,
that is not necessary.

For **pymemcache** it is... wait. There is more than one `Client` class.

There is `pymemcache.client.base.Client`, `pymemcache.client.hash.HashClient`... and some more.

As the first one is meant to be able to only connect to one server,
I chose the latter one.

## Signature has changed

Luckily,
we have a fairly strong test suite,
so after updating the import statements,
I knew that the test suite would drive my migration attempts.

The first problem was...

```python
TypeError: set() got an unexpected keyword argument 'min_compress_len'
```

The signature for `Client.set` has changed - `min_compress_len` is no more.

There was not much to do,
except reading the docstring of **python-memcached**'s `Client.set`.

I decided to just drop it,
as there is no replacement.

## Next one, please

```python
TypeError: set() got an unexpected keyword argument 'time'
```

When I compared the old and the new library,
I noticed that the `time` argument is now called `expire`.

So I just needed to update that - for all callers!

## No more debug

The next problem was a connection problem of the client to the server.

I used [pdbpp](https://pypi.org/project/pdbpp/) to have a look what was going on:

Passing `[('127.0.0.1:11242', 1)]` to a `Client`'s init (**python-memcached**) worked,
as `1` is recognized as debug flag,
but **pymemcache** dropped that argument.

**pymemcache** can handle different [formats](https://github.com/pinterest/pymemcache/blob/79d98d23cd589556b0dc51a7d10ea338e90a944e/pymemcache/client/base.py#L120-L138),
so I used a list of strings:

```
['127.0.0.1:11242']
```

## Connection refused

Oh my!

The next one was really tough.

```
ConnectionRefusedError: [Errno 111] Connection refused
```

No matter what I tried,
I could not connect to the server,
but only from within a test.

So, I had a hunch that this was about the test setup.

As I got stuck,
I asked a colleague for help,
who pointed me out to a pattern we use in the test setup.

During the test setup we try to set a value via `HashClient.set` to check whether the server is up already.

While `python-memcached` returned an int,
`pymemcache` raises an exception when the server is not yet available,
which I needed to catch now.

## Forget Dead Hosts

Next one...

```
AttributeError: 'HashClient' object has no attribute 'forget_dead_hosts'
```

Same as above for `min_compress_len`,
this member has gone and after reading its docstring I decided to drop it,
especially as it was only called in our test code.

## The end is near

The test suite now ran with 3 failures and 1 error.

```
AttributeError: 'HashClient' object has no attribute 'MemcachedKeyCharacterError'
```

Reading some old code comments, it looks like my predecessors had been bitten by a bug in memcached,
which allowed spaces in keys, so a regression test was written.

**pymemcache** decided to both move and rename the exception.

`MemcachedKeyCharacterError` -> `MemcacheIllegalInputError`

## Last but not least

The last three failures had the same reason:

```
testtools.matchers._impl.MismatchError: b'somevalue' != 'somevalue'
```

Oh wow! Instead of strings, we now got bytes!

I had a look at pymemcache's [documentation](https://pymemcache.readthedocs.io/en/latest/getting_started.html#interacting-with-pymemcache)
and it looks like this behavior is intended... wat?

```
>>> from pymemcache.client.base import Client
>>> client = Client('127.0.0.1')
>>> client.set('some_key', 'some_value')
True
>>> client.get('some_key')
b'some_value'
```

I tried to find a reason for this behavior,
but without success.

I really hope a reader can shed some light into this.

Even more confusing, I found a [blog post](https://realpython.com/python-memcache-efficient-caching/#storing-and-retrieving-cached-values-using-python) at Real Python,
where the example code showed a behavior I would expect, ie string in -> string out.

So, something was wrong here.

Digging in **pymemcache**'s source code and its documentation
I had a hunch that this had something to do with serializing and deserializing the data.

The solution was to pass in both a serializer and a deserializer when initializing a `HashClient` object:

```
from pymemcache.client.hash import HashClient

from pymemcache.serde import python_memcache_deserializer
from pymemcache.serde import python_memcache_serializer


HashClient(
    servers,
    serializer=python_memcache_serializer,
    deserializer=python_memcache_deserializer)
```

# The End

I hope this migration guide will help someone out there with a similar task.

I also hope the maintainers of `pymemcache` consider linking to this blog post :-)
