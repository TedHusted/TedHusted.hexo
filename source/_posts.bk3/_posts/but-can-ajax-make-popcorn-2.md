---
title: But can Ajax make popcorn?
id: 194
categories:
  - Uncategorized
date: 2007-03-17 05:00:00
tags:
---

If you're a Struts developer dabbling with Ajax, the new Ajax tags in Struts 2 can make it easy to get started. Both Struts 1 and Struts 2 are excellent Ajax environments for Java web developers. A S1 action can return whatever text stream your Ajax widgets might need. For S2, pop in the JSON [plugin](http://cwiki.apache.org/S2PLUGINS/home.html), and, bingo, instant Ajax computability! Better yet, just fire up [DWR](http://getahead.org/dwr/).

If the Ajax bug bites, and you go beyond dabbling, it's likely that you will want to add an Ajax library to the mix. After peeking at the source for an Ajax library, it may begin to dawn that you don't know so much about JavaScript after all. YUI and Dojo are not the tinkertoy scripts we cut and paste into our markup for the odd special effect.

An excellent way for a Java developer to lean more about Ajax is the [YUI Theater site](http://developer.yahoo.com/yui/theater/). It's a virtual convention, featuring sessions with Douglas Crockford (JSON), Joe Hewitt (Firebug), and Chris Wilson (IE). You can't pop out a question, but you can pause the action, and view the material at your own pace.

For starters, I heartily recommend the "Browser Wars Episode II" clip. It's an absolute must-see for anyone doing web development, whether it be bleeding edge or trailing edge. You won't learn many stupid browser tricks, but you will get a look around the bend at what's happening with browser development

For frontline JavaScript development, Doug crockford's video trilogy is another must-see. For more about that, see my [prior blog](http://jroller.com/page/TedHusted?entry=crockford_clips).

Featured this week is a brief but rich session with Gopal Venkatesan: **"Writing Efficient JavaScript"**. Some of the material overlaps with the Crockford Clips, but there's are new goodies too. At 22 minutes, it's well worth the time.

Sadly, the slides aren't posted with the video clip, though I'm told that they will be posted eventually. [Evidentially, the laptop with the presentation and Venkaesanis are now in different zip codes :)] In the meantime, here's a set of summary bullets:

&nbsp;

*   Do not use eval() when there are alternatives
&nbsp;

&nbsp;

*   Do not pass a function as a string to setTimeout. (It uses eval.) Pass an inline function instead.
&nbsp;

&nbsp;

*   Do not use the "with" statement. Set a local var instead.
&nbsp;

&nbsp;

*   Do not use try/catch wihin loops. Put the loop inside try/catch.
&nbsp;

&nbsp;

*   Avoid global variables. Period.
&nbsp;

&nbsp;

*   When concatenating several strings (or DOM), use Array.join() or String.concat()
&nbsp;

&nbsp;

*   Hide element before modifying several properties
&nbsp;

&nbsp;

*   Avoid setting multiple element attributes by replacing the CSSText property
&nbsp;

&nbsp;

*   Setting HTML to the innerHTML attribute is faster than going through DOM (but less pure)
&nbsp;

&nbsp;

*   Detach DOM properties before appending several elements
&nbsp;

&nbsp;

*   Attach common event handlers to the elements' container and let the events bubble up
&nbsp;

&nbsp;

*   Use consistent naming conventions to help identify elements that share event handlers
&nbsp;

Next in line for me is Joe Hewitt's Firebug video. I truly enjoyed Firebug 1, but the latest version is so featureful, I feel a bit lost. Hopefully, Joe can machete me a path through the new interface.

Or, maybe I should just use [Aptiva](http://www.aptana.com/), which embeds Firebug into a very slick-looking Ajax IDE.

You know, sometimes, I do feel like [the machine is us/ing us](http://www.youtube.com/watch?v=NLlGopyXT_g)!