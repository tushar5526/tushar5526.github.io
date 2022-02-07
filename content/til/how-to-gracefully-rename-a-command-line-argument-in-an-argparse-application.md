---
title: "How to Gracefully Rename a Command Line Argument in an Argparse Application"
date: 2022-02-07T16:45:34+01:00
tags:
- argparse
---

The CI runner I am currently working on can be used as follows:

```
lpcraft run --output
```

Now, what is `output`? A boolean? In the sense of "do create output"? A path to a directory? Something else?

## disambiguate

`--output` can be used to specify a directory for the build artifacts.

So, let's rename it to `--output-directory` - problem solved.

Yes, but what about all the users out there... 

## argparse magic to the rescue

We all know [argparse]() is magic,
and sometimes that even helps.

There is a feature called [arguments abbreviations](https://docs.python.org/3/library/argparse.html#argument-abbreviations-prefix-matching),
which automatically -- drumrolls -- enables abbreviations for arguments.

After changing `lpcraft run --output` to `lpcraft run --output-directory`,
the first command still works, as the argument gets expanded automatically.

This magic behavior can be deactivated by setting `allow_abbrev=False`.
