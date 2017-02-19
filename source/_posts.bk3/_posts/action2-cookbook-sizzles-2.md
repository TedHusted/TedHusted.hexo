---
title: Action2 Cookbook Sizzles
id: 224
categories:
  - Uncategorized
date: 2006-03-12 06:00:00
tags:
---

So far, there's been only one thing I missed while setting up a new
[Struts Cookbook for Action 2](http://planetstruts.org/action2-cookbook/Home.jsp). An essential part of the Cookbook is displaying the source files used to code the example. In the Stuts Action 1.3 version, we were using the Bean tags to dynamically read the file into a page variable and then output it with the tags filtered. The Struts Bean tag idiom is:

I couldn't find an obvious way to do this with the WebWork tags and started a thread on the WebWork user list. After a post or two, we hit on the idea of using a WebWork result to display source code as plaintext. Before I had the chance to write it myself (in the Apache Way), WebWork committer Toby Jee posted a new PlainText result to the HEAD, which I happily plugged into my work in progress.

The WebWork idiom moves all the references from the pages into the configuration file. The simplest example is a one-page "Hello World" recipe. The mapping for the PlainText result looks like this:

<pre>  
/pages/Hello/Result.jsp
/WEB-INF/classes/xwork-Hello.xml
</pre>

In the page, the links are coded like this:

## Server Pages

*   <a>Result.jsp</a>
The cool part, is that through use of namespaces, each example can have its own "View=Result" action. We need to refer to the namespace on on the Home page, but after that all the tags render themselves relative to the namespace. Here's are the the home page links (one for the icon and another for the text):

<pre> namespace="/Hello" 
Execute
</pre>

But once we link into the "/Hello" namespace, the ww tags take care of injecting the rest of the path.

Of course, any post-Struts-1.1 developer is going to be saying, "Oh, namespaces are modules!", and you'd be right. Everything in WebWork is very much the same as Struts 1.x. Only better!

If you reviewed the previous code snippet, you probably noticed the reference to "icon-open,jsp". The Cookbook uses icons for navigation, which I removed to their own server pages and then brought back in using ww:include as needed. As a result, all the image markup is centralized and not scattered throughout the application.

The new Cookbook also uses ww:include to post navigational header at the top of the pages. The nifty part is that the WW tags track the current namespaces. If we consistently name the entry action to each example "Open", we can reuse the same menu link for every example/namespace. When the Hello namespace includes this snippet --

-- the action location renders as "/Hello/Open.do". When the Simple namespace renders the same snippet, the location renders "/Simple/Open.do". Quick, easy, effective.

I'll be heads-down on the Cookbook this week, and will surely post more about the Cookbook again next week. In the meantime, you can track the progress in the [Apache Struts sandbox](http://svn.apache.org/viewcvs.cgi/struts/sandbox/trunk/action2/apps/cookbook/src/), and I'll try to keep the [online demo](http://planetstruts.org/action2-cookbook/Home.jsp) fairly current.