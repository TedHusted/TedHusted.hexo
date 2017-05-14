---
title: Haromonizing the Good Parts
id: 133
categories:
  - Review
date: 2008-09-16 05:00:00
tags:
 - Programming
---

_"Sometimes a step backward is a step in the right direction ..."_

Over the summer, there have been two loosely related events on the JavaScript landscript:_ ECMAScript Harmony _and _JavaScript: The Good Parts_.

### ECMAScript Harmony

In the land of JavaScript, the "tyranny of the installed base" rules supreme. A key frustration of JavaScript developers is that platform innovations are metered by the glacial rate at which the marketplace upgrades browser clients.

Ironically, JavaScript pundits have been equally concerned about there being too much innovation in the ECAMScript 4 specification. The new specification includes ambitious notions like packages, namespaces and early binding.

In August, the working group met in Oslo and arrived at a new focus for the future of JavaScript, dubbed the _ECMAScript Harmony_ project. The core of ES Harmony can be expressed in a set of four goals.

1.  Focus work on ECMAScript 3.1 with full collaboration of all parties, and target two interoperable implementations by early next year.
2.  Collaborate on the next step beyond ECMAScript 3.1, which will include syntactic extensions but which will be more modest than ECMAScript 4 in both semantic and syntactic innovation.
3.  Some ECMAScript 4 proposals have been deemed unsound for the Web, and are off the table for good: packages, namespaces and early binding. This conclusion is key to Harmony.
4.  Other goals and ideas from ECMAScript 4 are being rephrased to keep consensus in the committee; these include a notion of classes based on existing ES3 concepts combined with proposed ECMAScript 3.1 extensi
For more about ECMAScript Harmony, visit [John Resig's blog](http://ejohn.org/blog/ecmascript-harmony/).

### JavaScript: The Good Parts

Admist the ES4 turmoil, workers at both Mozilla and Yahoo! have advocated moderation. John Resig, of Mozilla, went on a [ES4 speaking tour](http://ejohn.org/blog/ecmascript-4-speaking-tour/) to raise awareness, and Douglas Crockford, of Yahoo!, brought out [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742/), a testament to the doctine of "less is more".

In "The Good Parts", Crockford walks through both the good and the bad of JavaScript, pointing out best practices to embrace and design flaws to avoid. While Crockford tries to "accentuate the positive", one can't help but notice that many best practices are driven by design flaws.

### Some Design Flaws

*   Reliance on global variables weakens the resiliency of programs [p25].
*   Inner variables set to functions are bound to the global object (not the "this" of the outer function) [p28].
*   Arguments are not really an array [p31].
*   Support for block syntax implies a block scope, but there is no block scope [p36].

### Some Best Practices

*   Reduce your global footprint to a single name (like YUI's YAHOO namespace) [p26].
*   Throw exceptions when a mishap is detected [p32].
*   Declare all variables used in a function at the top of the function body [p36].
*   Use the module pattern to eliminate the use of global variable [p41].
Crockford is presenting a talk on the Good Parts at the [Ajax Experience 2008](http://ajaxexperience.techtarget.com/html/index.html) in Boston on September 29th. Sadly, his talk is up against my own talk "Ajax Testing Tool Review". I just hope someone shows up for mine ...

## Preaching to the Choir

At [VanDamme Associates](http://www.vandamme.com/blog.aspx?id=1940&amp;blogid=238), we make good use of John Resig's JQuery library and Yahoo's YUI library in our Ektron-based web applications. Together, these packages already deliver the same utility that an overly-ambitous ES4 might provide. A kinder, gentler ES4 means a more stable and robust development environment for us and our clients. To quote a famous, if fictional, engineer: "The more they fancy up the plumbing, the easier it is to gum up the works."