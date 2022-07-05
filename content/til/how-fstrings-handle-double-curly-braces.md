---
title: "How f-strings Handle Double Curly Braces"
date: 2022-07-05T20:03:01+02:00
tags:
- python
- fstrings
---

Today I was about setting up a test case for parsing a YAML file which is
templated with Jinja variables.

Something like this...

```python
f"""
pipeline:
    - test
jobs:
    test:
        package-repositories:
            - type: apt
              url: "https://{{auth}}@example.org"
"""
```

My test case failed... 

My Pydantic model, which uses `yaml.safe_load` under the hood to load the above
configuration,
returned the URL, even before the value got replaced,
as `https://{auth}@example.org`.

One pair of the double curly braces was stripped away!

Oh no! Is my complete concept broken?
Can't I use Pydantic with `yaml.safe_load` and Jinja templates?
Are my deadlines in danger?

I tried to reproduce the issue in the REPL - hm, `safe_load` does not touch the
curly braces.
What's going on? Different Python versions? Different PyYAML versions?

## f-strings in da house

As I wrote the above string into a temp file in my test fixture,
next I checked whether either `Path.write_text` does nasty things or the test
framework.

And... actually it's Python itself - or more specific `f-strings`!

Turns out double curly braces are the way to write literal curly braces inside
an f-string!

From the official [Python documentation](https://docs.python.org/3/reference/lexical_analysis.html#formatted-string-literals):

> The parts of the string outside curly braces are treated literally,
except that any doubled curly braces '{{' or '}}' are replaced with the
corresponding single curly brace. 
