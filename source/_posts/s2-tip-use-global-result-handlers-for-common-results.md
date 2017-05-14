---
title: S2 Tip - Use global result handlers for common results
id: 15
categories:
 - Tutorial
date: 2007-04-16 08:00:04
tags:
  - Apache Struts
---

 Often, actions will share a number of common results. Rather than configure redundant result elements for each action, share common results through a global result handlers.

    /common/Error.jsp

    /common/Error.jsp

    Login_input

    <!-- ... -->

Using global result handlers ensures that common results will be
handled consistently, while reducing redundant code. Global results
increase coupling, but any action that needs to handle a result
differently can provide a local handler.