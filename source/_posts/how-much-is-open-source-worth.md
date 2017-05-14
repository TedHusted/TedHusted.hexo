---
title: How much is open source worth?
id: 189
categories:
  - Essay
date: 2007-03-22 05:00:00
tags:
 - Open Source
---

It's official! WebWork is "worth" more than Struts 2! About 4x more! And they are both written "primarily in JavaScript"!

... at least according to the [www.oloh.net](http://www.ohloh.net/) "Explore Open Source" website.

It seems that based on metrics like "lines of code", the WebWork codebase is "worth" a wopping $7,849,557\. Meanwhile, Struts 1 weighs in at a paltry $1,761,104\. Struts 2 falls in between at $2,305,602.

Obviously, there's something askew with the [WebWork metric](http://www.ohloh.net/projects/4312). I expect it refers to WebWork 1, which included what is now XWork, and imported JavaScript libraries that the project didn't code.

The [Struts 1 metrics](http://www.ohloh.net/projects/3562) are also weird. Although the project is six years old, and we have the history of every commit from day one, Struts 1 is rated as having a short version control history. Stranger yet, Craig McClanahan is not listed as one of the contributors! That's just not right. :)

The [Struts 2](http://www.ohloh.net/projects/3569) contributor metrics seem about right, though the language metric doesn't realize that most JavaScript libraries are imported rather than coded by the host project. But, I have to admit, the ohloh site design is pleasant, and it looks like they are trying to build a community around both contributors and other users. Given a sane set of metrics, ohloh would be interesting indeed. There's a good number of projects onboard now, though a few are still missing, like [SQL on Rails](http://www2.sqlonrails.org/) (don't overlook the screencast!).

Another "fly on the wall of open source" project is [SourceKibitzer](http://www.sourcekibitzer.org/). Rather than provide digital metrics, the Kibitzer specializes in trend reports. Evidentially, the idea is that we should choose open source projects using the same sort of charts we use to pick stocks (Buy!Buy!Buy!Ching!). Where ohloh offers metrics regarding lines of code, languages, licenses, and contributors, Kibitzer offers charts describing progress, completeness, complexity, and size.

Once upon a time, I tried submitting similar statistics about Struts as part of an [ASF board report](http://svn.apache.org/viewvc/struts/current/STATUS.txt?revision=51337&amp;view=markup). The numbers I collected related to the number of mailing list subscriptions, new posts, new issues, newly resolved issues, and total number of issues. At the board level, there was zero interest in this sort of thing. From the ASF perspective, so long as the interpersonal relationship between the PMC members is warm and fuzzy, the rest will take care of itself :)

Of course, I'm not only an ASF engineer, I'm an engineer, and, personally, I do like to see hard numbers. But, sites like ohloh and Kibitzer have to move beyond codebase analyses. To the user-only community, the things that matter are the mailing list and issue tracker. Kibitzer tries to track completeness by looking for semantic clues like "TODO" comments. In practice, we are far more likely to log TODOs in the issue tracker. While it's interesting that so-and-so contributes a lot of code, to someone casing a project, it's more interesting the so-and-so will answer my question on the mailing list.

In the same vein, it would also be important to analyze the project documentation. How many lines of text compared to how many lines of code? Is the wording complex? How many contributors? Are related books and articles being published outside of the project?

I suspect that the mailing list and issue tracker statistics might be more reliable than either code or documentation analysis. Compared with reality as we know it, much of the code-based analysis [seems dodgy](http://www.sucks-rocks.com/rate/struts2/tapestry/jsf/wicket/stripes), and a dodgy statistic can worth less than Ted's best guessimate at a release date :)