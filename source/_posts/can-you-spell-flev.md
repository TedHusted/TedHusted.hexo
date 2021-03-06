---
title: Can you spell FLEV?
id: 175
categories:
  - Essay
date: 2007-04-05 05:00:00
tags:
 - Programming
---

The most common workflow in any business application I've every written or used is Find / List / Edit / View -- or "FLEV".

At work, our last two point applications used this workflow over and over again, and the third will be no different. Often we start with Finding and Listing one entity, which is then used to Find and List related entities, which have related entities of their own to List and Edit. And, of course, we also need to Edit and View any of these entities along the way.

For example in a mail reader, the software starts by finding the new posts for us and presenting a list. We then view the thread, and maybe call an editor to post another article. When posting, we might want to find and list entries from our address book, and either select or likely suspect, or view his or her details, and maybe make some changes.

So on and so forth.

One would think that a FLEV widget would be standard fare in any widget library. But, strangely not. Looking about, I find several types of tables and grids, many with inline editing and filtering. But, in real life, inline editing and filtering via a grid isn't enough. We do need to rotate the axis and present a single entry on a form, often including fields not expressed on the grid.

Recently, I've been working with spanking-new DataTable from the Yahoo! User Interface (YUI) Library. Under the hood, DataTable use a RecordSet to represent the raw set of entries, and a ColumnSet to describe the schema for an entry (data type, editor, formatter, short label, long label).

All that's needed is a companion widget that can use the same Recordet and ColumnSet to present and edit one entry at a time.

Essentially, we're talking about a widget can present the same data in four different ways. To get started, I created an issue ticket that described the widget in terms of these four views.
> A FLEV widget would allow us to define a columnset and datasource once and use it to generate four presentations.
> 
> Finder - Columnset in vertical layout as a form with multifield dataentry and submit. Submit does not modify state. (Matching entries are presented through a Lister.) Some fields may be selectors with change events that update other selectors. Other fields may be calendars or sliders.
> 
> Lister - Columnset in horizontal layout as a grid with double-click single field editing. Command events to edit, view, delete, or add may be associated with selected row. Paging through rows is supported.
> 
> Editor - Columnset in vertical layout as a read-only panel. Command events may be available, such as save, save and add another, delete, or cancel.
> 
> Viewer - Columnset in vertical layout as a read-only panel. Command events may be available, such as edit, copy, delete, print, add, or cancel.
I got a good start on FLEV this week, and we are about ready to drop it into our application. Of course, the widget will be available as open source under the BSD license as the first installment of the [Yazaar project](http://jroller.com/page/TedHusted?entry=cathedral). Tomorrow is Struts 2 tip day, but I'll be blogging more about FLEV over the weekend, including how it uses the [YUI LogReader](http://jroller.com/page/TedHusted?entry=yui_logreader) as a development tool. Stay turned!