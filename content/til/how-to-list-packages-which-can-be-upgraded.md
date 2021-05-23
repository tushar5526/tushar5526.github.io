---
title: "How to List Packages Which Can Be Upgraded"
date: 2020-07-13T13:04:41+02:00
tags:
- ubuntu
- apt
---

Have you ever logged into your Ubuntu machine and you get greeted with `xxx packages can be updated`?

Do you want to know which packages can be updated before running `apt upgrade`?

This is for you (and me):

```bash
apt list --upgradable
```
