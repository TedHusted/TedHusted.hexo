---
title: S2 Tip - Place each namespace into its own Struts configuration file
id: 181
categories:
  - Tutorials
date: 2007-03-30 05:00:00
tags:
 - Apache Struts
---

The framework configuration supports an include element. When using packages and namespaces to organize an application, each namespace can be placed into its own configuration file, and then included by the default struts.xml configuration file.

<!-- ... -&gt;-->
</code></pre>
Maintaining a separate configuration file for each module reduces coupling and increases cohesion. If different members of a team are workinmg on different segments of the application (or "module"), each group can maintain the configuration file relating to their segment.

Note that configuration packages can extend one another.
<pre><code>

... --&gt;

Common configuration settings can be placed into a default file and shared between packages.