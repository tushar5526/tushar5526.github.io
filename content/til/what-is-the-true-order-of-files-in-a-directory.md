---
title: "What Is the True Order of Files in a Directory"
date: 2021-06-09T21:54:08+02:00
tags:
- linux
- bash
---

This week I faced a very odd bug at work.

A build process failed - on one machine, but not on the other.

Of course - identical setup.

The build script consists of a bash script, which wraps a call to the C binary.

The bash script was innocent.

When I had a closer look at the C source code,
it turned out there were about 30 lines of code,
just dealing with traversing directories and copying files and folders.

As I usually don't do a lot of C,
I asked a colleague to have a look and finally we figured out what is going wrong:

The C code was reading the directory layout at a very low level,
and the files/subdirectories were actually not sorted alphabetically,
but in the order they had been written into the so-called `directory-index`.

I even did not know this existed!

Having a closer look at the 30 lines of C code,
we noticed that the recursion had a bug,
as it relied on the order of the files and sub-folders in the directory.

Ok, but how can you find out the "real" order of files in a directory?

Turns out there is an option for `ls`.

Have a look at these two (unrelated) file listings:

```bash
❯ ls -la
total 28
drwxr-xr-x  2 jugmac00 jugmac00 4096 Jun  8 09:52 .
drwxr-xr-x 20 jugmac00 jugmac00 4096 Jun  8 09:53 ..
-rw-rw-r--  1 jugmac00 jugmac00 1949 Mai  9 15:49 conf.py
-rw-rw-r--  1 jugmac00 jugmac00 7526 Mai  9 10:58 index.rst
-rw-rw-r--  1 jugmac00 jugmac00   29 Mai 21 18:28 requirements.in
-rw-rw-r--  1 jugmac00 jugmac00 1247 Jun  8 09:52 requirements.txt
```

```bash
❯ ls -laU
total 28
-rw-rw-r--  1 jugmac00 jugmac00   29 Mai 21 18:28 requirements.in
drwxr-xr-x  2 jugmac00 jugmac00 4096 Jun  8 09:52 .
drwxr-xr-x 20 jugmac00 jugmac00 4096 Jun  8 09:53 ..
-rw-rw-r--  1 jugmac00 jugmac00 1949 Mai  9 15:49 conf.py
-rw-rw-r--  1 jugmac00 jugmac00 1247 Jun  8 09:52 requirements.txt
-rw-rw-r--  1 jugmac00 jugmac00 7526 Mai  9 10:58 index.rst
```

By the way - I never had to do this in Python,
but I'd probably use `shutil.copytree` - what about you?
