---
title: "Variable Scope and List Comprehensions"
date: 2021-05-22T14:02:09+02:00
tags:
- python
- comprehensions
hideSummary: true
---

## Python 2

In Python 2 the `temporary` variable was not so temporary at all...

```bash
❯ python2
Python 2.7.17 (default, Feb 27 2021, 15:10:58) 
[GCC 7.5.0] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> s = "abc"
>>> [x for x in s]
['a', 'b', 'c']
>>> x
'c'
```

As you can see, `x` leaks outside the scope the list comprehension.

## Python 3

This has been changed in Python 3.

```bash
❯ python3
Python 3.6.9 (default, Jan 26 2021, 15:33:00) 
[GCC 8.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> s = "abc"
>>> [x for x in s]
['a', 'b', 'c']
>>> x
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'x' is not defined
```

Outside the scope of the comprehension, `x` is no longer defined.


### Thank you!

I just read about this in [Luciano Ramalho's](https://twitter.com/ramalhoorg)
excellent `Fluent Python 2nd edition`.

The book is available as a preview version at [O'Reilly](https://www.oreilly.com/library/view/fluent-python-2nd/9781492056348/).

It will be released at the end of 2021.
