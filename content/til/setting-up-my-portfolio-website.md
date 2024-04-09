---
title: Create files bigger than available space 😱
date: "2023-08-06T16:34:25+05:30"
type: "post"
description: "Sample Blog - Setting up a minimalistic blogging/personal website"
in_search_index: true
tags: ["website", "blogging", "portfolio"]
---

### spoiler: sparse files / holes in a file

I was recently going through the book *The Linux Programming Interface* and came across the concept of sparse files. Files are stored on multiple blocks on a disk and sparse files is a smart optimisation that skips allocating disk block if there is no data to be stored. 

You can create holes in your files by moving the file pointer `n` bytes ahead than the actual size and then write some random data to it.

TODO: add a small C snippet here on how to make sparse files. 