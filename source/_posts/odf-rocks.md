---
title: ODF Rocks
id: 245
categories:
  - Review
date: 2006-01-27 06:04:00
tags:
 - Apache Roller
 - Productivity
---

The [OpenOffice](http://www.openoffice.org/) suite provides an interesting opportunity for open source products. Since the suite is free, open source, and multiplatform, using this tool with our projects is little different than using Subversion or Ant.

Problem is, the format is not change-log friendly. By design, all changes made to a ASF product are logged to one of the mailing lists, where they become part of our "communal memory". When a change is made to an OpenOffice document and checked into the repository, it is [logged](http://www.mail-archive.com/roller-commits%40incubator.apache.org/msg00632.html) as a change to a binary file. No one watching the project knows what changed unless they spend several minutes opening the document and reviewing the internal change log.

Albeit, the [Roller](http://rollerweblogger.org/page/project) community is [deliberating](http://www.mail-archive.com/roller-dev%40incubator.apache.org/msg01642.html) whether to use the OpenOffice to maintain it's user documentation. The vote is pending now. Since OpenOffice can save to multiple formats, my suggestion is that we also checkin a companion HTML document, so that everyone can see what changes in real time. We'd contnue to edit the ODF file, and just Save As to HTML before checking in both files. Film at 11.