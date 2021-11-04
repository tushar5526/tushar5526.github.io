---
title: "How to Add a Menu to the Sidebar With Sphinx"
date: 2021-11-04T11:13:10+01:00
tags:
- sphinx
- navigation
- menu
- readthedocs
---

Currently I am publishing the documentation on https://readthedocs.org/ for a lot of projects.

I was asked that the menu,
which is shown at the bottom of the main page,
should be also listed in the sidebar.

You need to know that menus are `toc`s in the Sphinx world,
short for **table of content**.

## initial sitution

- one long document with code examples (`index.rst`)
- menu at the bottom (keyword: `toctree`), referencing `CONTRIBUTING.rst` and `NEWS.rst`

```rst
=======
wadllib
=======

An Application object represents a web service described by a WADL
file.

# many lines

.. toctree::

   CONTRIBUTING
   NEWS
```

## show the menu

In order to show a menu on the left (location depends on the used theme),
you need to add/update a `html_sidebars` entry in the `conf.py`,
the configuration file for Sphinx.

```python
html_sidebars = {
    '**': [
        'globaltoc.html',
    ]
}
```

This shows the global table of contents on each page.

Other [valid options](https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-html_sidebars) are:
- `searchbox.html` -> shows a search box
- `relations.html` -> shows links to the previous and next documents
- ...

This worked, but there was no link to the page itself, ie the contents of `index.rst`.

## add missing link to self

Once you clicked on one of the links,
there was no obivous way to get back to the main content.

Adding `index` worked,
but generated a couple of `circular reference` warnings.

The solution was to use `self` to reference to the same page.

This worked, but the menu looked a bit off,
as the link items were capitalized,
just as the contents of the `toc`.

## improve formatting

For the `toc` the filename is used as a title by default,
which is not always a good fit.

Luckily, you can use a special syntax to use arbitrary titles.

Just put the document name in angle brackets, and the text before.


## final menu

```rst
 .. toctree::
 
    self
    Contributing <CONTRIBUTING>
    News <NEWS>
```
