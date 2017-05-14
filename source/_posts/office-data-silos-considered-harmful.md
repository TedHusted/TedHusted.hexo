---
title: Office Data Silos Considered Harmful
id: 33
categories:
  - Review
date: 2012-04-26 13:00:00
tags:
 - Salesforce
---

<div class="separator" style="clear:both;text-align:center;">[![](https://tedhusted.files.wordpress.com/2012/04/dad73-opssilo.jpg)](http://collaborationforgood.org/2012/01/17/breaking-down-data-silos-in-the-cloud/)</div>
<div style="font-family:Arial;">One of the great pleasures we have when rolling out Nimble AMS is rolling up a multitude of Excel Data Silos into a single, shareable, Enterprise database.</div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">When people learn about Salesforce CRM and Nimble AMS, we find that too many organizations are hamstrung by their current solutions. Customizations are difficult, expensive, and need to be redone whenever there is a significant upgrade. Given such hurdles, it's not surprising that people store important data in one-off Excel worksheets and Word documents, rather than go through the pain of customizing a shared solution.</div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;"><a name="more"></a>OTOH, with Salesforce CRM and Nimble AMS, our customers are able to customize the application, quickly and easily, often without our help. Better yet, customizations continue to work, upgrade after upgrade, without having to add-on the same wheel over and over again.</div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">Sound magical? To the Salesforce user, it sure seems that way. But, to quote the late, great, Arthur C. Clarke, "any sufficiently advanced technology is indistinguishable from magic". In the case of Salesforce CRM, customizations are painless and eternal through the courtesy of three technologies: point-and-click programming, API versioning, and mandatory unit-test coverage.</div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">**Point-and-Click Programming.** From the beginning, the Salesforce mantra has been "No Software", which not only means "no software to install", but also "no software to write". Many significant customizations can be created by any Salesforce power user via a point-and-click interface. You can add fields, tables, validation rules, workflows, UI screens, and more -- without ever writing a line of code.</div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">**API Versioning.** While P&amp;C can accomplish 90% of what most people really need to do, sometimes a deeper customization is needed. When a Salesforce Developer adds a database trigger or other code-based customization, Salesforce transparently notes the current version of the application. Whenever that customization interacts with Salesforce, it continues to use the same version of the application program interface (API). Since Salesforce brings out three versions a year (like clockwork), come 2014, a future-you might be using version 30, but that 2012 customization is still talking to version 24\. API versioning lets Salesforce continue to improve the platform, without breaking code that worked just fine yesterday.</div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">**Mandatory Unit Test Coverage.** Every day, Salesforce.com runs 50,000 automated tests against its own code base, and every new change has to pass that gauntlet before it ever makes it into a release. Likewise, code-based customizations made to an org must have a minimum unit test coverage of 75% before custom code can be deployed into a production org. That's not a guideline: it's a hard rule that the platform enforces.</div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">Salesforce also upgrades all of the development environment "sandboxes", automatically, in advance of every new release. Automatic sandbox upgrades gives those of us distributing a product with extensive customizations (like Nimble AMS) a chance to upgrade any of our own features for the latest and greatest improvements. And, when we do bring out an upgrade to our "managed package", we can push improvements (and fixes) to all our customers, transparently, at once, just like Salesforce itself.</div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">Taken together, these three technologies -- Point-and-Click Programming, API Versioning, and Mandatory Unit Test Coverage -- create an environment that is so sufficiently advanced that upgrades become painless and, well, automagical.</div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">For more about the Salesforce release cycle, check out "[All Aboard the Salesforce Release Train](http://nimbleuser.com/blog.aspx?id=4116&amp;blogid=236)".</div>