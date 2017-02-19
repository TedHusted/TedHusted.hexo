---
title: >-
  S2 Tips - Try to keep the Action classes for the namespace in a common Java
  package
id: 8
categories:
  - Apache Struts
date: 2007-04-12 08:00:15
tags:
---

_As part of the [Struts 2 from Square One project](http://code.google.com/p/sq1-struts2/), I've been reducing the Struts 2 tips to a manuscript. One Struts Tip a week isn't keeping up with the manuscript, so I'll be running two a day for the rest of the week.  _

An element of Java style is to place types that are common used together into the same package. If namespaces are being used to organize an application, then the Actions within a namespace should be found in the same Java package.