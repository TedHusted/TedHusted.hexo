---
title: 'Looking Forward, Looking Back'
id: 216
categories:
  - Essay
date: 2006-05-28 05:00:00
tags:
 - Apache Struts
---

With [WebWork2 out of the incubator](http://struts.apache.org/2.x/), and this being the sixth anniversary of the creation of the first Apache Struts codebase (Memorial Day weekend), I thought it might be time to reflect on some of the things that Apache Struts, as a project, has been doing right, as well as some things that we might do better.

NOTE: These are my own personal observations. Other committers may have different viewpoints (and probably do!).

##### Documentation, Documentation, Documentation

One thing we have always done well -- or least better than most -- is documentation. When I stumbled onto the project in the summer of 2000, there was already a concise introduction to the framework in place, as well as a working example application (the infamous MailReader). Since then, the end-user documentation has continued to grow and mature. For developers, one hallmark of Apache Struts has always been detailed JavaDocs and commit logs.

Although sometimes criticized for a lack of documentation, the WebWork 2 wiki is a treasure trove of information. For Struts 2, we're extending and refining the WW2 base into a [comprehensive documentation suite](http://cwiki.apache.org/WW/home.html). Open source products have always been "production quality", and our documentation can be "bookstore quality" too. Following WebWork's lead, we are maintaining the documentation in Confluence, and using a [plugin](http://confluence.atlassian.com/display/CONFEXT/AutoExport+for+Confluence) that automatically exports the pages as they are edited.

An exciting aspect of documenting with Confluence is our custom "snippet" macro. The macro merges text from the source code directly into the Confluence wiki text. It's a quick and easy way to "[single-source](http://en.wikipedia.org/wiki/Single_source_publishing)" more of the documentation. Best of all, it encourages us to create better Javadocs and example applications, so that we can suck the source into the wiki.

It would be great fun to apply the snippet macro to the Struts 1 docs too, since the codebase still bears the imprint of Craig's very excellent JavaDocs. Personally, I think it is safe to say that the quantity and quality of Struts 1 documentation is one of the things that lead to its wild success.

##### Status and Summaries

One thing that we have not done well enough is to create and _maintain_ current roadmaps for new features and changes. We have created roadmaps, but as "detours" arise, we forget to update the roadmap, and it gets out of synch with the nightly build.

We also have not been very good about keeping the release notes up to date, so that updating the notes becomes yet-another chore that falls to the release manager.

On the list, we often have extended development discussions that go on for weeks, even months. While those of us in the "thick of things" can follow along, it's often hard for a newcomer to catch up. After six years, the advice to "read the dev@ list", becomes a herculean task. Over the years, we have often made decisions like "the JSP taglib API will strictly observe HTML 4.01" in the midst of mailing list discussions, but, we usually don't followup and document these decisions, outside of the thousands upon thousands of dev@ threads.

I'd suggest that we try harder to maintain a coherent status report for the project, along the lines of [what HTTPD does,](http://svn.apache.org/viewvc/httpd/httpd/trunk/STATUS?view=markup) leveraging JIRA where we can. Regardless of format, I do think it's important that the status report be something that can be automatically emailed to dev@ every month.

##### Stone Soup

I think if one thing has held the project back, it's the notion that the PMC ia suppose to be a steering committee, trying to decide what is best for other people. Committers (including myself) don't always contribute some of the coolest bits that we use in our own applications. An ASF project should be like [stone soup](http://stonesoup.esd.ornl.gov/stonesoup.html), we each throw what we have into the pot and share the wealth.

Often, it seems that we don't contribute some bits because we think our solution doesn't have a broad-enough audience. One reading of the ASF maxim "[we eat our own dogfood](http://jroller.com/page/TedHusted?entry=dogfood)" is that _we_ are the audience. If we need it for own applications, then it follows someone else will too, and, ipso facto, it belongs in the product.

When the ASF talks about a project's [community](http://jroller.com/page/TedHusted?entry=community), we are not talking about anyone who downloads the software. The ASF is talking about the people who contribute to the project through patches and posts. **We are the community.** If we need it, then it belongs in the product.

I'm hoping that by creating packages like "extras" and opening the door to the idea of specialty subprojects, like Scripting, we might be able share more of our own solutions, without bloating the core distribution.

##### Best Practices and Best Practice Examples

On the other hand, we've been much too reluctant to publish best practices, along with example applications that exhibit best practices. For Struts 1.3.x, the infamous MailReader application has been updated to demonstrate best practices and avoid "demonstration only" tangents. Moving forward, I would like to continue expanding the new "Cookbook" applications, to demonstrate common use cases, so that other example applications can be an example of the kind of applications that we ourselves write at our day jobs. For Struts 2, we've already started a [list of best practices](http://svn.apache.org/viewvc/struts/sandbox/trunk/action2/PRACTICES.txt?view=markup), which I hope we will continue to refine and publish as part of the documentation.

##### Developer Independence

Another weak area is reliance on key developers, both for work on the core framework as well as speciality packages, like the Validator and Tiles, and lately for key aspects of the infrastructure. Someone will start out doing a lot of the commits, and we fall into the habit of thinking that "so and so will take care of that". Then "so-and-so" becomes busy with his or her day job, and, suddenly, we find that many new patches have not been committed. Usually, we have between one and zero active committers who are comfortable working with Tiles or the Validator. (With Struts 2, we're liable to have the same problem with OGNL.)

One tendency that contributes to this problem, I think, is that we don't often share our work schedules with each other. We might mention off-hand that things are busy, but we don't always stress that things might stay busy for (say) six or eight months. Aside from mentioning what we plan to do, it's also important to mention what we plan **not** to do, so others can pick up the slack.

##### Going Emeritus

When an ASF developer knows that he or she won't be able to contribute for some time (perhaps a year), it is helpful to announce that he or she is moving to "emeritus" status. Merit never expires, and an emeritus committer can reactivate his or her status upon request. But, if a committer is not going to be available for a while, it helpful to let the rest of us know. Again, so that others can pick up the slack. It's not uncommon for Apache Struts committers to "fade away" without notice. Eventually, we do move inactive people to the emeritus column, but that's not something we should have to do unilaterally.

##### Committer Nominations

IMHO, the first two things a new committer should do is volunteer to manage a release and start looking for another committer to nominate. Over the years, we've developed an image of the perfect nominee. The archetypical candidate has been someone who has been posting to the lists for at least six months, participated in development discussions in a helpful way, submitted several patches that were applied, and, most often, has created an independent Struts extension of his or her own. We've made some progress in breaking away from that mold this year, especially in terms of submitting a plethora of patches.

As we move forward, I suggest that we take to heart the ASF criterion that a committer is someone who "we believe is devoted to the task and matches the human attitudes required to work well with others, especially in disagreement." Patches are one way to make "welcome contributions" to the project, but there are others.

##### struts.sf.net

We started the Struts [sf.net site](http://struts.sourceforge.net/) some time ago, so that people could share example applications and extensions without setting up yet-another SF site or joining the ASF. The site has met with some success, and we've begun to use it as a staging ground for new extensions. But, we might try to promote it more, since it could also be a proving ground for new committers. (Of course, in terms of neglect, I'm the main culprit here, since I was the lunkhead who set up the site, but never cared for it properly. Many thanks to others like Don Brown and James Holmes for picking up the slack.)

##### Mailing List Demeanor

From the beginning, the Apache Struts user list has always been patient and welcoming to "newbies". Of all things, leaving the "welcome mat" out is probably what we have done the most right, and I hope we remain cordial in the future.

Though, an important part of managing the user list is maintaining the separation of concerns between the user list and the dev list. Moving forward, we should continue to be quick and strict about keeping threads in the proper forum.