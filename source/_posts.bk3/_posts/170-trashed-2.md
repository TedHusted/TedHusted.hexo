---
title: 170__trashed
id: 170
categories:
  - Uncategorized
date: 2007-04-10 05:00:00
tags:
---

<span style="font-size:180%;">Fortifying Ajax</span>

Last month, Fortify Software posted a [white paper](http://www.fortifysoftware.com/servlet/downloads/public/JavaScript_Hijacking.pdf) describing a security exploit dubbed JavaScript Hijacking. Being a slow news month, a number of online journals trotted out "the end is near" headlines.

Of course, Ajax development groups have been quick to post responses to the "advisory". Despite the hyperbole, engines like Dojo, GWT, and YUI are not "vulnerable". Certain applications using Ajax engines may fit a "vulnerability profile", and if so, there are simple and concrete steps that developers can take.

If your Ajax application exposes sensitive data via raw JSON, do this:

*   Enclose JSON responses in JavaScript comment characters, and
*   Strip the comments before parsing the response

Click. Done.

Like many security issues, the "vulnerability" is mainly a developer education issue.

The Dojo Toolkit is providing patches in version 0.4.3 "to inform developers of the potential risks their server-side components may be exposing them to and making it even easier to do the right thing on the client side".

The Yahoo! User Interface (YUI) Library is now adding a specific header to each request. The server side code looks for the header and refuses to service the request if the header is absent or not valid.

For more about security Ajax applications, see

*   [A Note on "JavaScript Hijacking](http://dojotoolkit.org/node/619)" (Dojo)
*   [YUI Security](http://tech.groups.yahoo.com/group/ydn-javascript/message/11723)" and [Yahoo! Developer Network - Security Best Practices](http://developer.yahoo.com/security/)
*   [Security for GWT Applications](http://groups.google.com/group/Google-Web-Toolkit/web/security-for-gwt-applications)

Since many developers were not aware of this exploit, it is a Good Thing that a White Hat brought it to our attention before the Black Hats did. Though, I hope the next White Hat takes the high road and alerts the development group first. That way, we can have a response out the day the alert is posted, rather than a week or two later.

But, of course, if you happen to be a security consultant, a blindside brouhaha is not bad for business!