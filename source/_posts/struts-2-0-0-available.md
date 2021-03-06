---
title: Struts 2.0.0 Available
id: 212
categories:
  - Event
date: 2006-09-27 05:00:00
tags:
 - Apache Struts
---

We started rolling snapshots in August, and the time for tagged builds has arrived. The distribution still have some rough edges, but it is feature complete, and ready to take on eager early adopters.

The API is essentially WebWork 2.2 with some deprecations and legacy features removed. New features include wildcard mappings, JSF support, and stateful checkboxes.

We adopted the notion of **wildcards** from Cocoon and introduced the feature in Struts 1.3\. With wildcards, we can define a action name pattern, like "Edit*" and have it match any incoming URIs that being with "Edit". Or, if you prefer, "*Edit" to match URI names ending it Edit. Or, perhaps "*/*" to match names like "People/edit".

The latter pattern, "*/*" is a useful way to handle "dispatch" actions. S2 includes a mapping attribute for the method name, making it easy to setup a pattern like "method={2}" "class=actions.{1}". Given "People/edit", the action mapping would expand to "method=edit" "class=actions.People". In other words, call the edit method on the People Action.

The **JSF support** is still new, but it looks sound. Essentially, you can use JSF components as another type of view. Of course, the view is only one part of JSF, but it's the part that interest people first.

**Checkboxes** have always been a thorn in the side of web developers. If the checkbox is clear, the client submits nothing back. Nada. Zilch. It's like the checkbox was not even there. The problem being that HTTP, being text-based, has no clear way to say "FALSE". Struts 2 fixes this glitch by keeping track of when a checkbox is used on a page. If the checkbox control doesn't post back, then the framework inserts a false value into the context, and, suddenly, even checkboxes are stateful. (That only took six years!)

But the sparkle on Struts 2.0.0 has to be the distribution itself. The "all" distribution weighs int 40mb, but it includes ready-to-deploy WARs of the example application, binary dependencies, source code, Java 4 compatabilility JARS, and the new and improved documentation.

The WebWork 2.2 documentation is actually quite good, and given a fresh coat of paint, the Struts 2.0 documentation almost glistens. The docs are now organized into three main parts: Tutorials, Guides, and FAQs. The Tutorials are designed to get people up and running. The Guides provide the detailed information people need as applications are created and improved, and the FAQs fill in any gaps.

If you are looking for the Next Big Thing in action-based frameworks, take [Apache Struts 2.0.0](http://struts.apache.org/2.x/) for a spin. You won't be disappointed.