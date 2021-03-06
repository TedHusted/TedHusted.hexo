---
title: YUI Test - The New Kid on Block
id: 130
categories:
  - Review
date: 2008-09-23 05:00:00
tags:
 - YUI
---

One of the great features of the [Ektron](http://ektron.com/ "Ektron") content management system platform is that it plays well with other technologies. At [VanDamme Associates](http://www.vandamme.com/content.aspx?id=362), we often mix custom JavaScript and AJAX elements into our Ektron solutions.

Ektron itself is no stranger to AJAX, and it even bundles a tweaked version of JQuery in the standard distribution. One good tool for testing JavaScript, that [Ektron has blogged about](http://dev.ektron.com/blogs.aspx?id=13568), is the venerable [JsUnit](http://jsunit.org/) framework. While JsUnit is a solid tool, there are drawbacks.

*   Although JsUnit has been available since 2001, it's still the love child of a sole developer.
JsUnit's release schedule is irregular. (Release 2.2 has been in "beta" for over two years now.)

#### Look Ahead

Happily, there's a new kid on the block: [YUI Test](http://developer.yahoo.com/yui/yuitest/). YT is bundled with the Yahoo! User Interface Library (YUI). To quote the YUI site:

> "_YUI Test is a testing framework for browser-based JavaScript solutions. Using YUI Test, you can easily add unit testing to your JavaScript solutions. While not a direct port from any specific xUnit framework,YUI Test does derive some characteristics from nUnit and JUnit._"

Key features of YUI Test include:

1.  Rapid creation of test cases through simple syntax.
2.  Advanced failure detection for methods that throw errors.
3.  Grouping of related test cases using test suites.
4.  Asynchronous tests for testing events and Ajax communication.
5.  DOM Event simulation in all A-grade browsers.
YUI Test has been available since July 2007 (YUI 2.3.0), and made "GA" grade in February 2008 (YUI 2.5.0). The YT framework was created by Yahoo! engineer [Nicholas C. Zakas](http://www.nczonline.net/). It's regularly released and maintained with the rest of the library, and its distributed under the library's BSD license. (All together, there seem to be about sixteen developers on the YUI team, maintaining about thirty components).

(!) Note that YUI Test can be used to test **any** JavaScript code -- the application doesn't need to be based on YUI to use YUI Test.

One of the pleasures of YUI Test is that it can use the YUI TestLogger as a test harness.

[![](https://tedhusted.files.wordpress.com/2008/09/6bbdc-yui-test.png)](http://developer.yahoo.com/yui/logger/)

TestLogger is a subclass of [YUI Logger](http://developer.yahoo.com/yui/logger/), which can be used for general-purpose JavaScript logging and debugging, giving us two great capabilities in one flexible component. YUI Test can also be used in collaboration with the new YUI Profiler (another Zakas creation) to create performance-based tests.

#### Trip the Rift

Unit testing conventional, classical code often relies on exercising the "API Contract". If we pass certain parameters to method, the method should return a certain result, or raise a certain exception. Since, JavaScript is event-driven, in order to determine the outcome of a method, we often need to know what events it raises. Meanwhile, in an Ajax application, there can be a disconnect between when a method is called and when the event is ultimately reaised. YUI Test helps us bridge the gap with a "wait/resume" feature. A test can subscribe to an event, and call "resume" in the event handler. When the test reaches the point where an action might take an indeterminate amount of time, we can call "wait", and the test continues when the event fires.

For example, here's a test that calls a time-consuming animiation routine. The test registers an event handler, starts the animation, and waits for the event to fire. When the animation completes, the test confirms that the routine accomplished the expected result.

<span style="font-family:courier new, monospace;"> </span>
<div id="testLogger"></div>

<span style="font-family:courier new, monospace;"> </span>
<div id="testDiv" style="position:absolute;width:10px;height:10px;"></div>

<span style="font-family:courier new, monospace;"> YAHOO.namespace("example.yuitest");</span> <span style="font-family:courier new, monospace;"> YAHOO.example.yuitest.AsyncTestCase = new YAHOO.tool.TestCase({</span> <span style="font-family:courier new, monospace;"> name : "Animation Tests", </span> <span style="font-family:courier new, monospace;"> testAnimation : function (){</span> <span style="font-family:courier new, monospace;"> var Assert = YAHOO.util.Assert;</span> <span style="font-family:courier new, monospace;"> var YUD = YAHOO.util.Dom;</span> <span style="font-family:courier new, monospace;"> var myAnim = new YAHOO.util.Anim('testDiv',</span>

{ width: { to: 400 } }, 3, YAHOO.util.Easing.easeOut); <span style="font-family:courier new, monospace;"> myAnim.onComplete.subscribe(function(){</span> <span style="font-family:courier new, monospace;"> this.resume(function(){ </span> <span style="font-family:courier new, monospace;"> Assert.areEqual(YUD.get("testDiv").offsetWidth,</span>

400, "Width of the DIV should be 400."); <span style="font-family:courier new, monospace;"> });</span> <span style="font-family:courier new, monospace;"> }, this, true);</span> <span style="font-family:courier new, monospace;"> // Start the animation and wait for the resume function</span> <span style="font-family:courier new, monospace;"> myAnim.animate();</span> <span style="font-family:courier new, monospace;"> this.wait(); </span> <span style="font-family:courier new, monospace;"> }</span> <span style="font-family:courier new, monospace;"> });</span> <span style="font-family:courier new, monospace;"> YAHOO.util.Event.onDOMReady(function (){</span> <span style="font-family:courier new, monospace;"> var logger = new YAHOO.tool.TestLogger("testLogger");</span> <span style="font-family:courier new, monospace;"> YAHOO.tool.TestRunner.add(YAHOO.example.yuitest.AsyncTestCase);</span> <span style="font-family:courier new, monospace;"> // Run the tests when DOM is ready</span> <span style="font-family:courier new, monospace;"> YAHOO.tool.TestRunner.run();</span> <span style="font-family:courier new, monospace;"> });</span>

Not every test needs to use wait/resume, but, when we do, it's an indispensible feature.

#### Be Assertive

Asynchronous or not, essentially, unit testing is about making assertions. We unit test by invoking a method and passing in known parameters and observing the outcome. We might expect the method to return a simple value, or another object, like a Date Type, with certain attributes set to certain values. Or, we might expect the method to fail and raise an exception, or to succeed and raise and event.

Since we're talking JavaScript, YUI not only supports all the usual assertions, but type cohersion to boot.
> var oTestCase = new YAHOO.tool.TestCase({
> 
> name: "TestCase Name",
> 
> testEqualityAsserts : function () {
> 
> var Assert = YAHOO.util.Assert;
> 
> Assert.areEqual(5, 5); //passes
> 
> Assert.areEqual(5, "5"); //passes
> 
> Assert.areNotEqual(5, 6); //passes
> 
> Assert.areEqual(5, 6, "Five was expected."); //fails
> 
> }
> 
> });
For more precise testing, YUI Test provides for Sameness Assertions.
> var oTestCase = new YAHOO.tool.TestCase({
> 
> name: "TestCase Name",
> 
> testSamenessAsserts : function () {
> 
> var Assert = YAHOO.util.Assert;
> 
> Assert.areSame(5, 5); //passes
> 
> Assert.areSame(5, "5"); //fails
> 
> Assert.areNotSame(5, 6); //passes
> 
> Assert.areNotSame(5, "5"); //passes
> 
> Assert.areSame(5, 6, "Five was expected."); //fails
> 
> }
> 
> });
Since JavaScript is loosely typed, asserting data types is a common need. YUI Test provides assertions for all the usual suspects; Array, Boolean, Function, Number, Object, String. To close the loop, an <span style="font-family:courier new, monospace;">isTypeOf</span> assertion interprets the data type as string.
> var oTestCase = new YAHOO.tool.TestCase({
> 
> <span style="font-family:courier new, monospace;"> name: "TestCase Name",</span>
> 
> testTypeOf : function () {
> 
> var Assert = YAHOO.util.Assert;
> 
> Assert.isTypeOf("string", "Hello world"); //passes
> 
> Assert.isTypeOf("number", 1); //passes
> 
> Assert.isTypeOf("boolean", true); //passes
> 
> Assert.isTypeOf("number", 1.5); //passes
> 
> Assert.isTypeOf("function", function(){}); //passes
> 
> Assert.isTypeOf("object", {}); //passes
> 
> Assert.isTypeOf("undefined", this.blah); //passes
> 
> Assert.isTypeOf("number", "Hello world", "Value should be a number."); //fails
> 
> }
> 
> });
A companion assertion, <span style="font-family:courier new, monospace;">isInstanceOf</span>, can be used to evaluate object types (Array, Function, Object). Other assertions are available for evaluating JavaScript-isms like NaN, Undefined, and truthiness.

Finally, YUI Test also supports date and time assertions, forced failures, skipping tests, and test suites.

#### Push the Envelope

Aside from exposing the usual xUnit API, YUI Test goes a step further, and provides support for _UserActions_ and _Asyncronous Tests_.

*   **User Actions** are simulated user-initiated events that can be used to test how scripts react to mouse or keyboard events.
*   **Asyncronous Tests** can be programmed to pause for a certain amount of time (while an out of process action occurs), or to pause until an event handler in the test script calls a "resume" method.
Whether you've outgrown JsUnit, or whether you're finally ready to start unit testing JavaScript for the first time, be sure to give YUI Test a try.

#### More about YUI Test

*   [Writing Your First YUI Application](http://www.insideria.com/2008/05/writing-your-first-yui-applica.html), [<span style="color:#0066cc;">Eric Miraglia</span>](http://www.oreillynet.com/pub/au/3447) (2008 May).

#### More about JsUnit

*   [<span style="color:#0066cc;">AJAX and Unit Testing - it's time to mingle</span>](http://www.litfuel.net/plush/?postid=117 "Permanent Link: AJAX and Unit Testing - it"), Jim Plush (2006 Feb),
*   [<span style="color:#0066cc;">Ajax and Unit Testing Part Two, The Wrath of Mock</span>](http://www.litfuel.net/plush/?postid=154 "Permanent Link: Ajax and Unit Testing Part Two, The Wrath of Mock"), Jim Plush (2006 Nov).

#### More about Open Source Testing Tools

*   [Open Source Testing Tools Site](http://www.opensourcetesting.org/)
*   [Test Infected - The Seminal Tutorial by Beck and Gamma](http://junit.sourceforge.net/doc/testinfected/testing.htm)