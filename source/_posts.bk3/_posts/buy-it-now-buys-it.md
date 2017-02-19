---
title: Buy It Now Buys it
id: 254
categories:
  - Uncategorized
date: 2003-04-02 09:22:00
tags:
---

Got the new Buy It Now feature up and running now. Wanted to get some tests running against the bid process in the bargain, but the opporunity never presented itself. By the time I got the feature cut into the (monsterous) JSPs, there was very little business logic left. All I needed to do was put in a short clause to close the lot if a valid BIN bid came by. Not to be foiled, I'm forging on with the Retract Bid story. There *must* be something in there I justifiably test!

Meanwhile, the sysops ran a fresh install on the new server, with the X-Window libraries this time, so Java could run on native threads. JVM and Resin went in just fine. (Tracing that one down drove me nuts!) Still couldn't get the MySQL benchmark RPM installed. Seems to be some RPM naming glitch (Mysql vs MySql.) N'suth.