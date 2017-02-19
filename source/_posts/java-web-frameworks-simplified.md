---
title: Java Web Frameworks - Simplified!
id: 210
categories:
  - Reviews
date: 2006-11-16 06:00:00
tags:
 - Programming
---

**Is there a standard Java web framework?**

Yes. Sun has added JavaServer Faces to its
[Java Enterprise Edition](http://java.sun.com/javaee/), which includes other goodies like JavaServer Pages and the JavaServer Pages Standard Tag Library. If you grab the distribution that bundles the NetBeans IDE, Java has finally become a one-stop shop!

**Is JSF a specification, like JSP and JSTL?**

Yes. Sun provides its own Reference Implementation, but other people can provide their own implementation of the specification.

**Is anyone doing that? **

Yes. The Apache Software Foundation offers the [Apache MyFaces](http://myfaces.apache.org/) JSF implementation that you can use instead of the Sun RI. Along with a certified JSF implementation, the MyFaces project also offers a number of additional JSF components. Standard JSF components include gizmos like a DataGrid. Additional components include goodies like tabbed panels, various tree views, HTML editors, scrollers, and such, that you can drop into your application. If you are just getting started with JSF, the MyFaces mailing list provides a lot of friendly, free support.

**Are there other JSF component libraries available? **

Yes. For example, Oracle offers ADF. In fact, Oracle donated the better part of its ADF component library to the ASF some time ago. It's slowly becoming part of MyFaces under the name Trinidad.

**What about AJAX? **

People are trying various approaches toward integrating AJAX with JSF components. Some AJAX frameworks for JSF are already available, including [Ajax4jsf](https://ajax4jsf.dev.java.net/nonav/ajax/ajax-jsf/). Generally, you can use AJAX frameworks with the Sun RI or MyFaces, as you prefer.

This is the same sort of approach that Microsoft is taking for its ASP.NET framework (which is similar to JSF). To leverage AJAX, Microsoft is building a second framework, Atlas, on top of ASP.NET.

**Are there other JSF frameworks? **

Yes. The [Apache Shale framework](http://shale.apache.org/) extends JSF to add advanced features, many of which are designed to woo Java web developers used to Action-based frameworks, like Struts.

**What's happening with Struts? **

By any measure, Apache Struts is still the most popular Java web framework. Last year, Struts merged with another web development community, WebWork, to create a new version of Struts. [Apache Struts 2](http://struts.apache.org/2.x/) is in beta release now.

**Are there any comparisons of the JSF and Struts 2 feature sets? **

In practice, comparing JSF and Struts 2 is less about the features and more about the development paradigms. The reason we have these two frameworks is because each solution approaches the web development problem from a different perspective.

The implicit goal of JSF-style component-based development is to make web development more like conventional desktop/GUI development. The implicit goal of Action-based development is to streamline web development and make the best of the web platform. If a team likes the idea of creating web applications as if they were desktop applications, then platforms like JSF and ASP.NET are the way to go. If a team is ready to embrace the web for what it is, then platforms like Struts, Ruby, PHP, and so forth, are still the way to go.

In other words, it all depends on the problem we are trying to solve. If the problem is that web development is unlike conventional GUI development, then the component model is a valid solution. If the problem is that web development lacks an elegant and extensible infrastructure, then the action model is a valid solution.

**But can't we create a framework that resembles GUI development but provides a simple, web-savy infrastructure?**

Possibly, but that would be the web equivalent of [unified field theory](http://en.wikipedia.org/wiki/Grand_unified_field_theory). Today, in physics, and in web development, we have two models that are each used for different reasons, but, side by side, the two are wildly incompatible.

In physics, it boils down to "little" or "big"? In web development, it boils down to "GUI Desktop" or "Enterprise Web"?

Pick your poison!