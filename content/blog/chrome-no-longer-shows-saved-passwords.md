---
title: "Chrome No Longer Shows Saved Passwords"
date: 2019-05-10T13:35:01+02:00
---

Concerning security and privacy,
you either can get stalled because you can't get it perfect right from the beginning,
or you improve over time.

I prefer the latter one.
And concerning **password security** I go with Troy Hunt:
[Password managers don't have to be perfect, they just have to be better than not having one](https://www.troyhunt.com/password-managers-dont-have-to-be-perfect-they-just-have-to-be-better-than-not-having-one/).

And actually the password manager of a browser is already a password manager.
I would not use it for very important passwords,
but it is absolutely fine for those dozens of dozens forum logins and so on.
Currently, I still manage the more important passwords with [VeraCrypt](https://www.veracrypt.fr/en/Home.html) - which is not that comfortable, but it does its job.

## works fine except when it does not

So, Google Chrome's password manager works fine for me... except when it does not.

Chrome stopped showing the saved passwords - from one day to another.
I can't recall whether it was the [Fedora 28 -> 29 Upgrade](https://fedoramagazine.org/upgrading-fedora-28-fedora-29/) or maybe a Chrome update.

Anyway - it is extremely annoying to lookup passwords for every forum and and other sites which require a login.

## the easy fix

As I sync ~~everything~~ my profile with Google (hum, this sounds like something I should reevaluate some time) it is just a matter of deleting the broken profile and restarting **Chrome**.

```bash
$ cd /home/jugmac00/.config
$ rm -rf google-chrome
```

It is as simple as that.
And sure, I experience many more rough edges since I migrated to a Linux desktop,
but I always feel like I keep the control and every problem is very easily fixable.
