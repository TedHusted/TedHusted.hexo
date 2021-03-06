---
title: To ORM or to Map?
id: 249
categories:
  - Essay
date: 2004-04-25 05:00:00
tags:
 - Programming
---

The Java community (incuding Sun itself) have been working with SQL generators for several years now. Results have been mixed. The current wisdom is that SQL generation is a good thing when you

(1) Have complete control over your database implementation

(2) Do not have a Database Adminisrator or SQL Guru on the team

(3) Need to model the problem domain outside the database as an object graph.

When one or more of these conditions are not meant, many developers are turning to SQL mapping tools.

A mapping tool lets you assign a logical name to a SQL query. The parameters the query needs can be taken from a single object passed at runtime, or as a primitive number or string. Likewise, the result of a query is passed back in a disconnected object.

A mapping tool is less like a "persistence layer" and more like a lightweight wrapper to simplify using ADO.NET. You still have full control of the SQL, but you can test and manage the SQL code outside of the application.

The best time to use a mapping tool is when:

(1) You do not have complete control over the database implementation, or want to continue to access a legacy database as it is being refactored.

(2) You have database administrators or SQL gurus on the team.

(3) The database is being used to model the problem domain, and the application's primary role is help the client use the database model.

For a time, there was a feeling in the Java community that developers shouldn't need to worry about SQL. After some bad experiences, this feeling is changing. More developers now recognize that SQL is here to stay. We've been using SQL and the Relational Model virtually unchanged for 40 years now --- which is more than we can say for any other development platform. :) The question for many teams is not how we can "hide" SQL, but how we can ~manage~ SQL.

For managing SQL, the [iBATIS Database Layer](http://ibatis.com/common/sqlmaps.html) for Java has become quite popular. There is also a .NET version called [Nausicaa](http://sourceforge.net/projects/nausicaa). It's code-complete and is now working on porting the iBATIS documentation.

Of course, some projects have a rich business object layer that models the problem domain independently of the database. In other projects, the SQL is not complex and is mainly grunt work. Some projects need to migrate to different database systems, for example, from Access to SQL Server. In these cases, a solid object-to-relational "persistence layer" can be a very good thing. Many of these have been written for Java, most based in whole or in part on Scott Ambler's famous [white papers](http://www.ambysoft.com/persistenceLayer.html).

Most Java implementations require developers to specify a good deal of metadata. There is a new implementation for .NET that cuts the redtape to a minimum and faithfully follows Ambler's architecture, called [Gentle.NET](http://www.mertner.com/projects/gentle/). I took Gentle for a test drive the other day, and it works very, very well.

But, at the end of the day, most of my projects work better with a mapping approach, like that taken by iBATIS and Nausicaa. But, then, I actually enjoy writing SQL :)