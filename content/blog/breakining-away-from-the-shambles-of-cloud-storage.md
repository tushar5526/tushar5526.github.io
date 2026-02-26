---
title: "Breaking Away From the Shambles of Cloud Storage"
date: 2026-01-03T11:55:52+05:30
type: "post"
description: "Self hosting immich 101"
in_search_index:  true
tags: ["immich", "self-hosting", "tailscale", "opensource"]
---

One bright sunny day, while I was riding my bike to my friend's house, my phone slipped out of my pocket. I was super tense by the thought of losing all my photos (I like to click a lot of photos). Thankfully, a good samaritan found the phone and returned it to me.

Until that point, I had been procrastinating setting up backups for my phone, but that incident was the inflection point. I finally decided to check out the pricing for [iCloud](https://www.apple.com/icloud/) storage -- and it was outrageous!

Essentially, I would need to keep paying a subscription fee just to get more storage -- and that too for my entire life. (Someone bring back SD cards)

I know iCloud is more than just "storage," and there are tons of extra features that such solutions provide including "Fault Tolerance" (but really when was the last time your hard disk crashed?). Paying a recurring fee for hardware made me wonder if there was another way out. I was pretty sure that people in the OSS community would have decided to say "frick you" to these corporate giants and must have probably created something helpful.

I was right and found [Immich](https://github.com/immich-app/immich) and [Nextcloud](https://github.com/nextcloud/server). I researched a bit more, and since my primary focus was on storage and backups for photos, Immich felt like a better fit.

As I finally had an actual problem at hand to solve, this was good motivation for me to finally self-host something of my own.

Back to Immich -- it provides quite impressive [iOS](https://apps.apple.com/us/app/immich/id1613945652) and [Android](https://play.google.com/store/apps/details?id=app.alextran.immich) apps, alongside other helpful features like duplicate detection, face recognition, photo tagging for search, and a lot more. The features I liked most were linking external libraries (as I had a bunch of backups from my old phones lying around on several hard disks) and first-class support for backups.

My first laptop served me well for 5 years and had been lying around in my closet for some time. Since I didn't want to spend money on setting up a full-blown self-hosting stack (thanks to AI, compute prices have jumped 10x), I decided to use that laptop. It had enough CPU power and RAM for my tiny use case. I [configured the settings](https://askubuntu.com/questions/15520/how-can-i-tell-ubuntu-to-do-nothing-when-i-close-my-laptop-lid) to not suspend the laptop when closing the lid and disabled the Ubuntu desktop UI.

The plan was simple: start with a very basic setup and then harden the security. For connectivity, the wifi router was good enough to create a local network.

Getting Immich up and running is quite [simple](https://docs.immich.app/overview/quick-start/); you just download a docker-compose file and then `docker compose up` it. I also had a bunch of hard disks, for which I [created permanent mount paths](https://serveracademy.com/courses/linux-server-administration/creating-permanent-mounts-with-etc-fstab/) on my server and then bound those mount paths in my Immich server container, and that's it!

It really is that simple. I tried connecting to the server over plain HTTP via the iOS app, and once Immich finished transcoding images and generating thumbnails, I had access to all my old photos!

OSS feels like a superpower. :)

Next, I wanted to make this setup more secure, as the traffic was being sent as plain HTTP requests and it's easy for anyone with access to the wifi network to sniff those packets and see all my photos.

Also, I wanted to add some firewall rules to prevent exposing the Immich server to the entire local network.

For SSL, I initially thought of setting up self-signed SSL certificates locally and using them to enable SSL communications. However, there was a better solution in place -- [Tailscale](https://tailscale.com/).

Tailscale is an awesome piece of software that allows you to create a VPN network and provides a secure way to expose your applications. TLS was solved out of the box by Tailscale for me, and it also provided a domain name that I could use even if the wifi router rotates the local IP. (I really want to understand how Tailscale works -- but for now, let's not lose our focus.)

In my previous jobs, I was bit by the famous [docker bug](https://github.com/docker/for-linux/issues/690). So, you should always expose your services to the localhost when working with docker.

```
# bind localhost's 3003 to immich:3003
ports: 127.0.0.1:3003:3003
```

This will still allow Tailscale to connect to `127.0.0.1:3003`, but other machines on the local network won't be able to access it.

With that done, I had my self-hosted photo backup and storage solution in place, and I was pretty happy with how it was working. I also added my sister to my Tailscale network, and she was able to offload some large videos from her phone to Immich to free up space before her next trip.

Next, I would like to experiment with using [Ceph](https://ceph.io/) to add fault tolerance to my setup, but that's a project for another time.
