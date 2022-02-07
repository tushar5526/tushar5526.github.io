---
title: "How to Create a Host Dependent Bash Configuration"
date: 2022-02-07T15:36:44+01:00
tags:
- bash
- lxd
---

I use LXD containers to develop locally.

To ease development, I share my home directory with the containers.

This is convenient, but brings along a couple of issues on its own,
especially for `.bashrc` modifications, which only apply to the host.

e.g. activating bash completion for [pipx](https://github.com/pypa/pipx).

```
eval "$(register-python-argcomplete pipx)"
```

This certainly only works on my host, where the binary is on the path,
but not in my development containers.

## if - then - fi

One simple solution is to wrap the host specific parts in an if block:

```
if [ "$HOSTNAME" = "my-hostname" ]
then
    # pipx completions
    eval "$(register-python-argcomplete pipx)"
fi
```
