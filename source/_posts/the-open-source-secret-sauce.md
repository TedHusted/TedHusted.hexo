---
title: The Open Source Secret Sauce
id: 184
categories:
  - Essays
date: 2007-03-27 05:00:00
tags:
 - Programming
---

I have no idea why JIRA, Confluence, and Subversion are still separate products. (Well, being a working engineer I do have at least an idea ...)

In practice, most of us use JIRA, Confluence, and Subversion as if they were one product. (A rich man's [Trac](http://trac.edgewall.org/).) I'd like to be able to refer to using all three products together as "Convergence", but that would be confusing, since it sounds too much like a real product. There's the hollywood hybrid approach, but terms like "JirFlusion" or "ConVera" or "SuJiCo" seem distracting. Let's just go with a simple acronym, like JCS.

At work, we use JCS for everything. When we are ready to do something, anything, we open a [JIRA](http://www.atlassian.com/software/jira/) ticket. If it's a coding task, the ticket will relate to a Use Case that we create in [Confluence](http://www.atlassian.com/software/confluence/), which will link back to the JIRA ticket. When we commit the bits to [Subversion](http://www.atlassian.com/software/confluence/), we reference the JIRA ticket, and the [JIRA-Subverson plugin](http://confluence.atlassian.com/display/JIRAEXT/JIRA+Subversion+plugin) closes the loop.

In these days of "[TeamCity](http://www.jetbrains.com/teamcity/)" and "[Team System](http://msdn2.microsoft.com/en-us/teamsystem/default.aspx)", we like to think of JCS as "Team Best of Breed", especially when you toss JetBrains IDEA or Resharper into the mix.

But, issue tracking, document management, and version control aren't just for coders. Other workers in the enterprise been using it to draft grant proposals and evaluate accessibility compliance. These are products that any information worker can (and should) use.

At work, one of our more ambitious Confluence applications is the infrastructure documentation. I spent the last week of last year's contract dumping everything I knew about our infrastructure into Confluence. We ended up finding another contract for me after that, and another one after that, but at the time, I wanted to be sure the team had a solid reference.

Of course, a problem with putting software documentation into software is that you need software to use the software that tells you how to install the software. Happily, between the [Confluence autoexport plugin](http://confluence.atlassian.com/display/CONFEXT/AutoExport+for+Confluence) and SpiderZilla, that's not a problem. The autoexport plugin renders the Confluence wiki as plain text, and the [FireFox SpiderZilla plugin](https://addons.mozilla.org/firefox/1616/) sucks it down to a local machine. [Here's the result](http://husted.com/archive/WQD-INF/), no server required.

The fully-loaded "Team Best of Breed" doesn't stop at JCS. Clever shops will also want to install [ViewVC](http://viewvc.tigris.org/), so that prior Confluence revision logs and diffs are only a click away. ViewVC means adding Apache HTTPD to the mix, but it's worth the effort. (And HTTPD makes a good host for exported Confluence spaces!)

A developers mailing list with an archive is the final, but perhaps most important, team member. Each component of JCS can send change alerts to a list, to keep everyone in the loop with no extra effort. JCS doesn't include an actual mailing list, but Confluence can be used as an [archive](http://www.blogger.com/post-create.g?blogID=5208774).

Taken together, I like to think of these products as creating a PRIM architecture: Portal - Repository - Issue Tracker - Mailing List. Every successful open source project I know uses PRIM. Every closed source project I know, doesn't. (Well, except mine!) Many will use two or three of the four components, but most teams will wobble on the mailing list. In practice, an archived mailing list is essential. A list ties the other components together, creating a coherent communications system.
> It's truly beyond me why more product managers don't insist on an archived development mailing list. It's the best way to keep track of what everyone is doing. You can just sit back, watch the emails come in from JIRA, Confluence, and Subversion, and know exactly who is doing what and what they plan to do next.
People wonder how open source projects manage to create high-quality products without managers or accountability. The answer: we're accountable to our infrastructure.

PRIM is the open source secret sauce.