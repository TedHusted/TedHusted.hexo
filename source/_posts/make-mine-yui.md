---
title: Make Mine YUI
id: 207
categories:
  - Reviews
date: 2007-03-04 06:00:00
tags:
 - YUI
---

After two years of bashing our brains against ASP.NET at my day job, we're diving into Ajax for our next frontend.

We looked at Microsoft's AJAX.NET, and at Anthem, and GWT, and Dojo, and we settled on the Yahoo User Interface Library. Out of the box, the widgets aren't as sexy as some others, but the library is well documented, easy enough to use, and Yahoo seems committed to supporting YUI over the long haul. (I haven't run any benchmarks, but it also seems like its the fastest.)

Of course, I'm also liking that Yahoo [eats their own dog food](http://yuiblog.com/blog/2007/02/20/yui-220-released/) ... "the YUI you can download here is exactly the same YUI Library used all across Yahoo!".

Though, it's not YUI that's really making the move from ASPX to HTML-and-Ajax possible for us. The "reality check" kudos go to a .NET JSON-RPC libary named "Jayrock". Using [Jayrock](http://jayrock.berlios.de/), we are able to create a paper-thin ASHX handler ("servlet") that calls our business logic framework ([Anvil](http://code.google.com/p/anvil/)).

From a coding perspective, the handler is a "plain-old" C# class with some annotations to enable the JSON-RPC voodoo.
<pre>[JsonRpcMethod]
 public AppEntry entry(string key)
 {
    // ... arbitrary business logic to obtain a value object (JavaBean)
    return appEntry;
}</pre>
On the JavaScript side, Jayrock bundles a utility script that generates the JSON API gluecode for us. In the end, we can just call a method on the handler and pass in our parameters and a callback function. Click. Done. Sweet.
<pre>  PhoneBook.rpc.entry(entry_key,onEdit_load).call(av_channel);</pre>
If anything does go sour, the handler can throw an exception, which, on the JavaScript side, is automatically routed to a separate error callback. Once there, it's trivial to display the message, and even the stack trace, if we want it.

Happily, we are already doing all the data conversion and formatting on the business logic layer, so for Ajax, we are good to go.

We did an initial "spike" using Dojo and our canonical example application (a simple PhoneBook). We then did the same thing with YUI. A keen happenstance was that we were able to use Jayrock to switch "channels" behind the scenes, so that the application could connect to the server using Dojo or using YUI, just by changing one line of code.

Along with the clear separation of concerns, I think what we like most about Ajax is that it plays well with others. We can start with YUI as a foundation, but if we want to toss in some [Dojo](http://dojotoolkit.org/) or [Ext](http://www.yui-ext.com/deploy/yui-ext/docs/) widgets, we can. No sweat.

I'm thinking this feeling is what JetBrains means by "Develop with pleasure!" :)