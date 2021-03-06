---
title: FrSIRT Alert does NOT apply to Struts 1.x !!!
id: 144
categories:
  - Event
date: 2007-09-05 05:01:00
tags:
 - Apache Struts
---

In July, the Apache Struts group issued a security fix to Struts 2 to eliminate a remote code exploit. The exploit was based on a problem with a Struts 2 dependency (XWork). Since Struts 1 does not use XWork, Struts 1 is not vulnerable to the exploit.

Unfortunately, a [recent security alert](http://www.frsirt.com/english/advisories/2007/3042%20) mis-described the problem as affecting all versions of Apache Struts prior to 2.0.9\. This is NOT accurate. No version of Struts 1 is affected by this security issue. The only affected versions of Apache Struts are 2.0.0 through 2.0.8\. Period.

We've updated the [Apache Struts website](http://struts.apache.org/) to clarify the scope of the exploit and asked FrSIRT to correct their alert to exclude Struts 1.x.

There are many reasons why someone might want to migrate from Struts 1 to Struts 2, but this security alert is not one of them!