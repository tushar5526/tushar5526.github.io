---
title: "How to Grep in PDF Documents"
date: 2021-12-27T16:34:42+01:00
tags:
- pdf
- grep
---

The end of the year usually also means having some fun with the tax declaration.

One of the tedious tasks is to match payments for insurances with bank statements.

As all my bank statements are PDF documents,
I wondered how to search/grep in them.

## pdfgrep for the rescue

Turns out there is [pdfgrep](https://pdfgrep.org/),
which you might need to install via `sudo apt install pdfgrep` or similar,
depending on your operating system.

`pdfgrep` supports many of the regular regex patterns,
so you when you want to search for **kfz** (German for car) in all PDFs,
ignoring the case,
you just need to write:

```
pdfgrep -i kfz *.pdf
```
