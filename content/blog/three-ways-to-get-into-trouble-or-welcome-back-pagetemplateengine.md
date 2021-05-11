---
title: "Three Ways to Get Into Trouble or Welcome Back PageTemplateEngine"
date: 2019-11-26T13:58:44+02:00
---

The web application server [Zope](https://en.wikipedia.org/wiki/Zope) has a very long history which dates back to the late '90s.

Quoting [Martijn Faassen](https://twitter.com/faassen/status/1197219574711803905):

> Zope must be one of the oldest Python codebases in the world that is still used outside the standard library.

During its many years of existence the way a view gets rendered has changed.

Basically there are two supported template mechanisms:
- Document Template Markup Language (DTML)
- Zope Page Templates (ZPT)

DTML is pretty much outdated for more than a decade, but still in use for the Zope Management Interface.

ZPTs are basically XML/HTML pages in which special markup is embedded ([TAL](https://en.wikipedia.org/wiki/Template_Attribute_Language)),
which gets processed server side.

The processing is done by a template engine - this used to be the **PageTemplateEngine**.

With the release of Zope 4 this engine has been superseded by the [Chameleon](https://github.com/malthe/chameleon) template engine,
which is now activated per default.

While Chameleon offers more flexibility and better performance,
there may be reasons(TM) for using the old template engine.

## word of caution

As of using Zope 4.1, the old PageTemplateEngine currently works,
but it is deprecated and subject to be [removed anytime](https://github.com/zopefoundation/Zope/issues/104).

## activate PageTemplateEngine

True to the Python motto **"There should be one—and preferably only one—obvious way to do it."**
there is no obvious,
but three rather obscure ways to get back the old engine - at least for the ZCML apprentice.
[ZCML](https://docs.plone.org/develop/addons/components/zcml.html) stands for Zope Configuration Mark-up Language.

ZCML was introduced with Zope 3, and is used to wire together different parts of your application.

### one

Go to `src/Products/PageTemplates/configure.zcml` and comment out the configuration.

This can only be a temporary solution, as you should not modify an upstream package, e.g. Zope.

### two

Make use of [z3c.unconfigure](https://pypi.org/project/z3c.unconfigure/) and unconfigure the registration.

### three

Create an `overrides.zcml` with the following content:

```
<configure xmlns="http://namespaces.zope.org/zope"
           xmlns:zcml="http://namespaces.zope.org/zcml">

  <!-- This registration disables Chameleon templates on Zope 4. -->
  <utility
    zcml:condition="installed chameleon"
    component="zope.pagetemplate.pagetemplate.PageTemplateEngine" />

</configure>
```
