---
title: "Just Enough Bazaar to Be Dangerous"
date: 2022-07-18T12:52:38+02:00
tags:
- bazaar
- breezy
- brz
- bzr
---

Last year I wrote a blog post about how to migrate from [Bazaar](https://bazaar.canonical.com/en/),
a distributed version control system, originally developed by Canonical,
to git.

While I still think this is the only way forward,
here, at Canonical, working in the Launchpad team,
we still have a couple of Bazaar repositories around,
which cannot be easily migrated,
as they are both integrated into deployment processes,
and used by other teams.

## basic commands to get around

> Please note: [Breezy](https://www.breezy-vcs.org/) (brz) is a friendly fork
of the Bazaar (bzr) project. Nowadays, it is strongly recommended to use Breezy.

While Bazaar and git are different, the basic commands are pretty similar.
So if you know git, you will probably recognize the following commands:

### clone a repository

```bash
brz branch lp:brz
```

### update a local repository

```bash
brz pull
```

### create a local branch

```bash
brz branch lp:brz <name-of-branch>
```

> Please note: You should do this outside the cloned repository,
as this will create a new folder.

### add changes

```bash
brz add <filename>
```

### commit changes

```bash
brz commit
```

or

```bash
brz commit -m "message"
```

### push changes

```bash
brz push lp:~<my-account>/brz/<name-of-branch>
```

### create a merge proposal

-> go to the branch in your account
( `https://code.launchpad.net/~<my-account>/brz/<name-of-branch>` )
 and click on "Propose for merging"

### show log

```bash
brz log
```

> Please note: As there is no builtin pager, you probably want to use `brz log | less`

### show a single rev

show revision number 3

```bash
brz log -c3
```

... including code changes

```bash
brz log -c3 -p
```
