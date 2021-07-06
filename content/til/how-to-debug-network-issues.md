---
title: "How to Debug Network Issues"
date: 2021-07-06T15:05:06+02:00
tags:
- linux
- dns
- network
- suse
---

Unlike I [stated earlier today](https://jugmac00.github.io/til/how-to-check-configuration-for-bind/)...

> It’s not DNS  
> There’s no way it’s DNS  
> It was DNS

... this time it was not DNS. But let's start from the beginning.

## two lonely OpenSUSE virtual machines

I inherited two OpenSUSE VMs from a departing colleague.

After changing passwords and cataloging what services are installed,
I tried to update the VMs... and got the following errors...

```bash
# zypper refresh
Problem retrieving files from 'Haupt-Repository (NON-OSS)'.
Download (curl) error for 'http://download.opensuse.org/tumbleweed/repo/non-oss/repodata/repomd.xml':
Error code: Connection failed
Error message: 

Please see the above error message for a hint.
Skipping repository 'Haupt-Repository (NON-OSS)' because of the above error.
```

At first I thought that maybe the repository URLs are out of date,
but they are ok, and I can access them in the browser.

## some network problems ahead

Ok, let's ping a host...

```bash
# ping www.google.de
ping: connect: Network is unreachable
```

Let's have a look whether a DNS server is configured...

```bash
cat /etc/resolv.conf

### Call "netconfig update -f" to force adjusting of /etc/resolv.conf.
nameserver 192.168.1.220
```

Looks good.

Let's see whether the DNS server can be pinged...

```bash
# ping 192.168.1.220
PING 192.168.1.220 (192.168.1.220) 56(84) bytes of data.
64 bytes from 192.168.1.220: icmp_seq=1 ttl=64 time=0.251 ms
64 bytes from 192.168.1.220: icmp_seq=2 ttl=64 time=0.247 ms
...
```

Okay, looks good.

Let's `dig` google...

```bash
# dig www.google.de

; <<>> DiG 9.16.6 <<>> www.google.de
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 31174
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: 5d52301444dda5910100000060e45814feffe9a478a3b819 (good)
;; QUESTION SECTION:
;www.google.de.			IN	A

;; ANSWER SECTION:
www.google.de.		296	IN	A	142.250.74.195

;; Query time: 4 msec
;; SERVER: 192.168.1.220#53(192.168.1.220)
;; WHEN: Tue Jul 06 15:18:12 CEST 2021
;; MSG SIZE  rcvd: 86

```

Hm, this also works...

Maybe the network is misconfigured?

```bash
2: ens192: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:50:56:9c:74:bd brd ff:ff:ff:ff:ff:ff
    altname enp11s0
    inet 192.168.1.27/24 brd 192.168.1.255 scope global ens192
       valid_lft forever preferred_lft forever
    inet6 fe80::250:56ff:fe9c:74bd/64 scope link 
       valid_lft forever preferred_lft forever
```

Looks okay, too.

> Narrator: The author starts scratching his head.

## the missing piece

I got the crucial hint from [Chaotenchaos](https://twitter.com/chaotenchaos),
in the most minimal way of communication :-)

> route -n

```bash
gaia:~ # route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker0
172.18.0.0      0.0.0.0         255.255.0.0     U     0      0        0 br-aac7f7b32b3d
172.19.0.0      0.0.0.0         255.255.0.0     U     0      0        0 br-22d0066bc0ac
192.168.1.0     0.0.0.0         255.255.255.0   U     0      0        0 ens192
```

Oh yeah, there is something missing... the default route!!

Let's add a new default gateway ...

```bash
route add default gw 192.168.1.1 ens192
```

`ping google.de` immediately worked.

## permanent solution

As you may know, `route add ...` is not a permanent change.

After a reboot or a `sudo service network restart` the changes are lost.

For OpenSUSE you need to add the configuration in `/etc/sysconfig/network/routes`,
in the form of...

```bash
# Destination    Gateway    Netmask    Interface
```

e.g.

```bash
default    192.168.1.1    -    ens192
```

Mission completed!

## almost

The second OpenSUSE VM had no `route` command installed.

Ok, let's install it quickly...
except I can't install anything as I cannot access the Internet :-/

Ok, let's add a temporary route ...

```bash
# route add default gw 192.168.1.1 ens192
```

... except there is not `route` command available :-o

`route` and a couple of other network commands are deprecated,
and most of them are replaced by the mighty `ip` command.

Instead of `route add...` I used ...

```bash
# ip route add default via 192.168.1.1
```

Also, instead of `route -n` to show all available routes,
you can use `ip route`.

Interestingly, there was no `/etc/sysconfig/network/routes` file.
Creating it and adding the default gateway worked, though.

Mission completed ... real!
