---
title: "Highlight All Matches in Firefox"
date: 2022-04-14T19:49:01+02:00
tags:
- firefox
---

Modern browsers are pretty similar,
so switching from Chrome to Firefox was no big issue...

... except that when I searched something on a page with `STRG + F` / `Ctrl + F`,
only the first match was highlighted.

This was a major annoyance.

## Was?

Yep. Turns out there are two ways to enable highlighting for all matches.

## Via Keyboard Shortcut

In search mode you just need to hit `Alt + A`.

Hit it again to deactivate it.

## Via Configuration Settings

In the URL bar type...

```
about:config
```

hit enter, and then set

```
findbar.highlightAll
```

to `true`.

P.S.: The keyboard shortcut also toggles the configuration settings.
