---
title: Free (as in beer) Microstuff
id: 211
categories:
  - Uncategorized
date: 2006-10-01 05:00:00
tags:
---

Inch by inch, step by step, Redmond softly pads closer and closer ...

Earlier this year, Microsoft leapt into the free zone by making free downloads of its Express line a permanent fixture. Originally, the Express line was being positioned as low cost, but now the cost is reduced to a free download and eventual registration.

To an extent, the Express line can trace its roots to Matrix, a free ASP.NET IDE that the Microsoft team cobbled up as scaffolding for Visual Studio .NET. Afterwards, some team members lobbied to make Matrix pubically available, and it was posted on ASP.NET, along with the "starter kits" and other goodies. Matrix is interesting, but it shuns code-behinds in favor of embedding scripts into the pages.

Though, Express ain't Matrix. Express is the real deal. Express is Visual Studio broken out as a set of point-applications, each focused on a specific part of the development process. If you are working on a n-tier application, then the Visual Studio suite is still the way to go. (Especially if you want to use plugins
like Ankh and Resharper.) But, if you are working on a simple Windows or web application, then Express may be just the thing.

All together there are six products in the [Visual Express](http://msdn.microsoft.com/vstudio/express/default.aspx) line.

*   Web Developer Express
*   SQL Server Express
*   Visual Basic Express
*   Visual C# Express
*   Visual C++ Express
*   Visual J# Express
All permanently free for the download.

A linchpin offering is SQL Express. Unlike its MSDE predecessor, [SQL Express is not performance throttled](http://blogs.msdn.com/euanga/archive/2006/03/09/545576.aspx). You can use it as a production database.

For ASP.NET web developers, this changes everything. In my experience with VS 2003, you can't really get the full benefit of the platform without using SQL Server. Of course, that's part of the plot, but, in for a penny, in for a pound. If I'm going to use a platform, I'm going to use it best I can.

Of course, back in the VS 2003 days, using SQL Server was a budget item, so it wasn't something everyone could do. (My team included.) Today, SQL Server Express
fits everyone's budget: free!

I've been working with an Oracle shop, but being renegades, we've been using MySQL, so that we could develop on the desktop. But, we've been careful to keep it all ANSI, just in case we ever have to migrate.

It's looking like that day is now coming, but instead of migrating to Oracle, we may be migrating to SQL Server Express!

Of course, switching database systems is, as they say, "non-trivial". But, as it happens, we are getting ready to roll out a new version of the database schema, with a new release of the application to match, and moving from MySQL 4 to MySQL 5 in the process. At this time, it would be just as easy, even easier, for us to move to SQL Express instead.

For backup, we've been grabbing MySQL dumps, which are designed to reconstruct the database from scratch using SQL statements. I've found that by adjusting the MySQLDump parameters (mysqldump [database] --skip-opts --compatible=ansi), we can get a decent SQL Server compatible dump. We just need to make a few minor tweaks to the create table DDLs, and it's good to go.

Just as a spike, we boosted a couple of tables into SQL Express, changed the iBATIS provider, and ran some of our unit tests. So far, all signals are green!

Of course, we don't plan to sell our souls to the dark side. Our data access layer relies on iBATIS and a coarsely-grained data transfer object. It's flexible and easy to test, and we won't be changing that any time soon.

Though, in ASP.NET 2, Microsoft is giving a nod to business objects. In fact, I was impressed by the number of good practices demonstrated in the
[Developing ASP.NET 2.0 Applications using AJAX video](mms://wm.microsoft.com/ms/uifx/asp_net_atlas.wmv). For example, along with business objects, Scott Guthrie relied exclusively on CSS for formatting.

Toss in a some actual unit tests, and we could almost call the demo Agile. But, I'm guessing we might have to wait for ASP.NET 4 or 5 before we catch Scott writing his own unit tests :)