---
title: Keeping Salesforce Implementations on Track with Milestones PM and ChangeIT
id: 51
categories:
  - Uncategorized
date: 2012-01-04 09:01:00
tags:
---

A secret to the success of Salesforce CRM is how easy it is to customize and extend. Out of the box, Salesforce provides new users with a world-class framework for enabling collaboration between staff members and customers, or between staff and staff, or even between customers and customers (if you dare!). If that wasn't enough, Salesforce provides a variety of ways to tailor your instance of the framework, so that it fits your own processes like a glove. 

When folks first implement Salesforce, it's easy to get carried away. Salesforce CRM can do so much, it's tempting (but not practical) to try and do everything at once. In fact, Salesforce.com recommends that people spread out customization plans, so that refinement becomes an ongoing process. 

Happily, two of the many Salesforce extensions include a project tracking app called "[Milestones PM](http://appexchange.salesforce.com/listingDetail?listingId=a0N30000005tvt6EAA)" and a change tracking app called "[ChangeIt](http://appexchange.salesforce.com/listingDetail?listingId=a0N300000016ct3EAA)". You can use Milestones PM to organize implementation projects, and ChangeIt to gather and prioritize new change requests. Both can be installed into your environment from the [Salesforce AppExchange](http://appexchange.salesforce.com/home), free of additional charge.

**Milestones PM**

Milestones PM is an elegant approach to project tracking that makes it easy to capture and follow a classic work breakdown structure. It's a great fit for IT projects, but the app would work just as well for any type of task-based project, such as organizing a retreat, publishing a newsletter, or planning an office move. 

The application uses six objects to track progress: Project, Log, Milestone, Task, Time, and Expense, as shown in the illustration.

[![](https://tedhusted.files.wordpress.com/2012/01/9ef16-6a00d8341cded353ef014e5f4794a0970c-pi.png?w=300)](https://tedhusted.files.wordpress.com/2012/01/9ef16-6a00d8341cded353ef014e5f4794a0970c-pi.png)**Project** is the top-level container for the other five objects. Aside from tracking key details -- like a Kick-Off Date, Deadline, and Description -- a Project contains a set of Milestones, along with an optional Log records.

**Logs** are generic memo records that you can attach to a Project, Task, Time, or Expense record. Logs can be used to capture any miscellaneous detail that doesn't fit neatly into the other fields. 

**Milestones** are the key organizing object within a project. To be useful, a Project must contain at least one Milestone record, which in turn can contain Task, Time, or Expense records. Milestones can have their own Kickoff and Deadline Dates, and be linked to Parent, Predecessor, or Successor Milestones. While not as featureful as Microsoft Project dependencies, good use of the Parent, Predecessor, and Successor Milestone groupings can make complex projects easier to understand and navigate. 

The backbone of any project are the **Tasks**. Every Task must have a Name and be assigned to a Milestone, and can also track an Assignee, Start and Due Dates, statuses like Priority (0-4), Stage (In progress, Resolved, Closed) and Class (Ad Hoc, Defect, Rework), Estimates, and other properties. 

**Time** and **Expense** records can be attached to Tasks, which are tallied as part of the Project's overall metrics. 

[![](https://tedhusted.files.wordpress.com/2012/01/17a07-mpm-dashboard.png?w=300)](https://tedhusted.files.wordpress.com/2012/01/17a07-mpm-dashboard.png)As shown in the screen shot, Milestones PM makes excellent use of native Salesforce metrics, and integrates with other native features like Chatter and Calendar It also supports batch operations based on views and comes bundled with two dozen ready-to-run Reports.

Even better, Milestones PM is an [Aloha App](http://appexchange.salesforce.com/results?filter=9&amp;sort=6&amp;type=Apps), so it doesn't count against the number of tabs or objects your Salesforce instance consumes.

**ChangeIT**

Like housework, a good Salesforce implementation is never done. The platform is so flexible and so deep, there will always be ways to make it work even better for your users. The ChangeIT application provides a vehicle for tracking new features and fixes (which you could then turn into Milestone PM tasks).

In a nutshell, ChangeIT provides a simple way to manage changes to your Salesforce instance, and to notify team members when changes are scheduled or implemented. It also helps coordinate change requests, so developers and administrators are not stepping on each other.

<div class="separator" style="clear:both;text-align:center;">[![](https://tedhusted.files.wordpress.com/2012/01/5f96e-changeit.png?w=300)](https://tedhusted.files.wordpress.com/2012/01/5f96e-changeit.png)</div>The application provides a single, simple tab with a form for making change requests, as shown in the illustration.

Saving the initial request triggers an approval workflow to the individuals that you set up. Once the initial request is approved or denied, the request can be worked on by developers or administrators (and/or transferred to Milestones PM). The application also includes a dashboard and supporting reports to view the pipeline of request changes. 

Since the applications are independent, you can use either or both -- your instance, your choice!

We're always exploring better approaches to project tracking, with applications like Basecamp, Tom's Planner, JIRA, and OnTime. Do you have a favorite tool? What features do you love? What features do you miss?