---
title: Hacking the Cathedral
id: 182
categories:
  - Reviews
date: 2007-03-29 05:00:00
tags:
 - YUI
---

A key reason we were able to use the [Yahoo User Interface (YUI) Library](http://developer.yahoo.com/yui/) is that the latest release includes a DataTable widget. Our application design uses lots of grids. Of course, there are grids from other toolkits and libraries that we could have used. But, honestly, we would have standardized on that toolkit instead.

Of course, the [YUI DataTable](http://developer.yahoo.com/yui/datatable/) is new and beta and not without its shortcomings. But, every time I found a flaw, I also cobbled a patch (and filed a [ticket](http://www.blogger.com/post-create.g?blogID=5208774)).

Looking at our design, what we do over and over again is Find, List, Edit, and View. The entity we want to Find/List/Edit/View changes, but the core workflow holds.

We did the last application module in ASP.NET. Most workflows were represented as pages that exposed one or more composite controls. Much of the page logic involved deciding which controls needed to be exposed.

For the YUI module, I'd like to have a single "chimera" widget that can reveal itself four or five different ways. As a Finder (QBE Dialog), as a Lister (DataGrid), as an Editor (Data Entry Form), as a Lister (View Only Panel), and as both a Lister and Finder in the same view. Behind the scense, the DataTable (Lister) is powered by [ColumnSet](http://developer.yahoo.com/yui/docs/ColumnSet.html) and [RecordSet](http://developer.yahoo.com/yui/docs/RecordSet.html) objects, which could also power a Finder, Editor, and Viewer.

For now, I'm referring to this as a [FLEV Widget](http://code.google.com/p/anvil/issues/detail?id=22), and I've started coding it through the Anvil site.

Meanwhile, another earnest YUI user [posted a DataTable subclass](http://tech.groups.yahoo.com/group/ydn-javascript/message/11247) that adds filtering on a column and hiding columns. Nice work, and a fine demonstrate of how easy it is to extend YUI widgets.

Since YUI doesn't accept contributions from the user community, I wonder if we should setup a community site on SourceForge or GoogleCode, where we could contribute and jointly maintain extensions like FLEV and RowFilterDataTable.

The idea would be that anyone with code to contribute would be welcome to join the site. Since widgets are granular, we could each retain our own copyrights to the original widgets, and just use the site as a one-stop shop for YUI extensions.

Of course, there is already a [YUI-Ext project](http://www.yui-ext.com/deploy/yui-ext/docs/), but that has evolved into a distinct library that supports both YUI and JQuery, and I expect other libraries one day. And YUI Ext was never a full-throttle open source project, it was always Jack Slocum's baby.

The Yahoo! team has built a cathedral. Perhaps it's time to open a companion bazaar -- a yazaar! :)