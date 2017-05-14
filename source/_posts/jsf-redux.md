---
title: JSF Redux
id: 148
categories:
  - Event
date: 2007-07-31 14:33:00
tags:
  - Programming
---

The Expert Group for JSF 2.x is [now forming](http://www.jcp.org/en/jsr/detail?id=314), and people continue to consider [whether JSF is the right choice](http://www.nabble.com/Is-Struts-still-a-better-choice-over-JSF-as-on-today---tf4148557.html).

To recap my own comments ...
> Here's the thing: The vast majority of web applications are intranet application that we build in six to twelve weeks to serve a handful of users.
> 
> In a small-group intranet environment, server/bandwidth efficiency is not the limiting factor. The overriding concern is whether the application helps the people using it become more efficient. Do our people get more work done by using the application? Since we may be talking about five or twenty-five people connecting to a server on the next floor in the same building, scalability, in an environment like this, literally doesn't matter. We still need a reasonable response time, but, often, people will forgive a slightly-longer response time for a more helpful user interface.
> 
> Of course, not all of us are building small-group intranet applications. If we have thousands of users who hit the application occasionally, from all over the world, our non-functional requirements can be quite different. Or, if we have a small group of users, but those users are working on the space shuttle, again, our requirements are going to be different. Different requirements imply different solutions.
> 
> There's an old saw: Do you want it Fast, do you want it Right, or do you want it Soon? Pick any two.
> 
> For an intranet application, we might choose Right and Soon. (If it's not right, no one will use it.) For an Internet application, we might pick Fast and Soon. (If it's not fast, no one will use it.) For a space shuttle application, Fast and Right would more important than time to market (and we wouldn't even use web technology!).
> 
> For an intranet application, JSF can be an excellent choice -- if you have a group of developers that like working with Swing/VB type technology. For an Internet application, Struts can be an excellent choice -- if you have a group of developers that like working with standard web technology.
> 
> But, make no mistake, in final analysis, it's the developers that will make the difference. So, either use the technology that your team likes, or hire developers that like to use your technology. :)
In the realm of physics, in order to get any work done, we've had to resort to two separate, and seemingly contradictory, theories. Quantum theory is useful for calculations at an atomic scale, and general relativity is useful for calculations on a planetary scale. Either works fine on their own, and we don't hesitate to put each to good use, but we've yet to find a way to reconcile the two.

In my experience, the same principle applies to web development. Techniques that work well for small intranet applications may not work as well for busy Internet applications, and vice versa. When it comes to the craft of web development, one size does not fit all.

_Vive la difference!_