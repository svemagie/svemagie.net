---
title: "FreeBSD on RaspberryPi 4, updating from source"
date: 2021-02-27
tags: [ snippets]
categories: [sysadmin, freebsd]
comment: false
toc: false
---
A fully pandemics compatible microadventure: squeeze out everything possible an RaspberryPi 4B. On this episode: switching from 13-CURRENT to 13-RELEASE.

<!-- more -->

* fetch the sources: ```git clone -b releng/13 --depth 1 https://git.freebsd.org/src.git src``
* ```make buildworld && make buildkernel KERNCONF=$YOURCONF```
* ```halt```
* attach USB-Serial-Interface Connector (black, white, green); screen /dev/ttyUSB(`*) 115200
* boot into singleuser
* ```make installworld```
* ```make installkernel KERNCONF=$YOURCONF```
* ```mergemaster -Ui```
* if booting from SD-Card (like me), mount /dev/mmcsd0s1 /mnt
* ```cp -r /boot/kernel /mnt/boot/kernel```
* reboot
