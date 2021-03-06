---
title: Getting Started with Professional Ajax
id: 206
categories:
  - Review
date: 2007-03-05 06:00:00
tags:
 - Ajax
---

Like most developers of a certain age, I'm a latecomer to Ajax. We've adopted it whole-heartedly for my current project, so I'll be passing along my own spin on the learning curve.

In the beginning, I snagged a copy of "[Teach Yourself AJAX in 10 minutes](http://www.blogger.com/post-create.g?blogID=5208774)", which is actually quite good. But the notion that you can do the "chapters" in 10 minutes is a bit far fetched. (Ditto for the "24-hour" books. Most chapters seem to take me 90 minutes to two hours. Maybe I'm slow.)

Last week, I stumbled on a [PDF](http://www.ebook.gen.tr/ajax/Professional%20Ajax%20Php.pdf) of the first edition of Ajax Professional. The second edition is
[
hot-off-the-presses](http://www.amazon.com/exec/obidos/tg/detail/-/0470109491/husteddotcom-20), but I haven't had a chance to pick it up yet.

In the first edition, I was hoping the example application (Chapter 9) would contrast conventional versus event-driven programming, but it seemed to spend more time with the routine guts of the application, rather than the interesting Ajax bits.

Chapter 1 lays a good foundation for Ajax development. Most of Chapter 2, we are handling with frameworks, but it does make one appreciate how much YUI/Dojo and Jayrock do for us, given that with Jayrock we are boiling the Ajax calls down to
<pre>   PhoneBook.rpc.entry_list(entryTableLoad).call(channel);</pre>
Chapter 3 describes several patterns, most or all of which will apply
to my current project.

*   Predictive Fetch
*   Incremental Field Validation
*   Multi-Stage Download
*   Try Again
*   Submission Throttling
*   Periodic Refresh ("Polling")
*   Cancel Pending Requests
Another chapter provides a gentle introduction to [JSON](http://json.org/), though I think we have a handle on that now. Since JSON is the constant notation for
JavaScript, I'm finding that it's also handy for creating test data.
We could even create an mock RPC script for database-free testing (see
Anvil Issue 16).

[An excerpt from the second edition is online now.](http://yuiblog.com/assets/proajax.pdf) It's a section from Chapter 4, "Ajax Libraries" that covers the YUI Connection Manager. The second edition adds chapters on Ajax Libraries, Request Management, Mpas and Mashups, Ajax Debugging Tools, Ajax Frameworks (including Atlas), and a .NET case study. So, I'll probably have to get it.

Meanwhile, [Douglas Crockford's Wrrrld Wide Web" site](http://www.crockford.com/) is a menagerie of groovy links. (Crockford being responsible for the notion of JSON and JSON-RPC.) I've only pursued a few, but I'm sure to be back.

[JavaScript Lint](http://www.javascriptlint.com/) is a "must-have". It saved me several minutes of debugging the first time I used it.

So as to not let the tools have all the fun, I ordered a copy
of "[JavaScript: The Definitive Guide](http://www.amazon.com/exec/obidos/tg/detail/-/0596101996/husteddotcom-20)" through the link on Crockford's
site. (My pidgin JS is beginning to show!)

Speaking of tools, I wasn't thrilled with what [HMTL Tidy](http://www.w3.org/People/Raggett/tidy/) does with the JS script
elements. It wraps the script element but doesn't indent the closing tag, leaving the markup looking like a awkward hanging indent. Since our HTML files are heavy with script includes, I'd rather not use Tidy as a formatter. But, it still works well for validation. (As does The HTML Validator for FireFox.)

OTOH, I am thrilled to be back in a place where we can expect the pages to validate with HTML Tidy!

I'm sure Ajax will be another long strange trip, but at least the path is well-traveled. I'm looking forward to sharing the ride.