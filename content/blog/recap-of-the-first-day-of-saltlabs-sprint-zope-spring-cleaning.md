---
title: "Recap day one of \"Zope spring cleaning\""
date: 2019-05-09T13:14:12+02:00
---

[More infos about the sprint](https://www.meetup.com/de-DE/Zope-Sprint/events/259042717/)

- 10 local sprinters
- 2 remote sprinters (Jens Vagelpohl and Dieter Maurer)

## progress

- Steffen
  - had lots of fun with [Nonsensical Path Infos](https://github.com/zopefoundation/Zope/issues/575)
  - had a look at some of the open documentation tickets
- Jens Klein / Martin Häcker
  - created a [pull request](https://github.com/zopefoundation/zodbupdate/pull/15) to improve the zodpupdate script
  - created / extracted [plone/zodbverify](https://github.com/plone/zodbverify) which can be used to check ZODB before the Python 2 -> 3 migration
- Jeremy
  - worked on porting `recipe.testrunner` to Python 3 and had some *fun* with doctests
- Nilo
  - worked on an issue for `RestrictedPython`
  - is about fixing a regression in `ZCatalog`, where sorting does no longer work
- Jens Hinghaus
  - worked on [tempstorage](https://github.com/zopefoundation/tempstorage/pull/11)
  - found and filed a bug in ZMI when using search
- Michael
  - is about preparing the roadmap for the final release of Zope 4 (yee-haw)
  - made a ton of releases for packages from the Zope universe
  - spread rumors that the release date of **Zope 5** won't be in a very far future
- Jürgen (me)
  - found [Dieter's comment](https://github.com/zopefoundation/Products.ZCatalog/pull/73#issuecomment-490485033) about why so many Travis builds break with Python 3.8 => clear cache and restart build, did this for lots of broken builds
  - revised the subsection about "Exceptions" of the **Zope developer's guide** and found and filed a [bug](https://github.com/zopefoundation/zExceptions/issues/8) in `zExceptions`; also experienced some strange behavior with custom exceptions which inherit from **BaseException** (don't do this at home) with waitress

Also, Jens Vagelpohl and Dieter Maurer contributed a lot by adding valuable insights to some complicated discussions, reviewed a lot of pull requests... Jens also added documentation about how to daemonize Zope 4 on a systemd powered Linux box and fixed on old bug with [importing ZEXP files](https://github.com/zopefoundation/Zope/pull/599).

## Some more positive news...

Matthew Wilkes announced some [updates](https://community.plone.org/t/zope-contributor-agreement-process/8495) on the upcoming merger of the Zope into the Plone foundation, and thus the improvements on the onboarding for new developers concerning the committer agreement.

## other than that...

We have plenty of fun here. The [venue](https://twitter.com/koffij_halle) is great - the food is excellent!
Basti, the new cook, really rocks the kitchen!
