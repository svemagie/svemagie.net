---
title: "GTD: Todoist <-> Taskwarrior"
date: 2021-01-20
tags: [pinebook, python, snippets, gtd]
categories: [sysadmin]
---

Each of my legs stands firmly in one eco-system: most of my work is done on an Apple Powerhorse, but there are other beautiful Unixes, too. Some other things and most of my digital leisure happens on a Linux Laptop, the [PineBook Pro](/tags/pinebook/). So bringing those worlds play along nicely is a task. This time: GTD (Getting things done): managing business and private tasks.

I work in Unix terminals most of the time, so my nifty taskmanager for the console is [Taskwarrior](https://taskwarrior.org/). On MacOS and iOS I moved from [Omnifocus](https://www.omnigroup.com/omnifocus) to Todoist, and back and forth again. Those switches are more or less project-bound: heavy duty task- and projectmanagement is done on Omnifocus, everyday managing is on Todoist.

I recently found a nice script which does the syncing of Taskwarrior and Todoist: https://github.com/StefanBRas/todoist-taskwarrior (which is a fork of https://github.com/matt-snider/todoist-taskwarrior).

## In short:
- configure it: `./venv/bin/titwsync configure --map-project Inbox= --map-project Company=work --map-project Company.SubProject=work.subproject --map-tag books=reading <TODOIST_API_KEY>`
- setup a crontab: `./venv/bin/titwsync sync`
