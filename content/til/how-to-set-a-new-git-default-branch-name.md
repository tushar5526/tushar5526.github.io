---
title: "How to Set a New Git Default Branch Name"
date: 2021-11-02T19:08:34+01:00
tags:
- git
---

While [GitHub's default branch name](https://github.blog/changelog/2020-10-01-the-default-branch-for-newly-created-repositories-is-now-main/)
for newly-created repositories is **main** since the end of October 2020,
what about when you create a new git repository locally?

## first things first

git made the default branch name configurable in version 2.28 and higher.

So, when you run e.g. Ubuntu 20.04 like me,
you may have an older version installed.

Usually, you can't install a newer version without updating the distribution.

But there is hope,
you can install a newer git version via a PPA (personal package archive):

```
$ sudo add-apt-repository ppa:git-core/ppa
$ sudo apt update && sudo apt upgrade
...

$ git --version
git version 2.33.1
```

Now you can set a new git default branch name,
because the default value is still `master`.

## manual approach

You always can try to remember to run the following command...

```
$ git init --initial-branch=main

# or

$ git init -b main
```

I tried hard, but I certainly forgot to specify the different default name more or less 100%.

## update the configuration

To use the new default for all new git repositories,
you can update the configuration ...

```
$ git config --global init.defaultBranch main
```

```
$ cd /tmp/
$ mkdir bla
$ cd bla/
$ git init
Initialized empty Git repository in /tmp/bla/.git/

$ git status
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```
