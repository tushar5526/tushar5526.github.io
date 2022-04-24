---
title: "How to Combine PDF Files on the Command Line"
date: 2022-04-24T21:44:43+02:00
tag:
- ubuntu
- linux
- pdf
- combine
---

Without further ado...

```bash
pdfunite in-file-1.pdf in-file-2.pdf outfile.pdf
```

Simple as that.

## How to get pdfunite?

I had it already installed as a dependency when I installed
[calibre](https://github.com/kovidgoyal/calibre), but you can also install it
via

```bash
sudo apt install poppler-utils 
```

[poppler-utils](https://en.wikipedia.org/wiki/Poppler_(software)#poppler-utils)
is a collection of PDF utils.
