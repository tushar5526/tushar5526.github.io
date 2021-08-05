---
title: "How to Make Subversion Repositories Read Only"
date: 2021-08-05T10:25:15+02:00
tags:
- subversion
- svn
- apache
---

Finally, at work all other teams gave up [Subversion](https://subversion.apache.org/)
and migrated to [git](https://git-scm.com/).

The migration went pretty smooth thanks to `git svn` and a couple of bash scripts,
which made sure tags and branches were transferred correctly.

After the migration had been finished,
I wanted to shutdown the inherited Subversion server for good - but,
I was told to keep it running in read-only mode - for reasons.

Ok, can't be too hard, right?

A quick search in my favorite search engine revealed some promising hits,
e.g. https://stackoverflow.com/a/2411347/672833.

```ini
# give everyone read-only access to the entire repository
[reponame:/]
* = r
```

Though, there was something different.
All the solutions I found on the web were about configuring Subversion itself,
but on our Subversion server access was managed via Apache,
via `Limit`, `LimitExcept` and a bunch of methods/HTTP-verbs I never saw before,
e.g. `PROPFIND`, `REPORT`...

Thanks to the extensive Apache configuration I quickly found out
that `Limit` limits access and `LimitExcept` removes the restriction for the configured methods.
Dead simple - except - what the hell is `PROPFIND` and co and how do these methods map to actions in Subversion?

I directly searched for these verbs and found [one page (German)](https://svnbook.red-bean.com/en/1.8/svn.serverconfig.httpd.html),
which showed an example on how to restrict write access - bingo!

```apacheconf
# Authorization: Authenticated users only for non-read-only
#                (write) operations; allow anonymous reads
<LimitExcept GET PROPFIND OPTIONS REPORT>
  Require valid-user
</LimitExcept>
```

Though, again, there was no explanation about what the methods mean.

Usually, I really like to dig deep and fully understand what I am doing,
but here we had an ancient technology,
combined with a webserver which I do not use...

So I called it a day and updated the configuration...

```apacheconf
# deny write access for all
<LimitExcept GET PROPFIND OPTIONS REPORT>
  Require all denied
</LimitExcept>
```

As you can see, I only allowed an exception for - nobody.

After I applied the changes,
I verified both the working read-access and the no longer working write-access manually.

If you know more about this topic,
or a better solution,
do not hesitate to contact me.
