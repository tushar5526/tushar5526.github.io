---
title: "Anonymous Hyperlinks"
date: 2023-07-10T18:10:41+02:00
tags:
- documentation
- rst
- reStructuredText
---

Today, I looked something up in our [documentation](https://launchpad.readthedocs.io/en/latest/),
and noticed an unformatted link.

```
`launchpad-mojo-specs <https://code.launchpad.net/launchpad-mojo-specs>`
```

This is reStructuredText - a file format for structured data.
While many people have switched to MarkDown, which is unarguably easier to read
and write, I still like reStructuredText as it is very powerful.

The above link does not get rendered as a hyperlink, as it misses a trailing
underscore.

I was about to add it, and directly commit, but luckily I tried to render it
first.

And boom...

```bash
❯ tox -e docs
docs: commands[0]> sphinx-build -W -b html /home/jugmac00/launchpad/launchpad/doc/ /home/jugmac00/launchpad/launchpad/doc/_build/html
Running Sphinx v4.5.0
loading pickled environment... done
building [mo]: targets for 0 po files that are out of date
building [html]: targets for 1 source files that are out of date
updating environment: 0 added, 1 changed, 0 removed
reading sources... [100%] explanation/charms                                    

Warning, treated as error:
/home/jugmac00/launchpad/launchpad/doc/explanation/charms.rst:3:Duplicate explicit target name: "launchpad-mojo-specs".
docs: exit 2 (0.58 seconds) /home/jugmac00/launchpad/launchpad> sphinx-build -W -b html /home/jugmac00/launchpad/launchpad/doc/ /home/jugmac00/launchpad/launchpad/doc/_build/html pid=2179485
  docs: FAIL code 2 (0.62=setup[0.05]+cmd[0.58] seconds)
  evaluation failed :( (0.68 seconds)
```

What is going on?

```
Duplicate explicit target name: "launchpad-mojo-specs".
```

So, what does that mean?

I searched through the document and noticed that the very same link label
`launchpad-mojo-specs` was used with another URL and Sphinx does not like that :-)

So, now there are two solutions:

1. unify the links (that is what I did)

2. convert that link into an [anonymous hyperlink](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#anonymous-hyperlinks)
by adding two instead of one trailing hyperlink.

P.S.: One of my pet peeves are error messages.

Could we make the error message better?

Let's try...

```plain
The explicit target name "launchpad-mojo-specs" was used multiple times with
different targets / URLs.

This violates the spec (https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html).

You need to either use the same target, or use anonymous hyperlinks
(https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#anonymous-hyperlinks).
```

What do you say? Let me know.
