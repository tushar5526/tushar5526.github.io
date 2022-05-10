---
title: "How to Delete Documentation Hosted on pythonhosted.org"
date: 2022-05-10T16:29:54+02:00
tags:
- documentation
- psf
- pythonhosted
---

Today, I needed to lookup something in the documentation of `lazr.config`,
which - you guessed it - is a configuration system,
using an ini-like configuration file format.

The number one search result on Google is still
https://pythonhosted.org/lazr.config/.

For projects on available on PyPI both wheels and packages are hosted on
pythonhosted.org, and as it looks, at least before "my time" and before there
was Read the Docs, also documentation was hosted on pythonhosted.org -
and still is - or was - for https://pythonhosted.org/lazr.config/.

### The problem

We have moved the documentation to Read the Docs,
and nobody in our team remembers who created the documentation on pythonhosted.org,
and how you could access the settings, muss less how to delete the it.

### The solution

Thanks to [Marc-André Lemburg](https://discuss.python.org/t/deletion-request-for-documentation-hosted-on-pythonhosted-org/15648/2)
who pointed me to a [pull request by Ee Durbin](https://github.com/pypa/warehouse/pull/3413),
who implemented a delete function - yeah!

- go to your project on PyPI, e.g. https://pypi.org/project/lazr.config/
- click on "Manage project"
- on the left, click on "Documentation"
- click on "Destroy Documentation for project"
- in the popup, enter your project name and click on "Destroy Documentation for project"
