---
title: S2 Tip - Use Exception handlers
id: 12
categories:
- Tutorials
date: 2007-04-14 08:00:31
tags:
 - Apache Struts
---

Since Action classes tend to access the same business layer, most Actions often need to catch the same set of exceptions. Rather than sprinkle Actions with  nearly identical try..catch blocks, configure an Exception handler to catch whatever exceptions an Action may throw.

    &lt;struts&gt;

    &lt;global-results&gt;

        &lt;result name="error"&gt;/Error.jsp&lt;/result&gt;

      &lt;/global-results&gt;`  &lt;global-exception-mappings&gt;

        &lt;exception-mapping

          result="error"

          exception="java.lang.Throwable"/&gt;

      &lt;/global-exception-mappings&gt;

      &lt;!-- ... --&gt;

    &lt;/struts&gt;</pre>
    Exception handlers can be either global or local to an action mapping.
    <pre>`&lt;struts&gt;

      &lt;action name="Login"  class="actions.Login"&gt;

        &lt;!-- ... --&gt;

        &lt;result name="expired" type="chain"&gt;

          ChangePassword

        &lt;/result&gt;

        &lt;exception-mapping

          exception="dao.ExpiredPasswordException"

          result="expired"/&gt;

      &lt;/action&gt;

      &lt;!-- ... --&gt;

    &lt;/struts&gt;

Use of exception handlers separates concerns and reduces redundant code. Each Action has fewer lines of code to maintain, and we know that exceptions are being handled consistently.