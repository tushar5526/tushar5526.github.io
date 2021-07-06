---
title: "How to Check Configuration for Bind"
date: 2021-07-06T07:46:26+02:00
tags:
- bind
- named
- dns
---

You probably heard of the three steps on how to debug networking issues...

>1) It's not DNS  
>2) There's no way it's DNS  
>3) It was DNS

While this is not only funny, but also true,
you can do your part to prevent such issues.

When you change the configuration for `BIND`,
you should make sure it is still working.

## check bind configuration

```bash
$ named-checkconf /path/to/named.conf
```

The path depends on your distribution.
For Ubuntu 20.04 it is `/etc/bind/named.conf`.

## check bind zone file

```no
$ named-checkzone example.net /etc/bind/example.net
```

The path and the name of the configuration *database* was chosen by you or your administrator.
