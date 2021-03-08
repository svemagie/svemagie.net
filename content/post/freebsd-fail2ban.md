---
title: "fail2ban on FreeBSD"
date: 2021-03-08
tags: [ firewall, snippets]
categories: [sysadmin]
comment: false
toc: false
---

I had some trouble to feed fail2ban output to my pf firewall, so here's my how to:

In short: RTFM! Don't just copy'n paste configs; don't mix anchor (new) and tables (old)

1. pf.conf: delete all those <fail2ban> table and rules and just set the anchor `
anchor "f2b/*"`
2. fail2ban: enable the filters & actions you want (by creating jail.d/\*.local files). my bsd-ssh.conf looks like this, I use this as template for other services:
```
[bsd-sshd]
enabled = true
mode   = extra
filter = bsd-sshd
logpath = /var/log/auth.log
maxretry = 2
bantime = 86400
findtime = 3600
ignoreip = 192.168.0.0/24
```

you can check with
* show all anchors: `pfctl -a 'f2b/*' -sA`
* show all rules for all anchors: `pfctl -a 'f2b/*' -sr`
* show the rules for bsd-sshd: `pfctl -a f2b/bsd-sshd -sr`
* show banned IPs for bsd-sshd: `pfctl -a f2b/bsd-sshd -t f2b-bsd-sshd -T show`
* bonus: show everything `pfctl -a 'f2b/*' -sa
