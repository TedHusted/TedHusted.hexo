---
title: S2 Tip - Use namespaces to organize your application into logical modules
id: 188
categories:
  - Tutorial
date: 2007-03-23 05:00:00
tags:
  - Apache Struts
---

Many Struts applications contain hundreds of pages. To help organize large applications, the Struts configuration is designed around the notions of "packages" and "namespaces". Each package can set its own defaults, including a namespace setting.

/example/HelloWorld.jsp

Add actions here

Use the namespace attribute to create logical modules or units of work within an application, each with its own identifying prefix. In an accounting application, the actions relating to "payables" might be in one namespace, and actions relating to "receivables" in another.

Namespaces avoid conflicts between action names. Each namespace can have it's own "menu" or "help" action, each with its own implementation. While the prefix appears in the browser URI, the tags are "namespace aware", so the namespace prefix does not need to be embedded in forms and links.