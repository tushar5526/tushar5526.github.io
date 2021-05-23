---
title: "How to View Log Entries Using Systemd"
date: 2020-08-10T11:54:07+02:00
tags:
- systemd
- log-file
- journalctl
---

Until very recently I was lucky enough to have real log files,
but now I have to use systemd's way to view logs.

## show all log entries

```bash
journalctl
```

## show nginx' log entries

```bash
journalctl -u nginx
```

## directly jump to the latest entries

```bash
journalctl -e -u nginx
```
