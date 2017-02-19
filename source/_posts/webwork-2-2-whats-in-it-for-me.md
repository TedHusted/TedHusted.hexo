---
title: WebWork 2.2 - What's in it for me?
id: 236
categories:
  - Essays
date: 2006-02-12 06:00:00
tags:
 - Apache Struts
---

<pre>On 7 Feb 2006, Raghu Kanchustambham wrote on user@struts.apache.org:
&gt; I am using struts 1.2.8 and use the struts-layout
&gt; tags for my UI components. I have done some 50%
&gt; of my project work using this combination. And
&gt; I must say I am quite happy with what these
&gt; layout-tags offer so far.
&gt;
&gt; Now, how do I get webwork into my ecosystem?
&gt; Is it correct that webwork's main strength is
&gt; its rich UI components?
&gt; Can I make struts-layout (or for that matter
&gt; any other struts based taglibs) to co-exist with
&gt; webwork ? Will integrating with webwork mean that
&gt; if I replace my current struts version with the
&gt; integrated new version, my earlier taglibs will
&gt; stop working?</pre>
WebWorks is a very flexible platform, and there is room for creating a 1.x interceptor that mimick the Struts environment so that Struts specific taglibs could work. We could also do things like use XML transformations to migrate configuration files. I believe Don Brown's been working on some things along those lines. But I think the heads-down work on a compatibility layer will happen after we pass the codebase through the Incubator and can work on it here.

Of course, the web application platform is already open. If you wanted to use WebWork or Action 2 for some things in your current application, you could drop the new JARs into your application and have Action1 handle the .do's and have Action2 handle the .do2's.

For new development, the WebWork UI tags already do many of the things that packages like [ Struts Layout](http://struts.application-servers.com/) do, and we can continue to extend the UI tags, so that the UI tags better meet all of our needs.
<pre>&gt; What does this [WebWork] integration mean
&gt; to a developer like me?</pre>
Here's the bottom line: We could spend the next six years slowly evolving Struts Action 1.x until it ends up looking like a WebWork clone, or we could all work together on creating an even better Action-based framework. Both Struts and WebWorks are already the best in some way, but as Google says: "Never settle for the best."

Of course, a lot of us have Struts 1.x application in development or in maintainance mode. We're about to roll Struts Action 1.3.0, and on the dev@ list, people continue to talk abouta Action 1.4, and so on. Which is great! To keep a line of development alive, all we need is volunteers who want it to live.
<pre>&gt; In the Microsoft world, good(?),bad,ugly
&gt; - there is just one choice.
&gt; On day 1, you know "this" is what I need
&gt; to use in the MS world!</pre>
I've been working in ASP.NET for going on two years now. It's true that if you're writing simple applications, yes, what they ship in the box is just fine. Just like what Java ships in the box (JSTL and JSF) is just fine.

But, .NET developers writing enterprise applications are reaching for all the same tools we have for J2EE. If you look at the ASP.NET example frameworks, you can see the evolution away from simplistic, monolithic applications and toward rich, layered applications. One example uses this data access library, the next uses another. Same old, same old.

I think the difference is that ASP.NET teams tend to use extra libraries and packages in stealth mode, where J2EE teams wear their extensions on their sleeves. (And, at conventions, on our T shirts!)

I would agree that it's time that we did more to help enterprise ASP.NET developers come out of the closet and embrace choice. Action 2 for C# and FastCGI anyone?