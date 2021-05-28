---
title: "How to Delete Anonymous Docker Volumes on Tear Down"
date: 2021-05-28T13:10:08+02:00
tags:
- docker
- docker-compose
---

On one of our servers, there is a demo application installed for our customers.

The application,
which consists of two docker images,
is managed by a `docker-compose.yml` file.

As the customers are able to change the data,
the containers and the attached volume are torn down every night,
and then rebuilt.

Monitoring made me aware,
that we are running low on disk space.

Turns out the volumes do not get deleted with the current cronjob:

```bash
docker-compose down && docker-compose up -d
```

As a quick fix, I ran a `docker volumes ls`, followed by a `docker volume prune`.

This deletes all unused local volumes.

In order to use this as an unattended command,
you need to append the `-f` flag,
otherwise you get prompted for confirmation.

As I am pretty new to Docker land,
I looked around for other solutions.

One [answer on StackOverflow](https://stackoverflow.com/a/57726105/672833) suggests to use `docker-compose -rm -fsv`,
where
- `f` stands for `force`, ie no confirmation necessary
- `s` stands for `stop` the containers, if necessary
- `v` removes anonymous volumes, attached to the containers


At the end, I settled for `docker-compose down -v`,
where
- `v` removes both named, and anonymous volumes, attached to the container

So my new cronjob looks like
```bash
docker-compose down -v && docker-compose up -d
```

I keep my fingers crossed - tomorrow I will now whether this worked out :-)
