---
title: 164__trashed
id: 164
categories:
  - Uncategorized
date: 2007-04-14 05:00:00
tags:
---

<span style="font-size:180%;">S2 Tip - Use Exception Handlers</span>

Since Action classes tend to access the same business layer, most Actions often need to catch the same set of exceptions. Rather than sprinkle Actions with  nearly identical try..catch blocks, configure an Exception handler to catch whatever exceptions an Action may throw.

       /Error.jsp

       &lt;exception-mapping
         result="error"
         exception="java.lang.Throwable"/&gt;

     <!-- ... -->
    `</pre>

             Exception handlers can be either global or local to an action
             mapping.

    <pre>`

       <!-- ... -->

         ChangePassword

       &lt;exception-mapping
         exception="dao.ExpiredPasswordException"
         result="expired"/&gt;

     <!-- ... -->

Use of exception handlers separates concerns and reduces redundant   code. Each Action has fewer lines of code to maintain, and we know   that exceptions are being handled consistently. 