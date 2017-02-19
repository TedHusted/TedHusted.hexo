---
title: 163__trashed
id: 163
categories:
  - Uncategorized
date: 2007-04-15 05:00:00
tags:
---

<span style="font-size:180%;">S2 Tip - Use global result handlers for common results</span>

Often, actions will share a number of comn results. Rather than configure redundant result elements for each action, share common results through a global result handlers.

       /common/Error.jsp

       /common/Error.jsp

       Login_input

     <!-- ... -->

Using global result handlers ensures that common results will be handled consistently, while reducing redundant code. Global results increase coupling, but any action that needs to handle a result differently can provide a local handler.