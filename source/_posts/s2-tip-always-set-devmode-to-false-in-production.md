---
title: S2 Tip - Always set devMode to false in production
id: 195
categories:
  - Tutorials
date: 2007-03-16 05:00:00
tags:
  - Apache Struts
---

In the Struts 2.0.6 release, we made the mistake of setting devMode to true in some of the example applications. As a result, some developers copied the setting, and then wondered why their Struts 2 application seemed sluggish!

As of Struts 2.0.7 (hopefully coming soon), we've added a [Performance Tuning](http://struts.apache.org/2.x/docs/performance-tuning.html) page to the documentation. Here's the highlights:

&nbsp;

*   Do not use interceptors you do not need
&nbsp;

&nbsp;

*   Use the correct HTTP headers (Cache-Control &amp; Expires)
&nbsp;

&nbsp;

*   Copy the static content from the Struts 2 JAR when using the Ajax theme (Dojo) or the Calendar tag
&nbsp;

&nbsp;

*   Create a `freemarker.properties` file in your `WEB-INF/classes`directory
&nbsp;

&nbsp;

*   Copy the `/template` directory from the Struts 2 JAR into your web application's root
&nbsp;

&nbsp;

*   When overriding a theme, copy all necessary templates to the `theme` directory
*   Do not create sessions unless you need them
&nbsp;

&nbsp;

*   When using FreemarkerResult, try to use the Freemarker equivalents rather than the JSP tags
&nbsp;

For the nitty-gritty, visit the latest copy of the [page](http://struts.apache.org/2.x/docs/performance-tuning.html).

Kudos to Philip Luppens for starting the tuning page. If you have any tuning tips of your own, feel free to post comments directly to the page. (Gotta love Confluence!)