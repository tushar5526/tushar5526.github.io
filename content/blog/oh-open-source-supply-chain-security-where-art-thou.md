---
title: "Oh Open Source Supply Chain Security, Where Art Thou"
date: 2022-06-01T20:38:46+02:00
tags:
- security
- opensource
- supplychain
---

"This is horrifying. But also not surprising."

These are the words of a friend of mine,
a security specialist,
when I told him what I found out today.

But first...

## What is Open Source Supply Chain?

Most applications nowadays use open source libraries,
especially for common functionality like e.g. sending web requests,
so it is not necessary to re-invent the wheel all the time.

This is great! This saves a lot of work, time and money,
and usually when a library is widely used,
it is rock stable.

## What could possibly go wrong?

There are just too many moving parts,
so it is almost impossible to make sure the software you are using is really safe.

Think of the many different maintainers that are involved.

Do they all have good intentions?

Are their computers safe?

Are all of their accounts safe? Are they using 2FA?

Are the tools they are using really safe? e.g. coverage or CI?

And while many popular libraries are closely watched,
maybe backed by some companies,
maybe maintained by experienced software engineers,
what about their transitive dependencies?

## Transitive what?

[Seth Michael Larson](https://twitter.com/sethmlarson),
core maintainer of urllib3,
currently second [most downloaded Python package](https://pypistats.org/top),
wrote a [blog post](https://sethmlarson.dev/blog/people-in-your-software-supply-chain)
to make aware of that issue.

In a [diagram](https://sethmlarson.dev/blog/people-in-your-software-supply-chain)
he shows the [requests](https://pypi.org/project/requests/) package,
its maintainer, but also the dependencies of requests, ie. urllib3, idna and
chardet, and their maintainers.

When you depend on requests, and requests depends on urllib3,
then urllib3 is a transitive dependency,
which you need to install to use requests.

## Oh look who's there

When I had a look at the diagram,
I immediately noticed one of the maintainers of chardet,
as he is one of my "Python friends".
I do not publish the name / account until I receive his permission to do so.
Until then, you need to guess which one it is :-)

## Boom

When I asked him on Twitter whether that is him... he said "No."

![mic drop](/images/mic-drop.jpg)

But in fact he is - I checked the [maintainers list on PyPI](https://pypi.org/project/chardet/).

Having a look at the contributors of chardet on Github via
```git shortlog --summary --numbered --email```,
I had to swallow hard,
this time he is not trolling me,
he did not contribute a single line.

Let that sink in.

You have access to the PyPI account of a top 50 library,
which actually is also a dependency of a top 3 Python package,
and you are not aware of it.

There were quite some supply chain issues out there,
but currently I cannot imagine anything close to this one!

## Imagine

Last week there was a lot of buzz about someone
"hacking a popular PyPI package" called ctx,
where [PyPI administrators estimated 27.000 downloads in 10 days](https://python-security.readthedocs.io/pypi-vuln/index-2022-05-24-ctx-domain-takeover.html).

Ok, got a seat? Let's have a look at how many downloads chardet and requests have.

According to https://pepy.tech/...
- requests: 50 million per day
- chardet: 15 million per day

![This is fine meme](/images/this-is-fine.png)

## Is it safe to use open source software?

According to Dustin Ingram's 2022 PyCon US talk
[Securing the Open Source Software Supply Chain](https://www.youtube.com/watch?v=i1QqhGsbX6Y)...

Yes!

... [with a giant asterisk](https://youtu.be/i1QqhGsbX6Y?t=80) :-)

## What can we do?

I would suggest to watch both of Dustin's talks as a starter:

- [Secure Software Supply Chains for Python](https://www.youtube.com/watch?v=VWWgkF-0cDQ)
- [Securing the Open Source Software Supply Chain](https://www.youtube.com/watch?v=i1QqhGsbX6Y)

I also highly recommend Seth's blog posts:

- [Security for package maintainers](https://sethmlarson.dev/blog/security-for-package-maintainers)
- [People in your software supply chain](https://sethmlarson.dev/blog/people-in-your-software-supply-chain)

And in case you are a professional software developer,
ask yourself:

"What can my company do to make open source software even safer?"

## Pictures

- "this is fine" by [kcg](https://twitter.com/kcgreenn)
- "mic drop" from [Wikimedia](https://commons.wikimedia.org/wiki/File:Barack_Obama_Mic_Drop_2016.jpg)
