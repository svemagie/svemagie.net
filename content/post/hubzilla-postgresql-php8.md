---
title: "Moving Hubzilla from php74+mysql to php80+postgresql on a FreeBSD ZFS"
date: 2021-04-03s
tags: [ hubzilla, postgresql, freebsd, zfs, php, snippets]
categories: [sysadmin]
comment: false
draft: false
toc: true
---

Our small Hubzilla instance now ran for a few weeks and proved to be a quite useful and fun tool, so I decided to take it to the next level and move it to shiny new software: to current php80 and postgresql13.

The current installation is neatly sitting in two seperate [jails](https://docs.freebsd.org/en/books/handbook/jails/): 
* web (on 192.168.3.210, running nginx, php74) and 
* db (on 192.168.3.215, running mysql).

In hindsight and for later convenience we set up the jails in the first place with some IP space in between, so the new jails will be on .211 and .216. If we iterate that scheme further, we can re-use the old IPs and start all over in the future.


## the new postgres jail
First, I created a new jail for postgres. I use bastille as a jail manager just because it's so convenient. 

* ```bastille create -V db2 12.2-RELEASE 192.168.3.216 genet0```
will create a 12.2-RELEASE jail on the specified vlan IP Address on the NIC genet0. I'll update the jail (eg. all jails in one go) as soon as 13-RELEASE is there in a week or so.
* starting up postgresql in a jail needs the following setting: ```allow.sysvipc=1```; so put it into ```/usr/local/bastille/jails/db2/jail.conf```.

### ZFS
A quick look what the best settings for a postgresl setup brought up the following:

* ```zfs create -p store/db/postgres/data13/pglog```
* ```zfs set primarycache=metadata store/db/postgres/data13```
* ```zfs set recordsize=16k store/db/postgres/data13```
* ```zfs set recordsize=1M store/db/postgres/data13/pglog```
* ```zfs set mountpoint=/usr/local/bastille/jails/db2/root/var/db/postgres store/db/postgres```

we created the dataset on our "store" (2x 3 TB USB drives configured as a zfs-mirror with bells and whistles like a log and cache configured to run on a faster SSD), set some settings for the database and mounted the postgres directory it into the jail.

### install postgresql
Now login to the new jail and fetch the ports tree 
* ```bastille console db2``` and do a 
* ```portsnap auto```

then you can go to ```cd /usr/ports/databases/postgresql13-server/``` and ```make install clean```

I used https://pgtune.leopard.in.ua/#/ to get reasonable settings for postgres and added some extra tweaks for postgres on zfs:
```synchronous_commit = off``` and
```full_page_writes = off```
should be set because ZFS is handling atomic writes and does excellent caching.

Check the permissions on ```/var/db/postgres``` (which is zfs mounted from outside into the jail), it should be owned by your postgres user/group: ```chown -R postgres:postgres /var/db/postgres```.

## the cloned Web jail
For the sake of security and because we can, we spin up a second jail just for the webserver. In my case, I just cloned the old one to get an easy jumpstart: ```bastille clone web web2 192.168.3.211``` so I already have a fully configured and working nginx installation which just needs to be configured to talk to db2 instead of db.

there was a ports-tree installed already, so go for installing php80: 
```cd /usr/ports/lang/php80``` and ```make install clean``` and
```cd /usr/ports/lang/php80-extensions```;  ```make install clean```

## preparing for the switch
Now we prepare both new jails for the "great switch".

Sadly, all "mysql to postgresql" conversion tools I tried had some errors, so I decided to make use of Hubzillas ["nomadic identity"](https://medium.com/@tamanning/nomadic-identity-brought-to-you-by-hubzilla-67eadce13c3b) feature and exported all my postings and relations to a json file and then import it into the fresh Hub. What proved to be a problem, too, later on.

### on db2
* create new database and database-user in postgresql

* edit ```pg_hba.conf``` to let the webserver jail talk to us, the database jail:

```host    chatsubo        chatsubo        192.168.3.211/32        md5```

(chatsubo is my database and also the database-user for the hubzilla instance, make use of password for this connection)

### on web2
Edit php-fpm.conf to get the socket right, crank up the workers, and finally tell hubzilla to listen to the new database server. Move the old hubzilla directory out of the way (you can remove it safely, as this is already a clone from the old web jail) and git clone a fresh version of hubzilla. prepare and install it like on the [installation guide](https://framagit.org/hubzilla/core/blob/master/install/INSTALL.txt).

## the great switch
Because it's a small instance with just a few profiles on it, uptime and integrity wasn't a thing. If uptime was important, I would have set up a new subdomain, installed everything and after testing just switched the subdomain back to its original place.

The switch itself is purely IP-based: change the redirect on the gateway/router to the new IP of web2.

I just did a hard switch and installed the new hubzilla instance in the live environment. The Fediverse is thankfully forgiving as it will try and try again to deliver it's messages if a host is down for a while.

### the import
With one profile, the import went smoothly, all posts and friends except for the images obviously, where up and running again.
With the other profile, the import failed with a timeout, repeadedly. I tried to crank up the timeouts in php-fpm, but to no avail. Finally I gave in and used this as positive sign to clean up my roster ;-)

### cleanup
After testing that everything is fine it's time to clean up after a few days:

* remove all php74 related packages on the web2 jail. remember we've just cloned it, so there's still some clutter lying around.
* backup the old jails, just in case: ```bastille export db /store/backups/server/old-jails/ && bastille export web /store/backups/server/old-jails/``` (diskspace is plenty and cheap these days)
* cleanup: ```bastille destroy db && bastille destroy web```

### lessons learned
* I figured out I had 3 points of complete failure: the faulty database-conversion/translation from mysql to postgresql, the staging from dev to live and on part of Hubzilla the json import of profiles.
* Prepare more: research and use a database-conversion tool of some sort. If this had been a "real" instance with real profiles, I would've been up for a lot of trouble the dirty way I finally did the move.
* The switch: go the extra mile and do a soft switch. Set up everything the right and correct way in a staging environment, and only if the database and/or import problems are solved, stage it up to live (eg. do "the switch").
* On the hubzilla side of things: make use of the nomadic identity (earlier) and upload the channel json to a more stable and propably bigger environment - it will function as a online-live backup, too.
* After all, a RaspberryPi4 with 8 Gigs of RAM is a very capable device, but when running multiple services in production, operations like the import of a big json file through php into postgres it will push it onto it's knees. Maybe I'll buy a second one just for the database? A third one for the staging work?
