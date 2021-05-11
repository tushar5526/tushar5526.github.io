---
title: "Log Rotation for Python Applications - Without killing them softly"
date: 2020-04-08T14:20:11+02:00
---

A couple of days after a successful deployment of my [Zope 4](https://zope.readthedocs.io/en/latest/) application,
I noticed something worrisome.

Zope gets killed every night, shortly after midnight.

I use [Supervisor](http://supervisord.org/),
which monitors the process and restarts it, so that's not the problem.

But there are two problems:
- I did not trigger this on purpose.
- The cache of my app is cold,
that means that my colleagues get slow response times for the first queries in the morning.

## What's wrong?

Thanks to version controlled [configuration management](https://batou.readthedocs.io/en/latest/) it was not hard to pick up a trail of the problem.

The commit which looked most promising was one about introducing proper log rotation with `Logrotate`.

`Logrotate` manages automatic rotation and compression of your log files.

Log rotation was setup to send a signal to Zope to close and re-open the log file as following:

```logrotate
/srv/app/deployment/work/zope/var/log/instance*.log  {

postrotate
kill -USR2 $(cat /srv/app/deployment/work/zope/var/instance/Z4.pid)
endscript
}
```

`kill` sounds scary at first, but it isn't, or let's say, it shouldn't be.

It just sends a signal to a process - 
and `USR2` happens to be a signal for which your application can use a customized reaction,
e.g. log close and re-open.

Looking at Zope's documentation,
this is exactly how it should look like and it definitely used to work this way back then with Zope 2.
No mentions in the Zope documentation about a change.

```documentation
SIGUSR2
  close and re-open all Zope log files (z2.log, event log, detailed log.) The
  common idiom after rotating Zope log files is::

    kill -USR2 `cat $ZOPE_HOME/var/Z2.pid`
```

This excerpt was available under https://zope.readthedocs.io/en/latest/INSTALL.html - meanwhile it was updated.

So, this looks like a Zope bug, eh?

Minimal setup to reproduce it... yep, Zope dies.

Some back and forth between Zope's and Waitress' (WSGI server) maintainers about who should fix this,
and... there is nothing to fix.

**Zope decided to drop signal handling** with the 2 to 4 update,
and Waitress can't do it as it does not handle log files.

## Excuse me, sir. What is the real problem here?

When `Logrotate` archives the current log file, and creates a new one,
the current app's process keeps the handle to the original file,
and keeps logging into it, although there is a new one.

Usually, you can restart your app or send a special signal,
like USR2 to your app to fix this problem, but as outlined above,
both options do not work out great with my use case.

## So, what to do?

Turns out, there are many options!

- log to a supervising process
- use `Logrotate`'s [`copytruncate`](https://jlk.fjfi.cvut.cz/arch/manpages/man/logrotate.8#Files_and_Folders) directive 
- use Python's [`WatchedFileHandler`](https://docs.python.org/3/library/logging.handlers.html#watchedfilehandler)

Having used Python for more than a decade,
I still get surprised by gems in the standard library I never heard of before.
Just like the `WatchedFileHandler`.

While all three options are valid,
the `WatchedFileHandler` just does what is needed here.
Watch the file for a change, then close the old file stream and open a new one.

The final "configuration" of `Logrotate` is this simple:

```configuration
/srv/app/deployment/work/zope/var/log/*.log  {

}
``` 
