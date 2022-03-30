---
title: "add-apt-repository Does Not Support Private PPAs"
date: 2022-03-30T10:50:54+02:00
tags:
- ppa
- ubuntu
- apt
---

When you are a regular user of Ubuntu,
you are probably familiar with the term PPA:
Personal Package Archive

### Interlude

Ubuntu offers packages via different package repositories (
`Main`, `Universe`, `Restricted`, `Multiverse`).

Whenever you enter e.g. `apt install <package>` the mentioned repositories are queried.

Though, while Ubuntu offers many packages, not all are available via `apt`.
Also, for a given version of Ubuntu the packages usually only get security and bug fixes,
but no version updates.

So, if you need a newer version of e.g. `git`
or a package which is not available in the standard repositories,
you would need to download and install a `deb` directly (
There may be other ways available, e.g. using [Snap](https://snapcraft.io/store),
or compile from source...).

There is one huge drawback - this package won't get updates.

Let me introduce PPAs.

Imagine PPAs as additional package repositories.

PPAs offer a straightforward way to offer newer versions of packages,
or even completely new packages - with updates,
as far as the maintainer of the PPA delivers them.

Note, that those PPAs are usually not officially supported.

So you need to be trust the maintainer.

I do have trust in e.g. Anthony Sottile,
who is the maintainer of the [deadsnakes PPA](https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa),
which offers both newer and older Python versions.

In order to add this repository, I just need to enter...

```bash
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
```

### Private PPAs

This week I learned that there are also private PPAs.

Which makes sense,
as e.g. some companies offer their packages via a subscription model.

### Limited support

Unfortunately, I also learned this week,
that `add-apt-repository` [doesn't work with private PPAs](https://bugs.launchpad.net/ubuntu/+source/software-properties/+bug/645404).

You might get a similar error message when trying anyway...

```bash
# add-apt-repository ppa:username/package
Cannot add PPA: 'ppa:~username/package'.
ERROR: '~username' user or team does not exist.
```

### What can you do?

Add your vote to the [feature request](https://bugs.launchpad.net/ubuntu/+source/software-properties/+bug/645404).

You can also workaround this issue by directly updating the `sources.list`,
as there you can add the necessary credentials for authentication.

```bash
deb https://your-user:token@private-ppa.launchpadcontent.net/username/package/ubuntu focal main
deb-src https://your-user:token@private-ppa.launchpadcontent.net/username/package/ubuntu focal main
```