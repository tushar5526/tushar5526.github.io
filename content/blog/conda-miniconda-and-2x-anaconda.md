---
title: "Conda, Miniconda and 2x Anaconda"
date: 2022-05-23T15:43:35+02:00
tags:
- python
- conda
- miniconda
- anaconda
---

While I certainly heard of Conda, Miniconda, and Anaconda before,
I only had a vague idea of these terms and what is behind them.

So, let's get the terminology straight.

## Conda... what?

### Conda

[Conda](https://docs.conda.io/en/latest/) is a CLI application
which does package, dependency and environment management,
not only for Python, but also for other languages.

### Anaconda

[Anaconda](https://www.anaconda.com/products/distribution) is an open-source
Python distribution platform.

This is the all-in-one package, containing not only the Conda application,
but already many packages which you usually need for a data science project.

### Miniconda

[Miniconda](https://docs.conda.io/en/latest/miniconda.html) is a minimal
installer for conda. It only contains Python and a small number of other
useful packages.

### Anaconda... again

[Anaconda Inc.](https://www.anaconda.com/about-us) is the company behind all
of that.

## Conda basics

### Install miniconda

On Ubuntu 20.04 I just executed the following command(s):

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && bash Miniconda3-latest-Linux-x86_64.sh
```

But please have a look at the
[installation instructions](https://docs.conda.io/en/latest/miniconda.html)
for your operating system.

### Create a new project

```bash
conda create --name myproject scikit-learn pandas python=3.8
```

Please note that we do not only install Python packages,
but also Python itself, with the same command!

### Activate project

```bash
conda activate myproject
```

### Deactivate project

```bash
conda deactivate
```

### Install an additional dependency

```text
conda install tox
...
...
PackagesNotFoundError: The following packages are not available from current channels:

  - tox
...
...
To search for alternate channels that may provide the conda package you're
looking for, navigate to

    https://anaconda.org

and use the search bar at the top of the page.
```

Interesting!
So there is not the one package index, comparable to PyPI,
but there are several - searchable via the above mentioned website.

### Install an additional dependency from another channel

```bash
conda install tox --channel conda-forge
```

### Install an additional dependency from PyPI

```bash
pip install tox-current-env
```

Oh! This is nice!
If a package is not available on one of the special conda channels,
you can use pip as a fallback.

Though, always prefer the channels over pip,
as the packages in the channels are tested for compatibility / dependency resolution.

### Uninstall a package

```bash
conda uninstall tox
```

### Update a package

```bash
conda update tox
```

### Export an environment

```text
conda env export --file environment.yml
```

### Import an environment

```bash
conda env create --file environment.yml
```

### Packaging

What is missing?

Building your own packages.
This would go beyond the scope of this article,
so I am glad that Anaconda has
[extensive documentation](https://conda.io/projects/conda-build/en/latest/index.html)
out there.

## Conclusion

Wow!

Nice usability and smooth workflow!

Installing different Python versions, installing Python packages,
managing environments and much more...
feels a bit like that is the way Python should have been all the time, right?

## Updates

2022-05-24:
- add link to packaging tutorial
