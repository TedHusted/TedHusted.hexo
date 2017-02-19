---
title: 166__trashed
id: 166
categories:
  - Uncategorized
date: 2007-04-13 05:00:00
tags:
---

<span style="font-size:180%;">S2 Tip - If you are using templates, bundle the templates with the Actions</span>

Unlike Java Server Pages, FreeMarker and Velocity templates can be served from a JAR or loaded from the classpath. Rather than store Actions and templates in separate parts of the file system, consider maintaining both in the same Java package. 

 By storing together interrelated members, we spend less time navigating the file system, and we also increase cohesion within the application. 