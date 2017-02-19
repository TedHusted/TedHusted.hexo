---
title: Subversion Tips - SVN Redux
id: 191
categories:
  - Tutorials
date: 2007-03-20 05:00:00
tags:
 - Programming
---

When it comes to keeping track of each and every change to content, whether we like it or not, [Subversion](http://subversion.tigris.org/) is a stern gatekeeper. But, when it comes to retagging a version or rewriting a log entry, Subversion is happy to accommodate us.

### Retagging a version

Through its global revision number, every Subversion commit creates a working tag. But, instead of tracking that version 2.0.7 of your product maps to Subversion revision r520258, it's just as easy to create a tag. Later, if we need to make changes to that version without changing the head, we can convert the tag to a branch by checking it out and making a commit.

Some projects like to keep tags as a permanent record, so we often create a repository with three subfolders: trunk, tags, and branches. This particular set of folders is not required, but it's a convenient convention.

The easiest way to tag a revision is from the command line, using a command like

    $ svn copy -r 520258
    https://svn.apache.org/repos/asf/struts/struts2/trunk
    https://svn.apache.org/repos/asf/struts/struts2/tags/STRUTS_2_0_7
    -m "WW-1767 Tag r520258 as Struts 2.0.7"
    `</pre>
    On occasion, after tagging a version, we might find a minor problem before the bits are actually released, and we might want to tag it again. Not a problem for Subversion. The tags are folders in the repository, and we can rename or recopy tags as needed. But, Subversion can also add to a tag. If you try to recopy a tag by executing the previous example command using a new revision number, it won't overwrite the old tag, but merge the two together, and we end up with a mix of revisions.

    That's not exactly what we want. Better to first delete the old tag
    <pre>`svn delete https://svn.apache.org/repos/asf/struts/struts2/tags/STRUTS_2_o_7
    -m "WW-1767 Removing first try at 2.0.7."
    `</pre>
    Then, retag the branch as before, specifying the new revision number. If we don't specify a revision number, Subversion will use the latest revision (or "HEAD"). In a multi-developer environment, it's safer to use the revision so there's no doubt as to what is being tagged. Of course, since this is Subversion, we can also go back and create a tag on any revision at any time.

    ### Rewriting a log entry

    Verbose Subversion log entries is an excellent practice. A smart project will have a [developers mailing list](http://www.nabble.com/Struts---Dev-f205.html), and the Subversion alerts set to automatically post to the list. In this way, we can keep everyone in the loop as part of the everyday workflow. If we put any needed explanations in the log, we don't have to send out a special email or have a special conversation later. Very smart projects will also have [ViewVC](http://svn.apache.org/viewvc/struts/) hooked up, making it easy to browse prior revisions, view the DIFFs, and review the logs.

    On occasion, we might fire off a commit with an inadequate or incorrect log. No worries, mate. With Subversion, we can update the log so that it reads like a good entry should. Again, the easiest way to amend a log entry is via the command line. Say for example, we left off the issue reference or maybe cited the wrong revision number. To update the log for revision 504523, we can change to the local working directory, and execute
    <pre>`$ svn propset --revprop -r 504523
    svn:log "WW-1715 Branch for 2.0.x at Struts 2.0.6-SNAPSHOT r504196"
    `</pre>
    And Subversion will confirm the change
    <pre>`$ property 'svn:log' set on repository revision 504523

The change to the log won't be echoed to the mailing list like a regular commit. If a log entry is changed, the best practice would be to send a message to the developers list to document the change. ("If it didn't happen on the list, it didn't happen.")

While these aren't everyday tips, on occasion, retagging a version or rewriting a log entry can help Subversion help us.