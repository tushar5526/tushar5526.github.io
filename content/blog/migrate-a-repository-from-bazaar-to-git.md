---
title: "Migrate a Repository From Bazaar to Git"
date: 2021-11-05T00:05:09+02:00
tags:
- bazaar
- bzr
- git
- launchpad
- breezy
---

Bazaar is a distributed version control system (VCS), developed by Canonical.

For a long time, Bazaar was the only supported VCS on Launchpad.

Launchpad is a code hosting platform,
similar to the now prevalent GitHub,
and while open to the public,
nowadays it is mostly used by Canonical itself and many other individuals and companies
to manage the whole lifecycle of creating packages for Ubuntu and its distributions.

Since quite some time also [git](https://blog.launchpad.net/general/git-code-hosting-beta) is a supported VCS on Launchpad.

In order to ease collaboration,
let's convert a Bazaar repository to git.

## Prerequisites

In order get data out of a Bazaar repository,
you need to install a Bazaar client and an export plugin.

Without going into details,
you should install the latest version of [Breezy](https://pypi.org/project/breezy/),
which is a Bazaar client and comes with the `FastImport` plugin,
which also can export data.

```bash
$ pipx install breezy
```

## Clone the Repository

I choose [lazr.config](https://launchpad.net/lazr.config) to demonstrate the process.

Let's go to the project on [Launchpad]((https://launchpad.net/lazr.config)),
click on the code tab,
and copy the command to clone the repository:

```bash
$ brz branch lp:lazr.config
Branched 25 revisions.
```

```bash
$ cd lazr.config
```

## Interlude

Before we continue,
it is worth to have a look at the [active reviews](https://code.launchpad.net/lazr.config/+activereviews) tab of the project.

`Active reviews` are Launchpad's equivalent to GitHub's `Pull Requests`.
They can't be merged after the migration from Bazaar to Git,
so we need to decide whether we merge them now,
or politely ask the contributor to create a new merge proposal against the git repository.

## Migrate the Repository

As both Bazaar and git use a "hidden" folder to store the meta data,
we can perform the conversion in the same directory we cloned the repository into.

```bash
git init --initial-branch=main
```

We need to use the `-b` option to specify a branch name other then `master`.

```bash
$ brz fast-export -b main | git fast-import
21:33:45 Calculating the revisions to include ...
21:33:45 Starting export of 67 revisions ...
21:33:45 Adjusting path src/lazr/config/tests/test_docs.py given rename of src/lazr to lazr in revision b'barry@canonical.com-20130107011400-9fa1vo95jtuwtu6w'
21:33:45 Adjusting path src/lazr/config/NEWS.txt given rename of src/lazr to lazr in revision b'barry@canonical.com-20130110211153-1po6s4hapsvwyv4w'
21:33:45 Adjusting path src/lazr/config/tests/test_docs.py given rename of src/lazr to lazr in revision b'barry@canonical.com-20130110211153-1po6s4hapsvwyv4w'
21:33:45 Exported 67 revisions in 0:00:00
fast-import statistics:
---------------------------------------------------------------------
Alloc'd objects:       5000
Total objects:          477 (       148 duplicates                  )
      blobs  :          195 (        90 duplicates         71 deltas of        186 attempts)
      trees  :          215 (        58 duplicates        159 deltas of        200 attempts)
      commits:           67 (         0 duplicates          0 deltas of          0 attempts)
      tags   :            0 (         0 duplicates          0 deltas of          0 attempts)
Total branches:           7 (         1 loads     )
      marks:           1024 (        67 unique    )
      atoms:             50
Memory total:          2493 KiB
       pools:          2141 KiB
     objects:           351 KiB
---------------------------------------------------------------------
pack_report: getpagesize()            =       4096
pack_report: core.packedGitWindowSize = 1073741824
pack_report: core.packedGitLimit      = 35184372088832
pack_report: pack_used_ctr            =        137
pack_report: pack_mmap_calls          =         39
pack_report: pack_open_windows        =          1 /          1
pack_report: pack_mapped              =     555465 /     555465
---------------------------------------------------------------------
```

## Almost finished..

This was easy.

But wait...

```bash
$ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	deleted:    .bzrignore
	deleted:    COPYING.txt
	deleted:    HACKING.rst
	deleted:    MANIFEST.in
	deleted:    NEWS.rst
	deleted:    README.rst
	deleted:    conf.py
	deleted:    setup.cfg
	deleted:    setup.py
	deleted:    src/lazr/__init__.py
	deleted:    src/lazr/config/__init__.py
	deleted:    src/lazr/config/_config.py
	deleted:    src/lazr/config/_version.py
	deleted:    src/lazr/config/docs/__init__.py
	deleted:    src/lazr/config/docs/fixture.py
	deleted:    src/lazr/config/docs/usage.rst
	deleted:    src/lazr/config/docs/usage_fixture.py
	deleted:    src/lazr/config/interfaces.py
	deleted:    src/lazr/config/tests/__init__.py
	deleted:    src/lazr/config/tests/test_config.py
	deleted:    src/lazr/config/tests/testdata/__init__.py
	deleted:    src/lazr/config/tests/testdata/bad-invalid-name-chars.conf
	deleted:    src/lazr/config/tests/testdata/bad-invalid-name.conf
	deleted:    src/lazr/config/tests/testdata/bad-nonascii.conf
	deleted:    src/lazr/config/tests/testdata/bad-redefined-key.conf
	deleted:    src/lazr/config/tests/testdata/bad-redefined-section.conf
	deleted:    src/lazr/config/tests/testdata/bad-sectionless.conf
	deleted:    src/lazr/config/tests/testdata/base.conf
	deleted:    src/lazr/config/tests/testdata/local.conf
	deleted:    src/lazr/config/tests/testdata/master-local.conf
	deleted:    src/lazr/config/tests/testdata/master.conf
	deleted:    src/lazr/config/tests/testdata/shared.conf
	deleted:    tox.ini

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.bzr/
	.bzrignore
	COPYING.txt
	HACKING.rst
	MANIFEST.in
	NEWS.rst
	README.rst
	conf.py
	setup.cfg
	setup.py
	src/
	tox.ini
```

Help, what a mess!

## Sir git add-a-lot

I am not really sure why this happens,
but you can fix it with the following command.

```bash
$ git add -- . ':!.bzr'
$ git status
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.bzr/

nothing added to commit but untracked files present (use "git add" to track)
```

> Do not delete the `.bzr` folder yet!

## Rename .bzrignore to .gitignore

Before we continue
I recommend to rename `.bzrignore` to `.gitignore`.

While the format might not be 100% the same,
just renaming was enough most of the time.

Do not forget to add and commit this change to git.

## Upload the git Repository

Next, we need to upload the git repository back to Launchpad.

On the project's Launchpad site,
go to the **code** tab,
and then on the right click on `Configure Code`.

On the next screen select `git`.

Now you see the the command to set the git remote address.

```bash
git remote add origin git+ssh://jugmac00@git.launchpad.net/lazr.config
```

Now we can upload the repository.


```bash
git push --tags
```

## Where is my git Code?

When you go to the `code` tab of the project,
you still only see the Bazaar repository.

> Actually, you could already switch to the git repository by clicking the link on the right.

In order to change the default view,
you need to go to `Configure Code` again,
choose `git` and this time hit the `Update` button.

## git not done yet

When you clone this new repository for the first time,
you will be surprised...

```bash
$ git clone git+ssh://jugmac00@git.launchpad.net/lazr.config
Cloning into 'lazr.config'...
remote: Enumerating objects: 479, done.
remote: Counting objects: 100% (479/479), done.
remote: Compressing objects: 100% (203/203), done.
remote: Total 479 (delta 230), reused 479 (delta 230)
Receiving objects: 100% (479/479), 542.84 KiB | 2.71 MiB/s, done.
Resolving deltas: 100% (230/230), done.
warning: remote HEAD refers to nonexistent ref, unable to checkout.
```

The reason for the warning is that Launchpad's default branch name is `master`.

In order to the change the default branch name,
from the `code` tab,
under the `Other repositories` section,
I click on `lp:lazr.config`.

On the following page,
on the right side,
I click on `Change repository details`.

At the bottom I change the `Default branch`
from `refs/heads/master` to `refs/heads/main`.

Now, cloning works as intended.

## Subscriptions, anyone?

While I am here,
I check who is subscribed to the repository.

## Bazaar, anyone?

While the git repository is now the default one,
you could still push to the Bazaar repository.

There is no obvious way to delete the Bazaar repository,
but anyway there is a better way for a smooth transition.

First, let's delete all files from the Bazaar repository locally.

```bash
brz rm *
```

Then we create a `MOVED_TO_GIT` file
with the following content

```
https://code.launchpad.net/lazr.config/+git
```

... and then add and commit the note

```bash
brz add MOVED_TO_GIT
brz commit -m "Moved to git"
```

... and push it to the still existing Bazaar repository

```bash
brz push lp:lazr.config
```

The end

## Still here?

As the project's documentation is not yet on Read the Docs,
let's keep going.

As Read the Docs does not support Launchpad's webhooks,
we need to both manually import and build the documentation, but that is pretty straightforward,
so I am positive you will figure this out by yourself.
