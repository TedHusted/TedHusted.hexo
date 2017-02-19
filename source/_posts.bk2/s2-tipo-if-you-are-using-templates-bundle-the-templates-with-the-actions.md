---
title: 'S2 Tip - If you are using templates, bundle the templates with the Actions.'
id: 11
categories:
  - Struts
date: 2007-04-13 00:00:09
tags:
---

Unlike Java Server Pages, FreeMarker and Velocity templates can be served from a JAR or loaded from the classpath. Rather than store Actions and templates in separate parts of the file system, consider maintaining both in the same Java package.

By storing together interelated members, we spend less time navigating the file system, and we also increase cohesion within the application.