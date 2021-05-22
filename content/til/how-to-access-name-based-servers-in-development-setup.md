---
title: "How to Access Name Based Servers in Development Setup"
date: 2020-07-13T19:19:18+02:00
tags:
- curl
- nginx
- development
---

## The problem

You run a web application via **nginx** and you dispatch the requests based on the server name.

And maybe the **server name** header only gets set via a proxy or an URL,
which is currently or permanent not available in your dev setup.

## The solution

```bash
curl -- header "Host: example.com" http://localhost
```

## Further information

Blog post by the maintainer of curl:

https://daniel.haxx.se/blog/2018/04/05/curl-another-host/
