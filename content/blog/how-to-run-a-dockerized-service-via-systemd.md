---
title: "How to Run a Dockerized Service via systemd"
date: 2020-10-29T15:18:40+02:00
---

Until recently, I saw no good reason to use **Docker**,
as my [deployment tool](https://batou.readthedocs.io/en/stable/) of *choice* produces approximately identical builds,
locally on my Ubuntu laptop, on staging and on production.

But time does not stand still and especially as I have to deploy a Java application,
it was time to rethink my strategy,
as I do not want to play the **which Java runtime environment plays nicely together with which app version** game.

Fortunately, [JetBrains](https://www.jetbrains.com/)
offers [pre-built docker images](https://hub.docker.com/r/jetbrains/youtrack/)
for [YouTrack](https://www.jetbrains.com/youtrack/), 
my favorite issue tracker,
which is the app I plan to install today.

## simple is not always better

With a pre-built **Docker** image,
[running the app](https://www.jetbrains.com/help/youtrack/standalone/youtrack-docker-installation.html#run-youtrack-service) could be as simple as ...

```bash
docker run -it --name youtrack-server-instance  \
    -v {path to data directory}:/opt/youtrack/data \
    -v {path to conf directory}:/opt/youtrack/conf  \
    -v {path to logs directory}:/opt/youtrack/logs  \
    -v {path to backups directory}:/opt/youtrack/backups  \
    -p {port on host}:8080 \
    jetbrains/youtrack:{version}
```
... but it is tedious to type this long command,
and also the process would not survive a reboot of the host system.

## note

> Do you know the [difference](https://jugmac00.github.io/til/what-is-the-difference-between-docker-create-docker-start-and-docker-run/)
between `docker create`, `docker run` and `docker start`?

While we are here, let's dissect this complex command:
- `run` creates and starts a container
- `-it` provides an interactive tty, ie show output in the terminal
- `--name` gives the container a name
- `-v` maps folders between the host and the container
- `-p` maps ports between the host and the container
- `jetbrains/youtrack:{version}` is finally the docker image

## run docker container as a systemd service

Instead of manually starting the docker service,
let's use **systemd** to start it, even after a reboot.

JetBrains offers a [concise documentation](https://www.jetbrains.com/help/youtrack/standalone/run-docker-container-as-service.html) on how to do this.

## unit file

Basically, just create a unit file at `/etc/systemd/system/docker.youtrack.service`,
with the following content...

```systemd
[Unit]
Description=YouTrack Service
After=docker.service
Requires=docker.service

[Service]
TimeoutStartSec=0
Restart=always
ExecStartPre=-/usr/bin/docker exec %n stop
ExecStartPre=-/usr/bin/docker rm %n
ExecStartPre=/usr/bin/docker pull jetbrains/youtrack:<version>
ExecStart=/usr/bin/docker run --rm --name %n \
    -v <path to data directory>:/opt/youtrack/data \
    -v <path to conf directory>:/opt/youtrack/conf \
    -v <path to logs directory>:/opt/youtrack/logs \
    -v <path to backups directory>:/opt/youtrack/backups \
    -p <port on host>:8080 \
    jetbrains/youtrack:<version>

[Install]
WantedBy=default.target
```
... where, on a very high level,
- the unit section gives this service a name, and defines the prerequisites and dependencies,
- the service section configures the command to be executed,
- and the install section defines when this service should be started.

Finally, you can enable it via `systemctl enable docker.youtrack`,
so it starts after the next reboot,
and start and stop it manually via `sudo service docker.youtrack start` / `sudo service docker.youtrack stop`.

## some failures

This all worked out perfectly,
and `YouTrack` was available via my browser,
except I was worried a bit about the output of `systemctl status docker.youtrack`...

![systemd status](/images/docker-systemd.png)

Why are there two failures?

From all I knew about **Docker**,
the run command should create and start a container,
so the container should be persistent,
even after a restart.

Which would finally mean,
**systemd** should successfully stop and delete the docker container e.g. on restart.

### dissect the service section of the unit file

Let's have another look at the *Service* section from above, especially at the following two lines.

```
ExecStartPre=-/usr/bin/docker exec %n stop
ExecStartPre=-/usr/bin/docker rm %n
```

- `ExecStartPre` is pretty self explaining, and means "execute this command before starting the service"
- the dash, `-`, just before the command, means it is ok when the following command fails = do not stop on failure!
- `%n` expands to the service, ie `docker.youtrack.service`

So, this means, **failures** are expected!

Let's have a look at the `run` command...

`ExecStart=/usr/bin/docker run --rm --name %n`

- `ExecStart` is the main process
- `docker run`, as we now know, creates and starts a container
- `--rm` - here we go, this makes sure the container is deleted on exit!
- `--name` sets the name of the container
- `%n` is the placeholder for the service

## conclusion

Everything is working as expected!

The `stop` and the `delete` commands are only safe guards,
just in case another container with the same name exists,
or is even running.

Running pre-built **Docker** images is a breeze.

But you certainly should know the basics :-)
