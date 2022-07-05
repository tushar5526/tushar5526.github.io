---
title: "How to Debug SSH Issues"
date: 2021-10-05T15:23:38+02:00
tags:
- ssh
---

In order to set up a development environment for [Launchpad](https://launchpad.net/launchpad),
I needed to install and run [lxd](https://linuxcontainers.org/).

After launching my first container,
I wanted to ssh into it,
but was not successful.

Basically, now you have two ways to find out what is going on.

Run the ssh command again with `-v`,
so you see more output.

```
ssh -v user@host
```

The other choice is to go into the container and start the ssh server in debug mode.

### But how do you get into into a container without SSH working?

You can use the following command:

```
lxc shell [name of container]
```

Then run `systemctl stop sshd.service` followed by a `/usr/sbin/sshd -ddd`,
which starts sshd in the foreground.

Both ways showed the problem - `authorized_keys` had no entry for my ssh user.
