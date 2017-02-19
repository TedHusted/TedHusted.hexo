---
title: NYC Java OpenSource Conference
id: 251
categories:
  - Events
date: 2004-04-03 06:00:00
tags:
 - Programming
---

It's a small world after all. Vic Cekvenich contacted me earlier in the year about speaking at a conference this spring. It finally came together Saturday April 3 in Manhattan. It was fun to meet some of my favorite code warriors, like Clinton Begin and Matt Raible. Not to mention my Struts partner-in-crime, Steve Reaburn. It was even more fun to learn that Jason Carreira of WebWorks fame lives only a few miles from me. Sounds like an excuse to heft a few Guiness some time. :)

Some of the earlier presentations ran long, and mine was right before lunch, so I had to rush a bit to get everyone out on time. Pity really, since it was bleeding edge stuff. I'm working on an example application for [Commons Chain of Responsibility package](http://jakarta.apache.org/commons/sandbox/chain/), using the venerable Struts MailReader. The key idea is that you can reduce the Action to a "state adaptor". The Action "simply" passes the state of the (web) application to the Command, and the Command (or Chain of Commands) does all the dirty work. In this way, the Command is a pure business object and can be used in any environment. Say, a testing environment or a desktop environment.

Given the abbreviated presentation, I'm not sure everyone got it. But, I'm sure I was at least able to put Commons Chain on everyone's radar. Aside from the Struts example, I'd also like to do a WebWorks example too, along with a generic Servlet and Portlet example.

The one non open-source presentation was about Macromedia's new [Flex platform](http://www.macromedia.com/software/flex/). Flex is a flashy front end that you can program via XML, using tools that you already have (like IntelliJ). For now, the cost puts it out of reach of many projects ($12,000 for two cpus). Still, Flex is going to be the technology to watch as the price drops. Someday, I'm sure, all web sites will look like Flex.

One very nice thing is that Flex is a presentation-only platform. If you already using a MVC architecture, say in a [Struts application](http://www.macromedia.com/devnet/flex/articles/struts.html), you can just plug it in where you are using JSP or Velocity templates today.

One area where we all should start working together today is Validation. Validation runs up and down an application, as a system layer. The Commons Validator is a nice start, but there's still much work to be done.