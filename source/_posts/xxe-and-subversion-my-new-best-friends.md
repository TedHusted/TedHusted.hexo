---
title: XXE and Subversion - My New Best Friends
id: 250
categories:
  - Reviews
date: 2004-04-07 05:00:00
tags:
 - Programming
---

I'm dabbling with the idea of using DocBook for technical writing projects. Word doesn't agree with me, and I really want something I can check-in and diff. I finally took the time to visit XMLMind and take XXE for a spin ([http://www.xmlmind.com/xmleditor/](http://www.xmlmind.com/xmleditor/)). I was blow away. From the tutorial, it looks like the standard version will do what I need, but I'll probably get the professional version, just to show my appreciation. It's that good. :)

Most everything I do ends up in an open CVS these days. But there are exceptions. Subversion is the new darling of the Apache Group, so I thought I'd take it for a spin too. The Subversion book seems quite good, but I just wanted to get a Subersion up and running on a local machine, so I could start playing with it. Between the book and a quick-start in the FAQ, I was able to get Subversion up and running in the afternoon (so I could check in my XXE-made XML that evening).

Here's quick step-by-step of what I did from download to first commit.

0\. Download and install Subversion - [http://subversion.tigris.org/](http://subversion.tigris.org/)

Accept the defaults. The Windows installer with setup the path and so forth, and be ready to go. The directory conventions follow Unix, but all instructions are for Windows.

1\. Create a repository
<pre>&gt; mkdir /var
&gt; mkkir /var/svn
&gt; svnadmin create /var/svn/cookbook</pre>
2\. Setup permissions

edit /var/svn/cookbook/conf/svnserve.conf
<pre>[general]
anon-access = write</pre>
3\. Setup email notifications [TODO]

install perl for windows, install modified scriptenable template

4\. Launch svn server
<pre>&gt; svnserver -d -r /var/svn</pre>
5\. Import content
<pre>&gt; svn import -m "Initial import" /projects/ORA/svn/readme.txt svn://localhost/cookbook/readme.txt</pre>
6\. Checkout content
<pre>[delete working directory]
&gt; svn checkout svn://localhost/cookbook
Checked out revision 1</pre>
7\. Commit changes
<pre>&gt; svn commit -m "Routine changes" svn://localhost/cookbook/readme.txt
Committed revision 2</pre>
Try the [Tortoise client](http://tortoisesvn.tigris.org/) to get off the command line.

See also [http://www.onlamp.com/pub/a/onlamp/2002/10/31/subversion.htm](http://www.onlamp.com/pub/a/onlamp/2002/10/31/subversion.htm)