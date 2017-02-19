---
title: GoogleCode-a-Go-Go
id: 208
categories:
  - Uncategorized
date: 2007-03-03 06:00:00
tags:
---

I'm now the proud caretaker of two different Google Code projects.

[Anvil](http://code.google.com/p/anvil/) is a business logic framework based on the context and chain of command patterns. The Anvil core is a port of the Jakarta Commons Chain of Responsibility, used by Struts 1.3\. It's available for C# now. We use Spring.NET for the object factory, and iBATIS.NET for data access (so this would be an easy port back to Java). Anvil is still way under-documented, but it works well for us. Best of all, Anvil made the transition to Ajax without missing a beat.

When we created Anvil, our original goal was to keep as much code as possible out of the ASPX pages and code-behinds, including validation and formatting. As it turns out, that same goal makes the framework an ideal backend to Ajax pages.

The other new project is [Apache Struts from Square One](http://code.google.com/p/sq1-struts2/), which hosts materials used by my training course including slide presentations, coding exercises, and a companion textbook. The text is new, but the slides and other materials have been available at the [Struts University](http://www.strutsuniversity.org/) site for some time.

The course is something I've been creating through successive refinement. When Struts first came out, people asked me to come in for "Train the Trainer" sessions, where consultants wanted to learn Struts so they could teach it to their clients and other consultants. Then, it was direct requests from development teams who wanted to jumpstart a new project using Struts. By the time Struts 1.2 came around, people were asking me to come in and review what they've been doing in Struts, to see if they were on the right track.

[On each trip](http://husted.com/ted/vita.html), I'd create a new session or improve an old session, and eventually it all evolved into a course based on the Struts MailReader application. The course has now made the jump to Struts 2, and I'm working on a companion text to the original training materials, which I might offer through [LuLu](http://lulu.com/) when it's "ready for primetime".

An early and incomplete draft of Part 1 of the book ('Carpe Diem') is available for [download](http://sourceforge.net/project/showfiles.php?group_id=49385&amp;package_id=223769&amp;release_id=490486). The complete set of slides and other materials are available through the [Struts University site](http://www.strutsuniversity.org/). There's even a new [Google Group](http://groups.google.com/group/sq1-struts2) for questions and feedback.

Of course, although the text is a work in progress, the live course is complete, and I've presented it several times. Drop by [Struts Mentor](http://www.strutsmentor.com/) for more about the live course.

Many thanks to the Google Code ninjas who are making project hosting possible for grunts like me. There are rough edges still, but I can see where Google Code is going, and I'm happy to tag along for the ride.