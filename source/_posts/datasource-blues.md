---
title: DataSource Blues
id: 255
categories:
  - Reviews
date: 2003-04-01 08:03:00
tags:
 - Apache Struts
---

Been liking the Struts Generic DataSource lately, since it makes it so easy to switch between both databases and containers. But, alas, it's not working out well for the impending auction launch (April 21). Ran the DBCP over the weekend, and it kept exhausting connections. Tried the settings for reclaiming the connections, but it only helped marginally. The Auction application uses RowSets for retrievals, and they seem to be eating connections unless I use the JNDI form. The Resin connection pools supports JNDI directly, and I know that works just great, so it's back to that for this launch. Just need to get Tomcat configured to use its JNDI datasource so I can run the IDEA debugger in development. Very glad that I refactored the RowSet routines last week to eliminate duplication. Made switching back and forth between connections and JNDI quick and easy. Kent is absolutely right: normalization is just as important for programs as it is for schemas.

As a prank, Jake changed the screen savers on all the computers today. Reminds me of my networking days. I'd put in a batch of new computers. No one in the office would know how to use them, but still, I'd come back in the next day, and they would have managed to get all screen savers changed. =:0) "Agatha Christy" -- "JungleBook" -- what a cacophy!