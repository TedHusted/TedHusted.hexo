---
title: 'Aptana: Another brilliant IDEA'
id: 180
categories:
  - Reviews
date: 2007-03-31 05:00:00
tags: 
 - Apache Struts
 - Ajax
---

I've always been a die-hard IDEA fan. Jetbrains says "Develop with pleasure," and they mean it! Even with Eclipse and NetBeans nipping at their heels, Jetbrains has managed to stay ahead of the pack.

But, for Ajax applications, [Aptana](http://www.aptana.com/) is poised to give Jetbrains a run for its money.

Being an Eclipse application, Aptana already has a huge headstart. The JavaScript editor is augmented with all the features you would expect: code assist on CSS, HTML, and JavaScript, FTP support, and a JavaScript debugger to troubleshoot your code. The Aptana IDE also includes features you might not expect, like being extensible via macros ad actions writtnen in (yes!) JavaScript. There's also a very cool outline perspective (which I wished I had found sooner!). (Hint: Dock the Project and Outline perspectives with FastView.)

For a quick tour of key features, scope the [screenshots](http://aptana.com/screenshots.php) page.

Out of the box, Aptana is all about Ajax. For example, the project wizard will setup the Ajax libraries of your choice (or at least any of the top nine: AFLAX, Dojo, jQuery, MochKit, Prototype, Rico, Scriptaculous, YUI, and/or yui-ext). Being a good open source citizen, Aptana uses other OS products under the covers, including JS Lint and FireBug. A special bonus is that the [project documentation](http://aptana.com/documentation.php) includes a most excellent CSS/HTML/JavaScript reference useful to anyone developing AJax with any editor on any platform.

Quirks? There are a few. After three days, the debugging still isn't working for me. Just to be a tease, it did work once, and it worked brilliantly, but then stopped as suddenly as it started. With two monitors, Aptana and standalone FireBug work well together. (But standalone FireBug works well with anything!)

There is also an "Open Declaration" feature that works only sometimes for me too. But Aptana is young, and open source, and these things are sure to sort themselves out.

For straight-up Java/Ajax development, I expect that IDEA still has the edge. But, for anything else, IDEA's predilection for Java shows, and you're developing against the grain.

Although Aptana is based on Eclipse, and uses an embeded Jetty server for native debugging, it shows no favoritism to Java. Developers working in Ruby, PHP, C#, et cetera, are made to feel right at home.

In fact, a Ruby RAD environment seems to be an Aptana endgame. The RadRails IDE is being built over Aptana. Once Rails.net is ready for primetime, I expect that RadRails is going to draw me towards the light :)

I've only been using Aptana for a week. I haven't spent much time in the forum, and I'm still not sure of the project's motivation. The site mentions that Aptana is funded by venture capital, but I don't know what revenue stream is projected. I expect they could turn around and sell it, which is fine with me. People pay me, and I don't mind paying other people. Or, it may all be to promote a consulting business that will put the IDEs to good use. I really don't know yet.

But for the code I'm developing now, it's saving me time over using Visual Studio with Resharper. The code/test cycle is very quick, and I don't have to worry about running JS Lint. Aptana handles those types of syntax errors as we edit. I filed a ticket about improving the Resharper JavaScript support, but, evidentially, that's not going to happen any time soon. Resharper is focussed on C# 3.0 support, and JavaScript is on the back burner.

A key Aptana edge is that it's based on Eclipse, opening the door to other Eclipse plugins. There is a [C# plugin for Eclipse](http://www.improve-technologies.com/alpha/esharp/#features), and between iterations, I'll have to give it a try. If the C# plugin works well enough, or we can make it so, we might be able to sideline Visual Studio all together.

I enjoy using VS with Resharper, but we often code against the Visual Studio grain. If Resharper is not going to improve JavaScript editing, I might finally be lured away from Jetbrains.

In the end, I think Aptana might just become the "Ajax IDE for the rest of us". :