---
title: "FreeBSD on RaspberryPi 4, MANIFEST error"
date: 2021-02-05
tags: [ snippets]
categories: [sysadmin, freebsd]
comment: false
toc: false
---
A pandemic compatible microadventure: squeeze out everything possible an RaspberryPi 4
<!-- more -->

* fetch 13-CURRENT https://download.freebsd.org/ftp/snapshots/arm64/aarch64/13.0-CURRENT/ & flash to SD
* boot up and ssh in (freebsd/freebsd, su - pw root)
* bsdinstall will throw error about MANIFEST, so change to sh shell
* `export DISTRIBUTIONS="kernel.txz base.txz"`
* `mkdir /usr/freebsd-dist`
* `export BSDINSTALL_DISTDIR="/usr/freebsd-dist/"`
* `export BSDINSTALL_DISTSITE="https://download.freebsd.org/ftp/snapshots/arm64/aarch64/13.0-CURRENT/"`
* `bsdinstall distfetch`
* `cd /usr/freebsd-dist`
* `fetch https://download.freebsd.org/ftp/snapshots/arm64/aarch64/13.0-CURRENT/MANIFEST`
* `bsdinstall`
