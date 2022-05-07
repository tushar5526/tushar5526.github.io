---
title: "How to Render a Single ReStructuredText Document"
date: 2022-05-07T14:35:22+02:00
tags:
- rst
- restructuredtext
---

Today I came across [ubuntu-archive-tools](https://launchpad.net/ubuntu-archive-tools)
which is a great set of scripts to help administer the Ubuntu archive.

The project consists of Python scripts only, is not packaged,
and the repository comes with no setup documentation.

After I have figured out which dependencies the project needs,
I wanted to add a minimal documentation, so the next user has an easier start.

So, I created a `README.rst`, added some information... and huh,
how would I render the file locally to make sure I have not messed up the syntax?

### IDE / Editor

While I remember that PyCharm has a builtin rst renderer, I no longer use it.

I have been using VS Code for a couple of years now,
but it has no builtin rst renderer,
and let's say... I had issues with the one popular rst plugin.

### Some more options

Setting up Sphinx, which I usually use for documentation,
would have been an overkill for this single page.

Usually, when I update a `README.rst`,
there is also a `setup.py` included in the project.

This way I am able to `zest.releaser` or more precisely,
its `longtest` script, which renders the `README.rst` in the browser.
But not without a `setup.py`.

### Standalone solution

Finally, I came across [restview](https://pypi.org/project/restview/) by
[Marius Gedminas](https://twitter.com/mgedmin).

Instead of following the installation instructions
(`sudo apt-get install python-pip && sudo pip install restview`),
I used [pipx](https://pypi.org/project/pipx/) to install the package.

```bash
pipx install restview
```

In order to render the document in the browser,
you can simply run the following command:

```bash
restview README.rst
```

The browser window even updates when you edit your document! Awesome!

### Thanks!

Thank you, Marius, for creating this tool!

Took me only 17 years to find it :-)
