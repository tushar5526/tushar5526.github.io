---
title: "it's never too late to join the party!"
date: 2019-05-02T11:50:14+02:00
---

Although I started writing code already at the age of 13 (**generation C64**),
and I am a professional web and backend developer since 2007,
I only started contributing to **open source software** quite recently - apart from the occasional bug report.

## what is open source software?

Quote from [Wikipedia](https://en.wikipedia.org/wiki/Open-source_software):

> Open-source software (OSS) is a type of computer software in which source code is released under a license in which the copyright holder grants users the rights to study, change, and distribute the software to anyone and for any purpose.

### interesting tidbit

The term was coined by Tim O'Reilly, Eric Raymon and Bruce Perens in **1998** - that's just 20 years!

## why care about open source software?

The company, I currently work for, makes heave use of open source software - maybe not right from the beginning,
as the company predates the term open source software,
but at least from the very start of the **internet-age**.

### open source software in use (excerpt)

- Drupal / WordPress
- Roundcube / Dovecot / Postfix
- Apache / nginx
- PHP / Python
- Linux
- and many more...

## why would you not contribute to open source software?

- work and family related time constraints
- money - at least at work

## turning point

- among others, I am maintaining a 14 year old custom intranet application built on a Zope 2 / Python 2 stack
- Python 2 support ends [01. January 2020](https://pythonclock.org/)
- that means I have to migrate
  - the application server (Zope 2)
  - the application itself
  - the database (ZODB, a NoSQL database before the name was coined)
- resources - a team of one -> **mission impossible**!

## ... but ... Zope is open source software!

- literally: the source code is **open** - available for everybody to read and modify, so no legal restrictions
- but that does not mean there is no business interest/money involved

Despite its ~~age~~ maturity **Zope** is still widely used,
indirectly by the very popular and rock stable CMS [Plone](https://plone.org/),
but also for big custom applications, for instance **union.cms**,
which gets used by [ver.di](https://www.verdi.de/) and [DGB](https://de.wikipedia.org/wiki/Deutscher_Gewerkschaftsbund).

That means that there is a broad interest of companies and unions alike to keep Zope going.

Focussing on the core competence, it is not always the best idea to employ software engineers yourself - at least not to build the basis technology.

Luckily, there are quite some consultants out there working with the Zope / Plone universe,
e.g. [**gocept gmbh & co. kg**](https://gocept.com/),
which get hired by companies and the mentioned unions, in order to work on Zope.
**gocept** on the other hand also has great interest in **Zope's** future,
and therefore also heavily contributes back to the **Zope** universe,
both with code contributions, but also by hosting so called **sprints**.

Sounds like a win win win situation!

### sprint what?

A sprint is a fixed period of time during all kind of software developers come together to work on a certain project. In this case, the [migration of Zope from Python 2.7 to Python 3](https://www.meetup.com/de-DE/Zope-Sprint/).

### history of Zope sprints @gocept

- **May 2019** [Saltlabs sprint - Zope spring cleaning](https://www.meetup.com/de-DE/Zope-Sprint/events/259042717/) - next week! looking forward to participating a lot!
- **October 2018** [Saltlabs sprint – Zope and Plone together](https://www.meetup.com/de-DE/Zope-Sprint/events/252468356/) - 31 participants! both from Zope and Plone community - tons of fun - started revising the Zope Developer book - got a t-shirts from taking part at [Hacktoberfest 2018](https://hacktoberfest.digitalocean.com/)
- **May 2018** [Zope 4 Welcome Sprint](https://www.meetup.com/de-DE/Zope-Sprint/events/248187180/) - woot, my first sprint! paired a lot with Steffen and Daniel
- **September 2017** [Zope 4 Phoenix Sprint](https://www.meetup.com/de-DE/Zope-Sprint/events/240315699/) - my back then colleague took part - hi Gabe!
- **May 2017** [Zope 2 Resurrection Sprint](https://www.meetup.com/de-DE/Zope-Sprint/events/237672812/)
- **May 2016** [Zope Resurrection Sprint](https://www.meetup.com/de-DE/Zope-Sprint/events/232466542/)

## open source is fun and you'll learn a lot!

- new tools
  - travis
  - tox
  - sphinx
  - ...
- best practices
- different coding styles
- software architecture
- collaboration
- better utilization of GitHub and all its features
- ...

... but also historical facts ...

`src/ZPublisher/HTTPResponse.py`

```python
# Note that as of May 98, IE4 ignores cookies with
# quoted cookie attr values, so only the value part
# of name=value pairs may be quoted.
```

## one last thing

Contributing to open source software should not exclusively be at the expense of your free time!

Ask your boss - today! Eventually, you'll be surprised - so was I.

## notes

I presented this topic as a lightning talk at [Mobile Stammtisch/GDG Regensburg](https://www.meetup.com/de-DE/Mobile-Stammtisch-GDG-Regensburg/events/257156080/) on 20. December 2018.
