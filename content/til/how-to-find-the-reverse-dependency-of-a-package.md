---
title: "How to Find the Reverse Dependency of a Package"
date: 2021-07-14T15:59:27+02:00
tags:
- ubuntu
- apt
---

Today, there was a nice little discussion on [vacdec's](https://github.com/hannob/vacdec) GitHub project
about what needs to be installed to make the barcode scanner [pyzbar](https://pypi.org/project/pyzbar/) work on Ubuntu and MacOS.

The original poster proposed to install `zbar` on both operating systems.

[zbar for Ubuntu](https://packages.ubuntu.com/source/bionic/zbar) comes with a ton of dependencies.

So I had a look at my installed packages...

```bash
❯ apt list --installed | grep zbar
libzbar0/bionic,now 0.10+doc-10.1build2 amd64 [installed]
```

... which only revealed the `libzbar0` library.

As `vacdec` still works like a charm,
installing `zbar` does not seem to be necessary.

But how did `libzbar0` land on my pc?
I could not recall installing it manually.

## The Solution

```bash
❯ apt-cache rdepends --installed libzbar0
libzbar0
Reverse Depends:
  gstreamer1.0-plugins-bad
  gstreamer1.0-plugins-bad
```



