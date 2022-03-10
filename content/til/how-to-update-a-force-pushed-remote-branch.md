---
title: "How to Update a Force Pushed Remote Branch"
date: 2022-03-10T16:53:57+01:00
tags:
- git
---

Imagine your colleague works on a new feature,
and you have checked out their branch for a local review,
and after an initial round of feedback and fixes,
your colleague performs a `git push --force` to the remote branch.

When you just do a `git pull`,
you'll end up in a mess like this...

```bash
$ git pull
remote: Enumerating objects: 19, done.
remote: Counting objects: 100% (19/19), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 11 (delta 8), reused 0 (delta 0)
Unpacking objects: 100% (11/11), 3.09 KiB | 263.00 KiB/s, done.
From git+ssh://git.example.net/repository
 + b7206f8...59c6d76 branch-name -> remote-name/branch-name  (forced update)
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint: 
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint: 
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
```

I was today year's old when I realized I can just do a ...

```bash
git reset --hard remote-name/branch-name
```

... in order to update my local checkout.

### Thanks

... to my colleague [Guruprasad](https://github.com/lgp171188)!
