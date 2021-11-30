---
title: "Testing Argparse Applications - the Better Way"
date: 2021-11-30T17:07:22+01:00
tags:
- pytest
- argparse
- mock
- patch
- cli
---

When you are creating command line applications in Python,
you probably heard of [argparse](https://docs.python.org/3/library/argparse.html),
which is a great library for exactly doing this,
and it is even included in Python's standard library.

Imagine you have created the following [argparse](https://docs.python.org/3/library/argparse.html) application:

<main.py>
```
import argparse

def main():
    parser = argparse.ArgumentParser()
    parser.add_argument('--name', required=True)
    args = parser.parse_args()
    print(f'Hello {args.name}')

if __name__ == '__main__':
    sys.exit(main())
```

Looks straightforward, works great, but at one point,
you certainly want to add tests for it, right?

## The problem

`argparse.parse_args()` is reading the arguments (e.g. `--name`) from `sys.argv`,
so you cannot set some test values directly.

## How would you test the code?

A quick search on Google showed a couple of solutions,
from patching `parse_args` over to patching values into `sys.argv` and even more frightening ways.

Actually, you could also create a "black box test" by using `os.system("<call your app>")` to run your app.

## Let's try patching sys.argv

As mentioned you can patch `sys.argv`.

<test_main.py>
```
from unittest.mock import patch
from main import main

def test_main(capsys):
    with patch("sys.argv", ["main", "--name", "Jürgen"]):
        main()
        captured = capsys.readouterr()
        assert captured.out == "Hello Jürgen\n"
```

This certainly works, but there must be a better way, right?

## Less patching, more passing in

Indeed, you can rewrite the code as following...

<main.py>
```
import argparse
import sys

def main(argv=None):
    parser = argparse.ArgumentParser()
    parser.add_argument('--name', required=True)

    if not argv:
        parser.print_help()
        sys.exit(1)

    args = parser.parse_args(argv)
    print(f'Hello {args.name}')

if __name__ == '__main__':
    sys.exit(main())
```

Now, instead of relying on `sys.argv`,
you can also pass in arguments to your `main` function directly.

This way, you can rewrite your test as following:

<test_main.py>
```
from main import main

def test_main_even_simpler(capsys):
    main(["--name", "Jürgen"])
    captured = capsys.readouterr()
    assert captured.out == "Hello Jürgen\n"
```

There is no more need to patch `sys.argv`!

### Thank you

Thanks to [Anthony Sottile](https://twitter.com/codewithanthony/),
who should me this "trick" sometimes back in 2020.

## Updates

### 2021.11.30

Thanks to [Anthony Sottile](https://twitter.com/codewithanthony/status/1465740172633554948) I now know that "You don't even need the hackery with `sys.argv[1:]`"
and so I was able to make the above code even simpler.
