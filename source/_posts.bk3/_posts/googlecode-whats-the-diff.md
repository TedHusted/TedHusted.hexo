---
title: 'GoogleCode: What''s the diff?'
id: 197
categories:
  - Uncategorized
date: 2007-03-14 05:00:00
tags:
---

Good news! [GoogleCode Hosting](http://code.google.com/hosting/) already offered alert emails on a Subversion commit, but, strangely, the diffs were omitted from the alerts. Now, we get both logs and diffs on every commit!

This is especially great news for me. I was actually posting daily commit summaries by running svn diff to a text file and posting the output to our mailing list (Google Group). But, that was a poor replacement for per-commit logs and diffs, all in one handy-dandy email feed.

    Author: ted.husted
    Date: Fri Mar  9 09:46:00 2007
    New Revision: 182

Modified:
trunk/cs/Apps/PhoneBook/Web/PhoneBook-Editor.js
trunk/cs/Apps/PhoneBook/Web/PhoneBook-Lister.js
trunk/cs/Apps/PhoneBook/Web/PhoneBook-Menu.js
trunk/cs/Apps/PhoneBook/Web/PhoneBook-Profile.js

Log:
Issue 11 - Add events for all menu buttons. Setup "init guards" so that menu
buttons are not created more than once. Include logic to disable exting buttons
if user's status changes from editor.

Modified: trunk/cs/Apps/PhoneBook/Web/PhoneBook-Editor.js
==============================================================================
--- trunk/cs/Apps/PhoneBook/Web/PhoneBook-Editor.js (original)
+++ trunk/cs/Apps/PhoneBook/Web/PhoneBook-Editor.js Fri Mar 9 09:46:00 2007
@@ -98,8 +98,13 @@

EDITOR.onDelete_load = function (data) {
// ISSUE: This will fail if list is out of sync and data is null
- ANVIL.setMessage("Deleted: " + data.result.user_name);
- PhoneBook.rpc.entry_list(LISTER.onDeleteRefresh_load).call(ANVIL.channel);
+ if (data.result.user_name) {
+ ANVIL.setMessage("Deleted: " + data.result.user_name);
+ PhoneBook.rpc.entry_list(LISTER.onDeleteRefresh_load).call(ANVIL.channel);
+ }
+ else {
+ ANVIL.setMessage("Workflow error. Select row and try again.")
+ };
};
...

Why all the noise over diffs? Simple: Peer review and non-redundant team communication. My team is accustom to reviewing each others commits as an extended form of pair-programming. Following the example of many successful open source projects, we also use the commit logs to describe what we are doing, step by step. The alerts become part of an implicit [unit development folder](http://java.icmc.usp.br/books/ooc/html/udf_unit_development_folder.html), saving us an extra post or conversation later. And, unlike a random post to the mailing list, the commit log is directly linked to that code revision.

To help tie it all together, many projects apply "issue-driven development" to the commit logs. Under IDD, a team adopts a policy that **every** commit must reference one or more tickets from the issue tracker. If a commit doesn't relate to an open issue, then we have to create one first.

Like test-driven design, IDD encourages developers to do the right thing as soon as possible. If we are going to write the test anyway, we might as well write it first. If we are going to create the issue anyway, we might as well create it first. And, if the issue tracker is tied to the mailing list, when we create the issue, we put the team on notice of our short-term plan, just in case anyone has a suggestion.

Issue driven development works best when the issue tracker can scan the repository logs and link commits with issues. [Apache Struts uses IDD with JIRA and Subversion.](https://issues.apache.org/struts/browse/WW-1689?page=com.atlassian.jira.plugin.ext.subversion:subversion-commits-tabpanel) For odd jobs, we create an "omnibus" issue for each milestone, which we use (judiciously) as a catch-all.

[Vincent Massol (JUnit in Action) has also blogged about IDD.](http://blogs.codehaus.org/people/vmassol/archives/001063_clirr_rocks.html)

GoogleCode doesn't link issues with Subversion commits yet. In the meantime, we've been faking it by prefixing our commits with the issue number. There's not a live link to the revision yet, but at least we will be in the habit should [that feature](http://code.google.com/p/support/issues/detail?id=94&amp;can=2&amp;q=) come online.

Perhaps Devo said it best: _Duty now for the future!_ :)