---
title: 'Ajax: The usual suspects'
id: 192
categories:
  - Reviews
date: 2007-03-19 05:00:00
tags: 
 - Ajax
---

Blogger [Cheng Chun Kit](http://jaycck.blogspot.com/2007/03/wk-8-full-list-of-ajax.html) seems to building a survey of Ajax Toolkits, Frameworks, and Libraries. He has a good start, with twenty detailed listings. Though, he still has miles to go, since [Douglas Crockford](http://jroller.com/page/TedHusted?entry=crockford_clips) believes that there are at least two hundred Ajax library available today. But, I think Cheng has at least covered the top contenders.

Of the top contenders, the top-top four libraries seem to be Prototype, script.aculo.us, Dojo, and Yahoo! User Interface (YUI) Library. Happily, it's not an election of remedies. If need be, it's possible to mix and match libraries on the same page.

For example, during our [Ajax spike](http://www.blogger.com/%20http://jroller.com/page/TedHusted?entry=spike), we coded the same test application in Dojo and YUI. At one point, we had a mix of Dojo and YUI controls in the same application, and it all worked just fine. We even had the Dojo widgets going through YUI Connection Manager to talk to the server.

You can do the same kind of mash-ups with Java web frameworks too. There's nothing stopping us from having Tapestry answer some requests and Struts 2 answer some others. In fact, running Struts 1 and Struts 2 side-by-side is one of the recommended migration paths.

But, I digress ... Ajax widgets aside, there are a number of Ajax helpers that can work with any library. The [DesignCreatology Ajax Tools roundup](http://designcreatology.wordpress.com/2007/03/12/ajax-tools/) includes a few Ajax extensions, like Behavior and Event-Selectors. With [Behaviour](http://bennolan.com/behaviour/), you can add a JavaScript event to an element using a CSS selectors. Likewise, [Event-Selector](http://encytemedia.com/event-selectors/) binds events to elements using CSS style syntax.

Two other nifty Ajax helpers are DWR (for Java) and Jayock for .NET). [DWR](https://dwr.dev.java.net/) stands for Direct Web Remoting. DWR allows code in a browser to use Java functions running on a web server just as if it was in the browser. On the .NET side, [Jayrock](http://jayrock.berlios.de/) is able to call into server-side methods using JSON as the wire format and JSON-RPC as the procedure invocation protocol.

With DWR or Jayrock, it starts to feel like we are working with a full JavaScript stack. We can call server-side methods as if they were JavaScript functions. Meanwhile, with the [scripting support in Java 6](http://www.onjava.com/pub/a/onjava/2006/04/26/mustang-meets-rhino-java-se-6-scripting.html), we could actually be calling server-side JavaScript methods!

Can you say _convergence_? ... I knew you could :)