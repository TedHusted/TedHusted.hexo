---
title: 162__trashed
id: 162
categories:
  - Uncategorized
date: 2007-04-16 05:00:00
tags:
---

<span style="font-size:180%;">S2 Tip - If you are using JSPs, try to name the JSP folder after the namespace or Java package</span>

Unlike template systems like FreeMarker and Velocity, JavaServer Pages cannot be served from a JAR or loaded from the classpath. The next best thing is to name the JSP folder after the namespace or Java package for the corresponding Action class. 

 By creating correlations between the pages and the Actions through a shared set of naming conventions, we increase cohesion within the application. If wildcards are used, consistent, shared naming conventions can reduce the number action elements an application needs.