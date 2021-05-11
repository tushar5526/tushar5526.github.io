---
title: "Recap Day Two of Zope Spring Cleaning"
date: 2019-05-10T13:22:15+02:00
---

Very brief recap, as I want to spend the remaining time with some more documentation updates.

## progress

- Jens Hinghaus
  - [Fix bug in ZMI search](https://github.com/zopefoundation/Zope/pull/605)
  - [improve tempstorage fix](https://github.com/zopefoundation/tempstorage/pull/11)
- Steffen
  - finished the documentation for the [Nonsensical path issue](https://github.com/zopefoundation/Zope/issues/575)
  - had some fun with getting [AppVeyor](https://github.com/zopefoundation/Zope/pull/613) up and running for **Zope**
- Daniel
  - [fixed a bug in ZCatalog](https://github.com/zopefoundation/Products.ZCatalog/pull/75) (sorting did not work); migrated a DTML template to zpt
- Jeremy
  - continued his work on [zc.recipe.testrunner](https://github.com/zopefoundation/zc.recipe.testrunner/pull/3)
- Jens Klein with Martin
  - released [zodbupdate](https://pypi.org/project/zodbupdate)
  - released [zodbverify](https://pypi.org/project/zodbverify/)
  - opened a [pull request](https://github.com/zopefoundation/Zope/pull/608) for the ZODB migration story
- Michael
  - prepared a blog post for the Zope roadmap
  - prepared a blog post for the final release of Zope 4
  - released some more packages
  - is about preparing the final release of Zope 4!!!
- Jürgen
  - filed a [bug about a wrong path](https://github.com/zopefoundation/Zope/issues/602) when generating documentation locally
  - added a [warning box](https://github.com/zopefoundation/Zope/issues/568) when using **records**
  - updated [Exceptions and Transactions](file:///home/jugmac00/Projects/Zope/docs/_build/html/zdgbook/ObjectPublishing.html#exceptions-and-transactions) in the Zope developer guide


Again, we had some very active remote contributors: Jens Vagelpohl, Dieter, Marius and Jason

P.S.: I will update this blog post when I sit in the train back home... let's get Zope 4 ready to be released! (I never have and never will, as I do not know what I wanted to add :-) )

## updates

2019-05-10
- add some links to pull requests and packages
- fix my mix up of "zodbmigrate" -> "zodbverify"
