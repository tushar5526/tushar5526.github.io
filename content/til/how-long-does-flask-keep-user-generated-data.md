---
title: "How Long Does Flask Keep User Generated Data"
date: 2020-09-04T22:06:32+02:00
tags:
- flask
---

Easy!

Either Flask persists the data into a database or a file,
or it does not.

## Wait, not so fast.

There is also
- session scoped data (e.g. storing data in a cookie or similar)
- app scoped data (e.g. store data on the `app` object)

The caveats
- session scoped data is only available for one client,
and as long the session lasts
- app scoped data is only available within a single app - usually,
e.g. with the help of `gunicorn`,
you start more than one app instance
