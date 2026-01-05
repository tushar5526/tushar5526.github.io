---
title: "Breaking Away From the Shambles of Cloud Storage"
date: 2026-01-03T11:55:52+05:30
type: "post"
description: "Self hosting immich 101"
in_search_index:  true
tags: ["immich", "self-hosting", "tailscale", "opensource"]
---

On one bright sunny day, while I was riding my bike to my friend's house, my phone fell out of my pocket. I was super tensed that all my photos would be lost, but then thanks to a good samaritan who found the phone and returned it back to me. 

Till that point, I was procastinating setting up backups for my phone, but that was the inflection point and I finally decided to checkout the pricing for the icloud storage -- and it was outrageous! 

Esentially I need to keep paying a subscription fees just to get more storage and that too for my entire life. 

I know icloud is more than just a "storage", and there are tons of extra features that such solutions provide, but paying a recurring fees for hardware made me wonder if there is another way out, and I knew people in the OSS community would have decided to say "frick you" to these corporates giants and would have probably created something helpful. 

I was right and found [Immich](https://github.com/immich-app/immich) and [NextCloud](https://github.com/nextcloud). I researched a bit more and as my primary focus was around storage and backups for photos, Immich felt like a better fit. 

As I finally had an actual problem at hand to solve, this was a good motivation for me to finally self host. Another area that I have been dwindling about for quite long. 

Back to Immich, Immich provides quite impressive iOS and Android apps, along side other helpful features like Duplicate Detection, Face recognition, photo tagging for their search and a lot more. The ones I liked the most were linking external libraries, as I had a bunch of backups of my old phones lying around in several hard disks and first class support for backups.

My first laptop served me well for 5 years and has been lying around in my close for sometime, as I didn't wanted to spend money on setting up a full blown self hosting stack - I decided to use that laptop as it had enough CPU power and RAM for my tiny use case. I added the settings to not suspend the laptop on closing the lid and disabled the Ubuntu desktop UI. 

The plan was simple, start with a very simple setup and then harden the security. For connectivity the wifi router was good enough to create a local network. 

Getting Immich up is quite simple, you just download a `docker-compose` file and then `docker compose up` it. I also had a bunch of hard disks, for which I created a permanent mount path in my server and then binded those mount paths in my Immich server container -- and that's it! 

It is really that simple. I tried connecting to the server over plain http via the iOS app and once Immich finished transcoding images, generating thumbnails etc. I had access to all my old photos!! 

OSS feels like a super power :p

Next, I wanted to make this setup a bit more secure as the traffic was being sent as plain HTTP requests and its easy for anyone with access to wifi to sniff those packets and see all my photos. 

Also, I wanted to add some firewalls in place to not expose the immich server to the entire local network. 


For SSL, I initally thought of setting up self signed SSL certificates locally and use them to enable SSL communications, however, there was a better solution in place - Tailscale. 

Tailscale is an awesome piece of software that allows you to create a VPN network and provides you a secure way to expose your applications. TLS was solved out of the box by tailscale for me and it also provided a domain name that I can use even if the wifi router rotates the local IP. (I really want to understand how Tailscale works - but for now, lets not lose our focus)


From my previous jobs, I was aware that if you `expose: 3003:3003` the port, docker will create an IP table entry that will allow traffic from all hosts to that port. Even adding custom `ufw` rules to restrict the traffic does not work. 

One good solution to solve this is to bind the port to your local IP:

```
expose: 127.0.0.1:3003:3003
```

which will still allow tailscale to connect to `127.0.0.1:3003` but other machines on the local network would not be able to access it. 


With that done, I had my self hosted photo backup and storage solution in place and I was pretty happy with how it was working. I also added my sister to my tailscale network and she was able to offload some big videos from her phone to immich to free up some space before her next trip.

Next I would like to experiment with using CEPH to add fault tolerance to my setup, but that is a project for the next time. 





