---
title: Salesforce – Open Source Pioneer
id: 36
categories:
 - Essay
date: 2012-04-04 13:00:00
tags:
  - Open Source
  - Salesforce
---

<div class="separator" style="clear:both;text-align:center;">[![](http://opensource.org/files/garland_logo.png)](http://opensource.org/)</div>
**<span class="Apple-style-span" style="font-family:inherit;">
</span>**
<div style="display:inline !important;"><span class="Apple-style-span" style="font-family:inherit;">Before joining NimbleUser, I spent a decade working with open source software and participating in open source communities. Being part of an open source community, and helping to improve tools we use every day, is an enlightening experience, outside the scope of simple blog entry.
</span></div>
<div style="display:inline !important;"></div>
<div><span class="Apple-style-span" style="font-family:inherit;">And I miss it. I've tried to stay involved with new projects like Vosao CMS, I answer mail sent to human-response at apache-dot-org, I speak at ApacheCon when I can, and I'm trying to bootstrap an advocacy site with Tim O'Brien. But, it's a struggle to keep up with other projects when I'm still learning so much every day about the Salesforce CRM platform. </span></div>
<span class="Apple-style-span" style="font-family:inherit;">One consolation is that Salesforce is built with many products I touched during my open source decade. I worked with Doug Cutter and Jon Stevens to migrate Lucene to Apache Jakarta. Salesforce uses several Apache Commons products that I helped create (along with the Commons itself). So, although I don't get to work on open source projects as much as I used to, I am grateful to be working with a solid, thoughtful product built on a backbone of truly awesome open source components</span>

## Core infrastructure

[![](http://www2.sfdcstatic.com/common/assets/img/logo-new.png)](http://www.salesforce.com/customer-resources/learning-center/)Salesforce.com pioneered cloud computing. Its combined Software-as-a-Service and Platform-as-a-Service offering handles 100,000+ customers, over 2.1 million users, 30+ million lines of third-party code, and hundreds of terabytes of data, running on its own blend of open source, proprietary, and custom code. [[Wikipedia](http://www.blogger.com/blogger.g?blogID=5208774#Resources)]

While the Salesforce platform uses the Oracle Database system as the basis as its backend data store, the web infrastructure is powered by several open source products, like **Caucho Resin**. The Resin Application Server is a high-performance XML application server for use with JSPs, servlets, JavaBeans, XML, and a host of other technologies. [[Hurwitz](http://www.blogger.com/blogger.g?blogID=5208774#Resources)]

## Fulltext search

To help users find data quickly and easily, in 2002, Salesforce.com added the open source **Apache Lucene** search engine to its technology stack. Today, Lucene to manages an eight terabyte index of about twenty billion documents on the Salesforce platform. The Salesforce/Lucene cluster consists of roughly sixteen machines, which in turn contain many small (sharded) Lucene indexes. The system handles roughly 4000 queries per second, and it provides an incremental indexing model where the new Salesforce user data is searchable within approximately three minutes, through use of a custom optimizer. [[Igvita](http://www.blogger.com/blogger.g?blogID=5208774#Resources)]

## Quality assurance

Salesforce.com brings out three major releases a year, and to keep development on track, their quality assurance team runs 50,000 tests a day. About 20,000 of those tests run under the open source **Selenium** web application testing system. Selenium supports exporting tests to native application languages, like Java and C#, and a Salesforce.com engineer open sourced an extension for its own language, Apex. [[Porro, Nagata](http://www.blogger.com/blogger.g?blogID=5208774#Resources)]

## User-facing tools

Several of the Salesforce external data manipulation tools are built with open source components or are themselves open source. The indispensible **Salesforce IDE** is an **Eclipse** plugin. The popular **Force.com Excel Connector** is hosted at code.google.com. Responses to Outbound Messages are handled by the **Apache Commons Connection Pool**. The very useful **Apex Data Loader** is powered by the **Spring Framework** and **Apache Commons BeanUtils**. Since the Data Loader is open source, an OSX version sprang up (**LexiLoader**), and there is also an open source wizard (**DataLoaderCliq**) to help with Data Loader automation.

## Advocacy

At its 2011 Dreamforce Conference, Salesforce.com offered an entire track of presentations that encouraged users to adopt other open source tools, including a developer session entitled “**Build Better Apps with Open Source**”. On an ongoing basis, Salesforce.com hosts an **Open Source forum** as part of its support site. [[SFDC](http://www.blogger.com/blogger.g?blogID=5208774#Resources)]

By leveraging a solid core of open source packages, and mixing in its own proprietary customizations, Salesforce is able to deliver a robust, configurable platform, upon which millions of business users rely every day.

<span class="Apple-style-span" style="font-size:24px;font-weight:bold;">Products mentioned</span>

*   [Salesforce CRM](http://www.salesforce.com/)
*   [Caucho Resin](http://www.caucho.com/resin/)
*   [Apache Lucene](http://lucene.apache.org/java/docs/index.html)
*   [Selenium](http://seleniumhq.org/)
*   [Eclipse](http://eclipse.org/)
*   [Force.com Excel Connector](http://wiki.developerforce.com/page/Apex_Data_Loader)
*   [Apache Commons Database Connection Pool](http://commons.apache.org/dbcp/)
*   [Apex Dataloader](http://wiki.developerforce.com/page/Apex_Data_Loader)
*   [Apache Commons BeanUtils](http://commons.apache.org/beanutils/)
*   [Spring Framework](http://www.springsource.org/)
*   [LexiLoader](http://www.pocketsoap.com/osx/lexiloader/)
*   [DataLoaderCliq](http://code.google.com/p/dataloadercliq/)

## [References](http://www.blogger.com/blogger.g?blogID=5208774)

1.  [Hurwitz] [Is there beef behind SalesForce.Com?](http://judithbalancingact.com/2008/05/29/is-there-beef-behind-salesforcecom/)
2.  [Wikipedia] [Salesforce.com.](http://en.wikipedia.org/wiki/Salesforce.com)
3.  [Igvita] [Lucene in the wild: Salesforce, LinkedIn, Twitter, et al.](http://www.igvita.com/2010/10/22/open-source-search-with-lucene-solr/)
4.  [Porro, Nagata] [How to test Salesforce Apps with Selenium.](http://www.youtube.com/watch?v=TXNI9SRW1sY)
5.  [SFDC] [Apex Data Loader](http://wiki.developerforce.com/page/Apex_Data_Loader), [Build Better Apps Using Open Source](http://developer.force.com/dreamforce/11/session/Build-Better-Apps-Using-Open-Source-Code), [Open Source Labs tract](http://blogs.developerforce.com/reid-carlberg/2011/08/dreamforce-open-source-lab-sponsored-by-github-schedule.html), [Open Source Forum](http://boards.developerforce.com/t5/Open-Source/bd-p/sforceExplorer).