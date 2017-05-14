---
title: 'Log Me, Log YUI'
id: 177
categories:
  - Review
date: 2007-04-03 05:00:00
tags:
 - YUI
---

I've never been a logging enthusiast. Many of the frameworks I use put logging to good use, but it not something I've felt compelled to inject into my own applications.

Many coders use logging as a development aid. One of my earliest Turbo Pascal programs wrote a character to the menu bar as a "tell". As the application moved through a workflow, the tell would change. If it hung up, I knew where to start looking. It started as a development aid, but I left it in the production code because, well, it looked cool!

For most developers, our desire to log takes seed with a quick write to standard out, and blossoms as world-class products like Log4J, Log4Net, Log4PHP, and the other flowers in the [Apache Logging](http://logging.apache.org/) bouquet.

Rather use loggers as a development aid, these days I tend to set a lot of breakpoints as I develop. I like to follow brother Steve McConnell's advice from [Code Complete](http://www.amazon.com/exec/obidos/tg/detail/-/0735619670/husteddotcom-20). Every time I write a new swatch of code, first, I set a breakpoint and watch it run.

Unsurprisingly, Joe Hewitt's brilliant [FireBug plugin](https://addons.mozilla.org/en-US/firefox/addon/1843) is one of my new best friends these days. Running a close second is the Yahoo! User Interface (YUI) Library's [Logger](http://developer.yahoo.com/yui/logger/).

Like other implementations, the YUI Logger works as a singleton, so it's very easy to write a logging message from anywhere in the application.

    // Assigns default category "info" and default source "global"
    YAHOO.log("My log message"); `</pre>
    As implied by the comment, we can add categories and source parameters to the method call, which are also standard fare for logging systems.

    The trick with JavaScript logging is viewing the messages. Most of the Apache Logging bunch rely on logging to console or to a shared file for the application. Some browsers do support a JavaScript console window, but it's not a standard feature. For JavaScript, we need to shift the paradigm.

    But it's not such a big shift. Since YUI is a widget library, it would make sense to express the Logger Control (or LogReader) as a widget. To view the messages as they are logged, just add the LogReader widget to an application, again with one whole line of code.
    <pre>`var myLogReader = new YAHOO.widget.LogReader(); `</pre>
    By default, the LogReader widget plants itself in the top right corner of the browser window. If that arrangement doesn't work for you, the widget can also be attached to a div or dragged around the screen. If the window box still feels cluttered, there a builtin "Collapse" button. To be even more unobtrusive, you can add your own button to show or hide the widget.

    An especially good use of logging in a JavaScript application is to document when events fire. Widgets raise primitive events like "click". A well-designed application will handle those events and raise an "idiomatic" event of its own. We can rely on the widget to raise its event, the interesting bit is when we raise our own.

    Case in point, we're working on a DataForm widget that can share a RecordSet and ColumnSet with a YUI DataTable. The form widget raises "update" and "insert" events for the benefit of a client controller application. In early testing of the widget, we can't see those events happen, because they haven't been wired to a client yet. But, we can use a logger to view the events as they are raised.
    <pre>`oSelf.fireEvent("updateEvent",context);
    oSelf.logRecordEvent("updateEvent", oRecord, oOldRecord); // debug

Where logRecordEvent is a helper method that expresses the oRecord and oOldRecord data in human-readable JSON.

We could also set breakpoints to check that the events fire, and the first time around, we still do. But, the logging statements are also a rudimentary unit test. In fact, the next step might be to create Selenium tests that look for logging statements on our example pages.

The YUI library distribution includes *-debug version of all the modules that include this kind of logging statement. The production version has the logging statement extracted. (Leaving a few strange lines of code behind, like empty else statements.)

Of course, there would be a few ways to filter production code for logging statements. [I don't know what YUI does](http://tech.groups.yahoo.com/group/ydn-javascript/message/11598), but for my own projects, I might try an [Aptana](http://jroller.com/page/TedHusted?entry=aptana) action to do the dirty work.

Though, since the LogReader can be flipped on and off, it's not hard to imagine leaving some statements embedded, for the sake of technical support. So, for now, we're marking the logging lines "// debug" in case we decide to leave some in later.

I may never love logging quite as much as Ceki, but the YUI LogReader is at least a friend with benefits!