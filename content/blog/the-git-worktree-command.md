---
title: "A Special Tool for Special Occasions: The Git Worktree Command"
date: 2023-01-19T07:16:27+01:00
tags:
- git
- worktree
---

## What is the issue?

Working with branches is fine,
as long as you are able to work on one only,
or at least have the time to wrap up your work
and commit some self-contained
and complete state before switching to the next branch.

If you cannot complete your work, it gets messy.

Now, you can either
- use `git stash`
- create a work-in-progress(WIP) commit
- just keep the modified or new files, and try to work around them and to not commit them by accident
- `git clone` your repository into another directory

I have done all of them, and they all have their downsides.

## What is `git worktree`?

Imagine `git branch` on steroids.

With `git worktree` you can checkout or create a new branch
**into a new directory**.

This means all your changes to one branch are completely isolated from your work on other branches,
all your modified files, and especially also the untracked files.

But... isn't this the same as cloning the repository into another directory?

It is not! **git** is fully aware of all existing **worktrees**.

Amongst others, this means that remotes are already set,
and **git** offers some handy commands to work with **worktrees**.

## Enough `git worktree` (sub-)commands to be dangerous

### Create a new worktree

```
git worktree add -b <branch_name> ../<some_directory> main
```

This command
- creates a new branch with the name `<branch_name>`
- in a directory one level above called `<some_directory>`
- based of the `main` branch

### List all worktrees

```
git worktree list
```

### Switch between worktrees

As **worktrees** are directories, you can just use your usual tooling,
e.g. `cd` or `pushd` and `popd`.

### Delete a worktree

```
git remove <directory_name>
```

## Every light has its shadow

After using `git worktree` for a couple of days, I have come across a couple of downsides.

VS Code offers no [native support](https://github.com/microsoft/vscode/issues/68038),
this means it does not pick up your virtual environment, as that stays in the "main location".

Also some of my plugins do not work correctly, e.g. the **Spell Checker** plugin,
as it does not share the word list between the directories.

For one project I need to use a compound test command,
which is cumbersome to create when I need to use a longer path to the "main location".

I created a bash alias, but ran into another issue as
[strace](https://unix.stackexchange.com/questions/156103) does not work with aliases.

## Conclusion

Even only after using it for a couple of days,
`git worktree` has become an invaluable command in my tool box.

Given the downsides, I still try to do most of my work inside the "main location",
but I am really happy to know how to avoid a mess in my checkout when it is necessary.

## Thanks

Thanks to my colleague [Colin Watson](https://www.chiark.greenend.org.uk/~cjwatson/blog/)
who introduced me to the `git worktree` command.

## Further information

https://git-scm.com/docs/git-worktree
