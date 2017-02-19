---
title: S2 Tip - Maintain configuration files in a resource folder or Java package
id: 9
categories:
 - Tutorials
date: 2007-04-12 08:01:04
tags:
  - Apache Struts
---

The default struts.xml file is loaded from the root of the classpath. In a web application, the WEB-INF/classes folder is loaded to the root of the classpath.

To avoid maintaining files directly under WEB-INF, create a resource  folder that can be copied to WEB-INF/classes when the appliication  is compiled. If subfolders are also copied, then the resource folder  can mirror the Java package system. The Struts Showcase application uses this common approach.
<pre>+ java

  + receivables

    - Deposit.java

    - Menu.java

  + payables

    - Menu.java

+ resources

  - payables.xml

  - receivables.xml

  - struts.xml

  + payables

    - Menu.properties

  + receivables

    - Menu.properties

+ webapp

  + payables

  + receivables

    - Deposit-error.jsp

    - Deposit-input.jsp

  + WEB-INF</pre>
Alternatively, maintain the configuration files alongside the Java packages, and have the build system copy resource files from the   Java source root. The Struts Mailreader uses this alternative approach.
<pre>+ java

+ receivables

- struts.xml

- Deposit.java

- Menu.java

- Menu.properties

+ payables

- struts.xml

- Menu.java

- Menu.properties

- struts.xml

+ webapp

+ receivables

- Deposit-error.jsp

- Deposit-input.jsp

+ payables

+ WEB-INF</pre>
If FreeMarker templates are used in lieu of JSPs, then all the resources for a namespace can be kept in a single folder.
<pre>+ java

+ receivables

- struts.xml

- Deposit.java

- Deposit-input.ftl

- Deposit-error.ftl

- Menu.java

- Menu.properties

+ payables

- struts.xml

- Menu.java

- Menu.properties

- struts.xml</pre>