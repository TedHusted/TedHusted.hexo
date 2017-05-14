---
title: WebDev Pushmi-Pullyu
id: 204
categories:
  - Essay
date: 2007-03-07 06:00:00
tags:
 - Apache Struts
---

As might be expected, the Struts 2 GA announcement had its share of comments on [The Server Side](http://www.theserverside.com/news/thread.tss?thread_id=44429) last week.

One subtopic was push versus pull. As with many terms, I think we sometimes use "push" and "pull" to mean different things. Sometimes we mean it to contrast [component versus action paradigms](http://www.theserverside.com/news/thread.tss?thread_id=44429#228491). Other times, we mean to contrast [creating a custom context](http://jakarta.apache.org/turbine/turbine/turbine-2.3.2/pullmodel.html) (or API) for each page that exposes only what that page is suppose to know (push), or whether we should create a global context (or API) that can be exposed to every page, so each page can pick and choose (pull) whatever it wants.

Another use of the "push/pull" term is to contrast "merge" templates, like Velocity and FreeMarker, with "scriptlet" templates, like ASP, JSP, and PHP. In this usage, the point is whether it is better to push out to the page a prepared context, or whether the page should use scriplets to pull values from the platform's shared context.

One benefit of push is that it easier to use the technology outside of the environment, since we can create a prepared context independent of the target platform. One benefit of pull is that its easier to share values with other application resources, since the context is shared.

Struts 1 tends to muddle this kind of push and pull. ActionForms are push, but we also provide a lot of servlet attributes which pages need to pull from one of the platform's scopes (page, servlet, application). The Velocity support for Struts uses a chained context to provide access to a Velocity context as well as the platform contexts.

Struts 2 creates its own context that includes references to the servlet scopes (as plain-old Maps). In this way, S2 provides the benefits of both push and pull. For testing, it's easy to create our own action context, and at runtime, we can access the usual servlet resources. Another benefit of wrapping pull-within-push is that we can provide "first class" tag support for JSP, FreeMarker and Velocity.

Personally, I'm a fan of the template approach. The Struts 1 tags mitigated the [damage JSP scriplets were causing back in the day](http://www.servlets.com/soapbox/problems-jsp.html); before JSTL, stock JSPs were a ugly, inelegant mess. (And before Velocity people got involved in JSTL, the JSTL was a mess too.)

If there is a single reason why Struts 1 was so successful, it was because we provided a JSP taglib when everyone else (Barracuda, JPublish, Maverick, Tapestry, Turbine, among others) was focused on templates and other alternative solutions.

Over the years, I've consulted with some large concerns that standardized on templates pre-y2k. The technology worked well, but my clients eventually replaced the templates with Struts and JSPs. Not because JSP was "better", but because JSP worker drones are easy to hire. [As Craig said](http://www.servlets.com/soapbox/problems-jsp-reaction.html), project managers tend to choose "mainstream" technologies, regardless. We already have a hammer, so every problem must be a nail.

Ironically, Struts 2 "levels the playing field", so that "alternative" technologies like Velocity, Freemarker, and AJAX are on equal footing with "mainstream" technologies like JSP, JSF, and, well, AJAX. :)