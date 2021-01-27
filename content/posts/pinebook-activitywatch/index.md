---
title: "PinebookDiaries: installing ActivityWatch"
date: 2021-01-14T17:30:19+01:00
tags: [pinebookpro]
categories: [tech]
series: ["PinebookPro"]
---

I consider myself as quite a bit of a data-junkie, lifelogger, selftracker- and hacker and I generate lots of data. Except on the [PinebookPro](https://www.pine64.org/pinebook-pro/) - until now.
<!--more-->
Here are the steps to get [ActivityWatch](https://activitywatch.net/) built and installed on the PinebookPro (there is an AUR-bin, but for i386, no arm64):

## QD (quick 'n dirty) overview of the dependencies, build and install process:
Use their doc section [Installing from source](https://docs.activitywatch.net/en/latest/installing-from-source.html#). watch the dependencies (python3.9 works for me btw) 

install additional dependencies: 

	yay -S rustup python-pymongo python-flask-restx python-flask-cors python-pyqt5-stubs (and/or python-mongomock?)

update rust nightly:

	rustup install nightly && rustup default nightly

Everything ran fine, except for aw-watcher-afk: correct way of getting it to compile: update the poetry lockfile via command 

	poetry lock

try to compile (PyQT5 compiles quite a while - and breaks finally)

	make build (&& make package)
 
I just copied everything to /opt; copied and enabled aw-server-rust.service which is found in the directory of aw-server-rust; let the watchers start with windowmanager i3 startup config

## todo: 
- debug the build process of 'aw-watcher-afk' and the QT5 App. But even without this building cleanly, all 3 programs start up and everything is working on the user side. So the breaking builds are somewhat cosmetic. Not ready for release (see next point), though, but fine for my usage on the Pinebook.
- If I figure out the Arch/AUR build system, maybe submit a patch - any ideas on this?
