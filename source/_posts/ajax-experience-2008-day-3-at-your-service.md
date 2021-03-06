---
title: Ajax Experience 2008, Day 3, At Your Service
id: 125
categories:
  - Event
date: 2008-10-02 19:30:00
tags: 
 - Ajax
 - Continuous Integration
 - Hudson
 - Jenkins
---

The highlight of my [testing tools talk](http://www.slideshare.net/ted.husted/testing-tools-presentation "testing tools talk") turned out to be my new-best-friend, [Hudson](https://hudson.dev.java.net/ "Hudson"), an extensible [continuous integration server](https://hudson.dev.java.net/ "continuous integration server"). The talk was in one of the 90-minute slots, so I split the agenda between reviewing some of the available tools and demoing a simple-but-complete continuous integration workflow. (By complete, I mean that on every checkin, we build the distribution and run a suite of both unit and integration tests.) Originally, the demo was to include [YUI Test](http://developer.yahoo.com/yui/yuitest/ "YUI Test"), [Selenium](http://selenium.openqa.org/ "Selenium"), and [Cruise Control](http://cruisecontrol.sourceforge.net/ "Cruise Control"). At the last minute I switched in Hudson for Cruise Control.

Setting up Hudson has to be the easiest/hardest thing I've ever done. (The previous runner-up being using iBATIS for Query-By-Example database searches.) Most CI servers are designed to run as standalone critters. Hudson runs as a standard Java web application, and it is configured using a web UI. (A clean and elegant UI, I might add.) The totally cool part is that the server can be installed by dropping the hudson.war into our favorite Java container ... or running it standalone from the command line!

[Download the WAR](http://hudson.gotdns.com/latest/hudson.war "Download the WAR"), run

<span class="quote">&gt; java -jar hudson.war</span>

and [up pops Hudson](http://hudson.gotdns.com/wiki/download/thumbnails/753667/1.png "up pops Hudson"), at your service, ready for configuration. (The one prerequesite being a recent Java executable on your system path.)

By default, Hudson stores its jobs in your home directory (even on Windows), so once it's running, you can just have at it. Tell Hudson where to checkout your project from SVN or CVS, check a box or two, and you are good to go. ([Other SCMs](http://hudson.gotdns.com/wiki/display/HUDSON/Plugins#Plugins-Sourcecodemanagement "Other SCMs") also supported) For extra credit, you can indicate an Ant file to run along with Hudson's default build. ([Other build systems](http://hudson.gotdns.com/wiki/display/HUDSON/Plugins#Plugins-Buildtools "Other build systems") also supported.) Fill out a few more fields in the web UI, and you'll be getting an email nag whenever the build or test suite fails.

With a continuous integration server in place, the remaining trick is to export Selenium tests to JUnit (or C#, or Ruby, or Python) to run both acceptance tests and any JavaScript unit tests. While running YUI Unit won't trigger a JUnit failure out-of-the-box, we can use Selenium to watch for the outcome of the tests on the test logger. If ["Failed:0" doesn't materialize](http://developer.yahoo.com/yui/examples/yuitest/yt-simple-example.html), then we know the YUI test failed.

Summing up: If you have a chance to go to Ajax Experience 2009, I'd say: "take it". The organizers always manage to come up with a nice mix of introductory and advanced presentations, many by the people who are creating the technologies we use -- people like Brendan Eich, Nicholas C. Zakas, and John Resig, to name a few. Even better, it's a great chance to mingle with other real-live developers all trying to do the very same things you are trying to do. (And then discovering your organization isn't so backward, after all!)