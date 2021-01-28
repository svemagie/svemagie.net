---
title: "Connection test & get all mail"
date: 2021-01-17
tags: [ mail, snippets]
categories: [sysadmin]
comment: false
toc: false
---

This is the script I call from crontab entry  `*/10 * * * * $HOME/bin/fetchmail.sh`.

First, we check if there's an internet connection, then we fetch all mails via pop and imap.

```
#!/bin/bash

ping -c 1 8.8.8.8 2>&1 >/dev/null
if [ $? = 0 ]
  then
    # mpop
    mpop -a -q
    # kill mbsync if stuck
    killall mbsync &>/dev/null
    mbsync -a -q
fi
```
