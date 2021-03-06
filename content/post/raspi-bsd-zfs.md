---
title: "FreeBSD on RPI, ZFS tuning"
date: 2021-03-05
tags: [ snippets ]
categories: [freebsd, sysadmin]
comment: false
toc: true
draft: false
---

A fully pandemics compatible microadventure: squeeze out everything possible an RaspberryPi 4B. On this episode: tuning the ZFS storage system.

My Raspi is running: 
* in an Argon40 M.2 box, a 120Gig M.2-SSD is connected through USB3 and is bootable as "zroot". Reported as da2: 400.000MB/s transfers
* 2x 4TB disks are connected to the USB3 port - in serial connexion, one of the drives has an integrated usb-hub ;-). One of the disks is a raw ZFS, the other one is partitioned; both are mirrored to form the pool "store".

I spared some Gig on the SSD (I always do, it's a old habit). 8 Gig are swap, 2x4 Gig are added as log0 (ZIL) and cache0 (L2ARC). ZFS can only use half of the memory as ZIL/log, so 4Gig are perfect. The rest can go to the L2ARC cache - in my case it's 4Gig, too. Perfomance can be seriously degraded if these are not properly 4k block aligned.

```gpart add -t freebsd-zfs -a 4k -l log0 -s 4G da2```

```gpart add -t freebsd-zfs -a 4k -l cache0 da2```

add them to the pool "store":

```zpool add store log gpt/log0```

```zpool add store cache gpt/cache0```

benchmarking:

```
#zroot (single ssd) write
❯ dd if=/dev/zero of=test bs=1024000 count=10240
10485760000 bytes transferred in 37.165416 secs (282137568 bytes/sec)
282,14 MB/s

#zroot (single ssd) read
❯ dd if=test of=/dev/null bs=1024000
10485760000 bytes transferred in 32.209054 secs (325553184 bytes/sec)
325,55 MB/s

#store (mirrored usb, log & cache) write
❯ dd if=/dev/zero of=test bs=1024000 count=10240
10485760000 bytes transferred in 35.500731 secs (295367442 bytes/sec)
295,37 MB/s

#store (mirrored usb, log & cache) read
❯ dd if=test of=/dev/null bs=1024000
10485760000 bytes transferred in 43.287499 secs (242235291 bytes/sec)
242,24 MB/s
```

Interesting: write on the external USB drives is a bit faster, because they are mirrored.