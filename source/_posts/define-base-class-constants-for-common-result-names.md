---
title: S2 Tip - Define base class constants for common Result names
id: 19
categories:
  - Review
date: 2007-04-20 08:00:15
tags:
 - Apache Struts
---

The framework defines several constants that are used to identify common result use cases, such as, ERROR, INPUT, FAILURE, LOGIN. NONE, and SUCCESS.

When an application has common result cases of its own, such HELP, MENU, or CANCEL, the application should define additional constants to represent various result types.

The use of constants reduces programming errors, increases cohesion, and also documents an application's common result types.