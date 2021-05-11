---
title: "Face to face with \"Earl Zope and his henchmen\""
date: 2019-05-03T12:48:11+02:00
---

This blog post tries to ease beginning and keeping up with the development of the open source web application server [Zope](https://zope.readthedocs.io/en/latest/) and its universe.

## what makes it hard to start contributing

- **Zope** is a very mature product, as it was launched already back in [1999](https://en.wikipedia.org/wiki/Zope)
- **Zope** underwent some major architectural changes (keyword [ZCA](http://muthukadan.net/docs/zca.html#introduction))
- **Zope** itself is relatively complex
- there is also a complex development ecosystem around it
  - [Buildout](http://www.buildout.org/en/latest/)
  - TravisCI (as of 2021, Travis is no longer used, but GHA)
  - [GitHub.com](https://github.com/zopefoundation)
  - [tox](https://tox.readthedocs.io/en/latest/) / [quick introduction](https://opensource.com/article/19/5/python-tox)
  - [zope.testrunner](https://zopetestrunner.readthedocs.io/en/latest/)
  - [Read the Docs](https://readthedocs.org/)
  - ...
- documentation is partially out of date
- there are many developers which has been contributing for years, or even more than a decade, not many new developers
- there are many **unwritten** rules

## prerequisites

**Zope** and probably the most of its related packages are licensed under the [ZPL](https://directory.fsf.org/wiki/License:ZPL-2.1).
In order to contribute to the project,
you need to sign a [contributor's agreement](http://www.zope.org/developer/becoming-a-committer.html).

## development process

Once your application has been accepted, which is a mere formality, you may to start contributing.

### where to find the source code

Zope and all its related packages, which reside under the umbrella of the Zope Foundation, are hosted at [GitHub](https://github.com/zopefoundation).

For instance, the repository for Zope is located at https://github.com/zopefoundation/Zope

### Note

> There are plans to merge the Zope Foundation into the [Plone Foundation](https://plone.org/foundation) - current status is unknown (As of 2021, the Zope Foundation is a proud member of the Plone Foundation. Source code repositories are not affected by this merger.).

### development flow

Though not 100% consistent, usually you cannot and you should not directly work on the master branch.

The recommended way is to clone the repository and work on a new branch.

Once finished, push your branch to the repository and create a pull request.
Both your branch and the pull request will get automatically tested by TravisCI (see below) (As of 2021, this is GHA).

Most repositories need both green tests and a successful review before a pull request gets merged.

Usually you will merge the branch yourself - in order to show off that you have signed the contributor agreement.

After the pull request has been merged, please delete your branch.

### Note

> If you want to implement very small changes - e.g. fixing typos - you can directly edit the file in your browser by clicking the pencil icon when viewing the file on GitHub.com. 

### tests

It is highly recommended that all code changes, bug fixes and new features should be tested by automatic tests.

Unlike most other Python projects,
**Zope/universe** hardly makes use of pytest - which can be both quite surprising and a bit of a hurdle.

**Zope/universe** favors the [zope.testrunner](https://pypi.org/project/zope.testrunner/).

### supported python versions

For **Zope 4/universe** we support Python 2.7, 3.5, 3.6, 3.7 and 3.8,
for **Zope 5** support for Python 2.7 and 3.5 was dropped.
There is certainly support for Python 3.9 (in wide parts! with the exception of [ZEO](https://github.com/zopefoundation/ZEO/issues/165)).

This roughly reflects the officially supported Python versions.

Some packages also support [PyPy](https://pypy.org/).

In order to configure the supported Python versions,
but also much more,
we developed the [meta config tool](https://github.com/zopefoundation/meta/tree/master/config).


## updates

**2021.05.10**
- update information to reflect current state
- remove section about Travis

**2019.05.03**
-  update **what makes it hard to start contributing to Zope**; also add links

**2019.05.04**
- add section about support Python versions

**2019.05.06**
- add link to a very concise tox introduction

**2019.05.08**
- update TravisCI trouble shooting section
