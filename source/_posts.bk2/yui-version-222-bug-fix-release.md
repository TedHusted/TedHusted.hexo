---
title: 'YUI Version 2.2.2: Bug-Fix Release'
id: 18
categories:
  - Ajax
date: 2007-04-19 08:00:17
tags:
---

 [Version 2.2.2 of the Yahoo! User Interface (YUI) Library](http://yuiblog.com/blog/2007/04/18/yui-2-2-2-released/) is now available.

Usually this kind of update means downloading the new scripts, dropping them into your development folder, and ultimately updating the server. But since YUI is also [serving the minified scripts from Yahoo! servers,](http://developer.yahoo.com/yui/articles/hosting/) downloading the release is optional, all you may have to do is replace references to "http://yui.yahooapis.com/2.2.0/" with a reference to "http://yui.yahooapis.com/2.2.2/".

Of course, any decent IDE will do the search and replace for you, so it's not any more work. We simply trade updating our local copies of the scripts with touching all the files that use the script. Since my team wasn't checking in the YUI scripts, it's actually less work, since only one of us has to do it once, and we don't have to touch the server at all.

One handy result of the trade-off is the potential for mixing and matching versions. The 2.2.2 release is suppose to be a bug-fix, though the beta (repeat _beta_) DataTable saw some significant internal changes. In fact, my [DataForm widget](http://www.geocities.com/planetyazaar/examples/dataform/tutorial-tabview.html) can't use the new version (yet). But, no worries, I changed that reference back to  2.2.0 and its running, giving me breathing-space to sort out the problem. (Which I'm sure will be yet-another case of me pushing the envelope, and the envelope pushing back!)

Meanwhile ... the YUI release notes are helpful but high-level. That's not a bad thing, but if you are working closely with the library, and perhaps building your own widgets on top of YUI's, then it can also be helpful to have a line-by-line change log. Towards that end, I've checked in the last two YUI releases to the [Yazaar project](http://www.geocities.com/planetyazaar/). Having the releases under SVN means that we can obtain DIFFs between versions, and review the line by line changes. To keep the YUI archive out of the way, I tucked it under the [branches](http://yazaar.googlecode.com/svn/branches/yui/build/) folder.  (Gotta love Subversion!)

Of course, I'll be resolving my DataTable glitch today, and looking to see if [Jenny Han Donnelly and company](http://yuiblog.com/blog/2007/03/) slipped in any new goodies.