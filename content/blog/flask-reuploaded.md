---
title: "Flask Reuploaded"
date: 2020-06-28T14:27:13+02:00
---

It all started with this error message:

```python
File "/Projects/xxx/venv/lib/python3.7/site-packages/flask_uploads.py", line 26, in <module>
from werkzeug import secure_filename, FileStorage
ImportError: cannot import name 'secure_filename' from 'werkzeug''
```

Trying to build my `Flask` application, the above error message was generated,
as `Werkzeug`, a very important library in the `Flask` ecosystem,
changed its API in version 1.0, back in February 2020.

This broke a lot of plugins.

![Raging Ronacher](/images/flask-uploads-ronacher.png)

[Twitter thread](https://twitter.com/mitsuhiko/status/1225758711009902592)

As my app relies on `Flask-Uploads`,
I had a look at the problem 
and finally provided a [pull request](https://github.com/maxcountryman/flask-uploads/pull/28)
to the `Flask-Uploads` repository on GitHub.

This pull request was merged,
but the maintainer deliberately chose not to release a new version to PyPI,
though I even offered my help.

You can read his final argument here
https://github.com/maxcountryman/flask-uploads/issues/29#issuecomment-614218327

The latest version on PyPi (0.2.1) is from 2016,
and that one does not work with a current Flask/Werkzeug version.

You certainly could pin `Flask-Uploads` to a GitHub commit ID.

I think this is inconvenient at least, and doing so caused me a lot of problems,
as pinning a GitHub commit ID worked on my dev machine, but Travis CI was broken.
For reasons.

While I finally managed it "somehow",
I felt a bit sad for all those developers asking for help and a new release.

![somehow](/images/flask-uploads-travis.png)

Fast forward six months, there is still no new release.

I have thought about this situation a lot.
On the one hand, the GitHub repository is kinda up to date (the test suite is broken, and some more... ),
on the other hand there are many developers out there having trouble to fix their broken app.

So, yesterday I finally decided to fork this package,
and after fixing the test suite and some more housekeeping,
I released a new version to PyPI,
most importantly with a fix to the broken `Werkzeug` import - and tons more.

As only the original maintainer has access to `Flask-Uploads` on PyPI,
I had to create a new name: [`Flask-Reuploaded`](https://pypi.org/project/Flask-Reuploaded/).

One of my main goals is to stay compatible with `Flask-Uploads`.
It is even a drop-in replacement!

This means, instead of installing `Flask-Uploads`,
you only have to install `Flask-Reuploaded` and your migration work is finished.
You do not have to change a single line of code
[as of 2021, a few necessary, backwards incompatible changes had to be made, but are documented in detail].

The package can be found at
https://pypi.org/project/Flask-Reuploaded/

And the repository lives at
https://github.com/jugmac00/flask-reuploaded

Speaking of repository...
While I have fixed the broken test suite,
set up CI and more, there is still lots to do - especially beginner friendly tasks.
If you ever wanted to contribute to open source, why not to the library you use?
There are many [open issues](https://github.com/jugmac00/flask-reuploaded/issues).

When you now think... huh, this one guy... why should I use his package?
Maybe he is too busy soon, too?

The company I work for relies on `Flask-Reuploaded` - so there is big motivation to keep it working.
Also, I love Python and open source, and I am super open for other contributors.

As a matter of fact, my dream would be a `Flask Umbrella` organization,
which the Flask plugin developers could join and share their workload more evenly.
See my proposal to the Pallets project here:
https://github.com/pallets/meta/issues/28 (As of 2021, the repo was deleted, but... there is big news coming!)

This all said...
I am super thankful to Max and all the other contributors to this and other open source packages.
I respect the decision not to release a package.
The beauty of open source is **choice**.
And now there is a choice when you want a working [PyPI package](https://pypi.org/project/Flask-Reuploaded/).

Thanks!

## Update, 2020-06-28

Exactly 20 minutes after I have posted a similar, but shorter and more specific answer to one of the
["current release is broken"](https://github.com/maxcountryman/flask-uploads/issues/33)
issues on `Flask-Uploads` bug tracker,
the maintainer of `Flask-Uploads` fired a series of actions:

- he [kindly answered](https://github.com/maxcountryman/flask-uploads/issues/33#issuecomment-650776662) to this one month old issue for the first time

- he then marked [my answer](https://github.com/maxcountryman/flask-uploads/issues/33#issuecomment-650772815) as off-topic, so it is hidden

- he started to answering / closing all open issues, dating back til 2016

- he created a [new ticket](https://github.com/maxcountryman/flask-uploads/issues/39) and asked for help to automate releases to PyPi

Guess what? This is almost exactly the reaction I anticipated.

One fork, probably four hours of work, which I enjoyed a lot,
and a very good looking solution for all users of either `Flask-Uploads` or `Flask-Reuploaded` is within reach.


### Concerning the call for help with the automated releases...

I truly hope this will work out, and the many users of `Flask-Uploads`,
which had problems for months,
and probably never get to know `Flask-Reuploaded`,
get a working app again.

And Max, if you happen to read my blog any time...

**"If someone is so motivated..."**
I was. I am no longer.
I have a different mental model of how open source works.

**"This will allow the maintainers (including myself) to easily create releases without having to setup the local tooling to do so (truly a pain and not something I personally have the time for at the moment)."**

There is an excellent blog post out there on how to do releases to PyPI, by Hynek Schlawack: [Sharing Your Labor of Love: PyPI Quick and Dirty](https://hynek.me/articles/sharing-your-labor-of-love-pypi-quick-and-dirty/)

Basically, you need a virtualenv and five minutes.
That's it.
I know it. 
[I did it](https://github.com/jugmac00/flask-reuploaded/blob/master/PACKAGING.rst).
[Twice](https://pypi.org/project/Flask-Reuploaded/).
