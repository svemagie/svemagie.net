---
title: "Souvereign, selfhosted infrastructure"
date: 2021-03-05
tags: [ ideas, freedom, selfhosting, dataliberation]
categories: [selfhosting, sysadmin]
comment: false
toc: true
draft: false
---
Just a rant and some thoughts on **data liberation**. As you may know, I [like data](/categories/quantifiedself/). But only when I'm the sole sovereign of it. I like to decide on my own what happens with my data. Data is *not* the new Oil, rather it is personality, an extension and expression of myself.

## State of the Internet, Germany, 2021
We're lagging behind and pay way to much for a way to sloggish internet. It's impossible to get a decent DSL-Line for under 30€. One of the richest countries in the world can't (won't?) give it's citizens fast digital access to the internet. The roots of this problem goes way back to the 90's, where Media Mogul Leo Kirch was best buddies with conservative Cancellor Helmut Kohl, who himself cut formerly state-owned (and thus people owned) "Deutsche Post" into pieces and brought the already rotting telecommunications corpse to the stock market (Deutsche Telekom). We had given away all cabeling, all infrastructure, payed by decades of taxpayers money, to a private company. This company has one goal: to make money. Their enemy was and is: infrastructure rebuilding and modernization. So they tried to leave the 50+ years old copper cables where they were, in the soil, and used some shitty tech-voodoo called vectoring to enable some Megabits when the age of DSL came along. The still do that. Why? Because of money: they have no advantage in spending money on infrastructure, as long as the germans pay happily > 50€ on old copper-vectoring-shit for 100 Mbit lines.

## Spying is censorship
They try to spy on and think of everyone who uses the internet with a mindset of general suspicion. Exactly in this weird, pandemics ridden point in the space-time-contiuum, our BMI (Ministry of the Interior) thinks loudly and officially about forcing registration and identification on everyone who uses Email and Chat ([german article](https://taz.de/Registrierungspflicht-bei-Messengern/!5751246/)). This data will end up in some intelligence agencies database. That's the way our leaders think about digitalization and think about it's citizens.

This August our ID cards will get mandatory fingerprints ([german article](https://netzpolitik.org/2020/biometrische-daten-bundestag-beschloss-speicherpflicht-fuer-fingerabdruecke-in-personalausweisen/)). Think about general suspicion again. This means: your online communication is linked to your fingerprints through your ID card. In January 2021 they decided to use your tax-ID as an unique identifier for state owned databases. The Nazis used this unique ID (Personenkennziffer) and the allies prohibited germany to ever use a single, unique identifier again ([german article](https://netzpolitik.org/2021/bundesrat-die-individuelle-personenkennzahl-kommt/)). Next, also this year, they want to scan your car licence plates and save them, just in case you might commit a crime tomorrow or in the next weeks ([german article](https://netzpolitik.org/2021/kennzeichenscanner-bundesrat-fordert-auto-vorratsdatenspeicherung/)).

**Spying is cencorship** - when people know they are spied on, their behaviour changes. Thats the magic of spying, monitoring and (thought) control. You won't do anything what YOU are thinking is maybe wrong if you have the suspicion of beeing monitored. You won't talk about or discuss anything via email or chat of *what you think the state thinks* is not correct. This refers to the sociological theory of systems, where expectation of expectations or the anticipation of expectations play a major role, ultimately resulting in pre-emptory obedience. Important things like forming impressions of and inferences about other people as sovereign personalities will get heavily disturbed[^1].

Certainly you won't express (perceived) illegal THOUGHTS. Wait, what? Illegal thoughts? The mere concept of an *illegal thought* is introduced by monitoring. The controlling instruments are simultaneously introduced: monitoring and surveillance tools. Thereby it's irrelevant if the tools hold up to it's promises, the citizens knowledge of the surveillance tools are quite enough to produce *desirable conduct*. This principle of power is described in Benthams Panopticon, Michel Foucault elaborated on it way beyond this small thinking piece. If you're interested in this, look them up.

## HowTo fight back and keep people sane
I'm quite happy to have the advantage of curiosity on all tech related things, and of using computers since way back into the 80's, of growing up on Unix machines the one way or the other. Around the 2000's I already selfhosted a bunch of services. Now I think it's about time to return to the habit of **selfhosting**, of not trusting the state nor the companies, who will happily provide every bit of personal information if requested by any shady state entity.

### First: supersede the obvious: social media and communications
The first steps towards digital autonomy are the simplest: delete all social media accounts which are ran by companies. No more Facebook, Instagram, Twitter eg. Delete the Instant Messaging Apps they operate, too.

Better look out for federating protocols like ActivityPub and the services they provide. Facebook and Twitter for example can be superseded by Mastodon. Or, in my case, Hubzilla. Just dive into it, read around, and you'll surely find a comfy place.

### Second: build independend infrastructure
Next step: buy a small SoC Device to build up your own infrastructure of services: Email, Social Media... RaspberryPi or similar like [Pine64](https://pine64.com/) are relatively cheap and offer enough power for standard services. 

Learn Linux (or pick a \*BSD variant). The people online are happy to help! I use [FreeBSD](https://www.freebsd.org) which I find more straightforward and clean compared to Linux, but that's my personal taste. Most of the relevant tools are comparable anyway as all those operating systems share a common ancestor: Unix.

I even use those systems on my daily driver, the [Pinebook Pro](/tags/pinebook/) and on an Apple Macbook (MacOS is a Unix-like OS, too). I bet you already use an Unix-like system: your mobile phone. Android and iOS are both based on Unix.

### Third: Interconnect
Interconnect to your neighbours, to others, maybe through citizen-run networks like "Freifunk" and ultimately to the internet. Sharing is caring, so why the heck every single one appartment has to use and pay it's own DSL-Line? Interconnection via wifi and some repeaters is cheap, after only 2-3 months the hardware is amortized. The fun part is: you can connect with your neighbours not only personally, but technically.

You'll learn a lot about technology and how simple it is to implement by yourself. And you'll learn how most tech companies make money out of nothing (special). You'll be proud of yourself in making all this things by yourself.

### Fourth: implement clever, bombproof, encrypted meshing protocols
The Internet was once planned as a decentral, packet-routing network. There was no (or, not really in theory) single-point of failure. If one node will be down, the packets automagically seek their way around the faulty part of the network. In 2021 we're farther away from this concept than ever. Most people think of the internet as: Google, Facebook, Email. Period. Global capitalists introduced a bunch of single point of failures into our neat "failsafe" internet. 

**We have to route around them** again.

The last days I peeked into [yggdrasil](https://yggdrasil-network.github.io/), the 'End-to-end encrypted IPv6 networking to connect worlds'. More stable is [cjdns](https://github.com/cjdelisle/cjdns). These things, projects and tools are worth to evaluate and implement if ripe.

Yggdrasil is already running on my test-environment, and I'm curious how stable it is and how to implement some services.

[^1]: https://en.wikipedia.org/wiki/Social_perception
