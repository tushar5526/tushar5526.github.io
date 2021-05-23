---
title: "How to Setup Zope in Dev Mode"
date: 2020-09-29T13:40:28+02:00
tags:
- zope
---

# How to setup Zope in development mode?

```bash
git clone git@github.com:zopefoundation/Zope.git

cd Zope

python3.8 -m venv .venv

. .venv/bin/activate

pip install -U pip

pip install -e .[wsgi] -c https://zopefoundation.github.io/Zope/releases/master/constraints.txt
```

... where
- `-e` means *editable*, ie. developer mode
- the dot in `.[wsgi]` means here (similar to `cd ..` in bash) and `[wsgi]` means install the wsgi extras, which turns out to be `Paste`
- `-c` adds a constraints file, ie the versions of dependencies get pinned there

After the installation, you need to configure `Zope` before you can run it, cf https://zope.readthedocs.io/en/latest/operation.html

```bash
.venv/bin/mkwsgiinstance -d .

# provide username and password
```

Run `Zope`:

```bash
.venv/bin/runwsgi -v etc/zope.ini
```

