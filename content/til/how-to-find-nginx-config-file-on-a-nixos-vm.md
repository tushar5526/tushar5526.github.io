---
title: "How to Find Nginx Config File on a Nixos Vm"
date: 2020-07-15T09:09:41+02:00
tags:
- nginx
- nixos
---

Nixos... hm, a dream for my web hosting company,
but a nightmare as a casual user :-D

Compared to other Linux distributions,
there are so many differences.

For a start... where the heck is my `nginx.conf`?
Hint - it is not in `/etc/nginx/nginx.conf`

```bash
systemctl cat nginx

...
X-CheckConfigCmd=/nix/store/xxx-nginx-1.14.2/bin/nginx -t -c /nix/store/xxx-nginx.conf -p /var/spool/nginx
X-ConfigFile=/nix/store/xxx-nginx.conf
```

In order to show the configuration, FlyingCircus (my hosting company) created a custom command

```bash
nginx-show-config | less
...
```

This outputs the combined nginx configuration.
