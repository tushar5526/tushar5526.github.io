---
title: "How to Setup a Cronjob for a Tool Installed via pipx"
date: 2021-08-02T10:54:16+02:00
tags:
- pipx
- cronjob
---

The company I work for finally says good-bye to [Subversion](https://subversion.apache.org/),
and while migrating to git,
we also decided to move to GitHub and no longer host the repositories inhouse.

While I think GitHub should be pretty reliable,
data loss still could happen,
so I decided we need some backup.

I use [all-repos](https://github.com/asottile/all-repos) to manage open source repositories on a daily basis.
Its `all-repos-clone` command looks like a great fit to backup the repositories to a local virtual machine.

I setup everything,
double checked that it works manually,
setup a cronjob,
and ... was surprised that it did not work.

The cronjob looked like...

```cron
0 2 * * * cd ~/all && all-repos-clone
```

The syslog reveals... not much.

```log
Aug  2 02:00:01 hades CRON[3640]: (kopia) CMD (cd ~/all && all-repos-clone)
Aug  2 02:00:01 hades CRON[3639]: (CRON) info (No MTA installed, discarding output)
```

So, I setup the cronjob to log its output and let it run every five minutes to get quick feedback.

```cron
0 2 * * * (cd ~/all && all-repos-clone) 2>&1 | logger -t all-repos
```

This time there is a hint for me...

```log
Aug  2 08:30:01 hades CRON[7482]: (kopia) CMD ((cd /home/kopia/all && all-repos-clone) 2>&1 | logger -t all-repos)
Aug  2 08:30:01 hades all-repos: /bin/sh: 1: all-repos-clone: not found
```

So, the user is correct, the command is correct... what is missing?

Well, turns out, the `cronjob` does not source the user's `.profile`,
and so `all-repos` is not available on the path.

This also means,
there are two solutions...

- hardcode the path to `all-repos` or
- amend the PATH env-var

I went with hardcoding the path.
While this seemed hacky at first,
actually this the cleanest solution.

When I come back later,
or even a colleague who never saw this setup,
it is pretty obvious what should happen here.


## Thanks

Thank you [Bernát Gábor](https://twitter.com/gjbernat) and [senpos](https://twitter.com/_senpos) for the feedback!
