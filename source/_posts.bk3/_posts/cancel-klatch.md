---
title: Cancel Klatch
id: 243
categories:
  - Uncategorized
date: 2006-01-27 06:06:00
tags:
---

An old issue with the Cancel tag has broken out [on the list](http://www.mail-archive.com/dev@struts.apache.org/msg17335.html). Right now, you can bypass validation on any Struts Action by spoofing the cancel button. In some cases, that might be a bad thing if the Action is not expected to be cancelled. It looks like this might be fixed in 1.3.0, if we can agree on a patch soon enough. (Like before we resolve the Checkstyle issues.)