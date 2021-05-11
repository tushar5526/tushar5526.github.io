---
title: "Recap Day Three and Wrap Up of Zope Spring Cleaning"
date: 2019-05-10T13:27:31+02:00
---

This blog post is work in progress... much more to come - 
but the train is only 10 minutes late (uncommon in Germany :-) ),
so I am not able finish the blog post now (narrator: he never has updated and never will update this blog post).

## tl/dr - what a ride!

- today there was the final release of Zope 4! see [blog post @gocept](https://blog.gocept.com/2019/05/10/celebration-zope-4-final-release/)
- Michael / @gocept announced the [roadmap](https://blog.gocept.com/2019/05/10/zope-roadmap/) for Zope 2.13, Zope 4 and Zope 5

## progress on day 3

The main effort of everybody at the sprint was to wrap up all loose ends,
finalize what one started, and of course get Zope 4 final released!

As there was no final standup, I try to list what I picked up...

- Michael
  - finish and publish the above mentioned blog posts
  - trying to not get lost with those massive incoming GitHub updates
  - preparing and finally publishing the release of Zope 4
  - taking a deeper look into the "allowed attributes" ticket together with Steffen; a safe last minute fix was not possible
  - even found time to review some open pull requests and to give me a helping hand for a test setup
- Jeremy
  - finished enabling [support for Python 3](https://github.com/zopefoundation/zc.recipe.testrunner/pull/3) for the zc.recipe.testrunner
- Steffen
  - made up his mind about [how to improve the speed for the Windows tests](https://github.com/zopefoundation/Zope/issues/617)
- Daniel
  - [improved styling](https://github.com/zopefoundation/Products.ZCatalog/pull/77) of the `ZCatalog`
- Jens Klein
  - updated [ZODB migration story](https://github.com/zopefoundation/Zope/pull/608)
  - oh noes! announced the news that **Plone** - for some cases - can't yet use **Zope 4** - info came from Philip Bauer, cf [ticket "allowed attributes"](https://github.com/zopefoundation/Zope/pull/610)
- Jens Hinghaus
  - got his pull request merged for [fixing the ZMI search bug](https://github.com/zopefoundation/Zope/pull/605)
- Jürgen
  - update [section about XML-RPC](https://zope.readthedocs.io/en/latest/zdgbook/ObjectPublishing.html#xml-rpc) of the Zope developer's guide, which also was the last section of Chapter 4 - which I started updating a whopping 8 months ago!!
  - started working on Chapter 5 [Zope Products](https://zope.readthedocs.io/en/latest/zdgbook/Products.html)
  - ....

Again, Jens Vagelpohl, Marius and Dieter and maybe some more guys I possibly missed, supported the sprint massively from remote!

## more news

There are some more news about [merging the Zope into the Plone foundation](https://community.plone.org/t/zope-contributor-agreement-process/8495).
