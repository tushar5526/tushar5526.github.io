---
title: "Multiline Commit Messages With `git commit -m"
date: 2023-02-12T10:38:09+01:00
tags:
- git
- bash
---

When you execute `git commit` on the command line,
your specified editor opens
and you can create an expressive title and a longer description.

That is probably nothing new.

You are probably also aware of the shortcut `git commit -m <message>`,
which skips the step with the editor.

But there is only room for the title, right?

Nope, this is not true.

## one way

It turns out you can use `-m` multiple times.
For the second `-m` and each additional `-m` a new paragraph will be created.

So when you enter `git commit -m MyTitle -m MyDescription`,
you will get a commit message like...

```
commit <xxx>
Author: Jürgen Gmach <xxx>
Date:   So Feb 12 09:16:52 2023 +0100

    MyTitle

    MyDescription
```

## another way

While the above way is certainly preferable most of the time,
you can also use a lesser known Bash feature,
depending on who you ask it is either called [ANSI-C Quoting](https://www.gnu.org/software/bash/manual/html_node/ANSI_002dC-Quoting.html#ANSI_002dC-Quoting),
or [shell bling strings](https://www.youtube.com/watch?v=fhhznv4E1p) :-).

```bash
git commit -m $'MyTitle\n\nMyDescription'
``` 

The way it works is, that the enclosed string gets expanded first,
as specified by the ANSI C standard.

The result is then handled as a regular single-quoted string,
as if the dollar sign had not been there in the first place.

## why

Occasionally, I use a wrapper around `git`
and there you are only able to pass in one string for the commit message.

## thanks

Thanks to [Anthony Sottile](https://twitter.com/codewithanthony) pointing me in
this direction!
