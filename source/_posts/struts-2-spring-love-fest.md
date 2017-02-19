---
title: 'Struts 2: Spring Love Fest!'
id: 214
categories:
  - Tutorials
date: 2006-09-06 05:00:00
tags:
 - Apache Struts
---

In my own work, my (latest) favorite Struts 2 feature is the [Spring integration](http://cwiki.apache.org/WW/spring.html).

Want to expose a Spring bean named "peopleFinder" on your Action?

1.  Add a "peopleFinder" property to your Action.
&lt;&gt; Done. The framework sees that there's a peopleFinder Spring bean and a peopleFinder Action property, and Struts 2 does the math.

Or, would you prefer to snag the several beans from the Action by name. No problem. But, it does take three steps.

1.  Implement BeanFactory on your Action.
2.  Add a Spring bean element for your Action.
3.  In the Struts config, use the bean Id for the `class` attribute.
&lt;&gt; Done. If you want this Spring bean or that Spring bean or some other Spring bean, the Action can just ask the BeanFactory for it.

If you do use Spring to instantiate Actions, you can use Spring to populate any properties you like, or call any constructors you care to create. It would be more verbose, but I can't help but wonder if we should support using the Spring DTD to define it all!