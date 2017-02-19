---
title: 'S2 Tip - Within a namespace, reuse names for common concepts'
id: 174
categories:
  - Tutorials
date: 2007-04-06 05:00:00
tags:
  - Apache Struts
---

In practice, most applications are composed of a small number of workflows that repeat within the application. When configuration elements in different namespaces serve the same role, use the same identifier for each element. If each namespace includes a "Menu" or a "Help" action, use those same names with each namespace.

/receivables/Menu.jsp

<!-- ... -->

/payables/Menu.jsp

<!-- ... -->

Since the namespace feature avoids collisions between element names, we can increase cohesion within the application by using a consistent naming pattern.