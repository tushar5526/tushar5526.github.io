---
title: "What Is the Difference Between Docker Create, Docker Start and Docker Run"
date: 2020-10-29T21:56:22+02:00
tags:
- docker
---

In order to be able to answer this question,
you have to know that there are
- Docker images
- Docker containers

Docker images are blueprints or templates.

Docker containers are the instances,
created from the images.

## docker create

creates a container from an image

## docker start

starts a container

## docker run

creates a container and starts it

### note

Trying to use the `run` command twice results in something like...

```bash
docker: Error response from daemon: Conflict. The container name "/xxx" is already in use
```

## bonus

`docker run --rm`
- creates a container
- runs it
- deletes the container on exit

... which ensures you always run a *fresh* container.
