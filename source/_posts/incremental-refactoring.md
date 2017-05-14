---
title: Incremental Refactoring
id: 196
categories:
  - Tutorial
date: 2007-03-15 05:00:00
tags:
 - Programming
---

Recently, someone asked on the [Ext list](http://www.yui-ext.com/forum/viewtopic.php?t=2797):
> <pre>&gt; Is Diagraming your classes/code segments
> &gt; the best way to keep yourself
> &gt; organized during the programming stage?
> &gt; Or is there some more efficient
> &gt; and easier way to do it?</pre>
I thought the advice of the Ext lead developer, Jack Slocum, beared repeating.
> The approach I have found to be most effective is "incremental refactoring". This means starting out with a basic idea of how your object model should look. It will probably be raw and imperfect, but you know that and knowing that is the key to what makes it work.
> 
> Start out small and make pieces of it work. While it's small (and not a big object model with all kinds of pieces to develop before you have anything working) you should be able to find various imperfections with your initial design (like browser problems!) and you can refactor while it's still cheap. As soon as you have something working, it's time to refactor to look for ways to make things more generic, cut code duplication, improve interfaces and efficiency. Then start on the next piece in the same way.
> 
> The most important thing is you have to think OO, think efficiency and know your patterns. Once this is in place, the rest is kind of automatic.
IMHO, one of the best books on "incremental refactoring" (or successive refinement) is "[Test Driven Development: By Example](http://www.amazon.com/exec/obidos/ASIN/0321146530/husteddotcom-20) by Kent Beck. While it doesn't mean to be about successive refinement per se, that's exactly what Beck is doing in the book. At 240 pages, it also has the virtue of being relatively brief.

As he codes, Beck keeps a running TODO list of the next baby-steps needed to finish a larger task. Beck says he keeps his on paper, but I use a text file. I even post the running list to the issue tracker, like a mini-progress report.

Successive refinement works well with an issue tracker that supports roadmaps (like JIRA). We can then organize the major features into iterations or milestones and ship one iteration at a time. Ideally, each milestone should be a working, if incomplete, version of the target application.

At work, we follow the usual [Extreme Programming](http://www.extremeprogramming.org/) model, except that we prefer formal [use cases](http://opensource.atlassian.com/confluence/oss/display/STRUTS/Use+Cases) to XP stories. (See Cockburn's "[Writing Effective Use Cases](http://www.amazon.com/exec/obidos/ASIN/0201702258/husteddotcom-20)". It weighs in at 270 pages, but the first 20 pages tell you everything you actually need to know.)

Essentially, XP interations are successive refinements, which is an approach Nicholas Wirth was advocating decades ago, when he [designed Pascal](http://www.pascal-central.com/top10.html). The key is to organize your work so that there is some kind of working application at every step of the way -- even if you have to fake some of the resources. :)

In working with Ajax and JSON, one pleasant surprise is how easy it can be to fake resources. For example, the YUI connection manager can accept a JSON object, but it doesn't care if the object came from a XHR request or static data. We can switch between a set of test data and the live stream with one line of code.

    LISTER.init = function (data)  {
    PhoneBook.rpc.entry_list(LISTER.load).call(ANVIL.channel); // live database
    // LISTER.load(LISTER.localData); // static data
    };
    LISTER.localData = {
    "result" : [['c5b6bbb1-66d6-49cb-9db6-743af6627828', 'Clinton', 'William',
    '555-743-7828', 'bubba', '08/19/1992', 37.5, '0'],
    ['7c424227-8e19-4fb5-b089-423cfca723e1', 'Roosevelt', 'Theodore',
    '555-743-8942', 'bull', '09/14/2002', 37.5, '0'],
    ['9320ea40-0c01-43e8-9cec-8fb9b3928c2c', 'Kennedy', 'John F.',
    '555-743-3928', 'fitz', '05/29/1987', 37.5, '0'],
    ['3b27c933-c1dc-4d85-9744-c7d9debae196', 'Pierce', 'Franklin',
    '555-743-7919', 'benji', '11/18/1984', 35, '0'],
    ['554ff9e7-a6f5-478a-b76b-a666f5c54e40', 'Jefferson', 'Thomas',
    '555-743-5440', 'monty', '07/04/1976', 37.5, '0']]
    };

Now _that's_ how you spell MVC!