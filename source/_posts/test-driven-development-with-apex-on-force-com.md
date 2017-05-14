---
title: Test Driven Development with Apex on Force.com
id: 50
categories:
  - Review
date: 2012-03-04 06:00:00
tags:
 - Force.com
---

<div class="separator" style="clear:both;text-align:center;">[![](https://tedhusted.files.wordpress.com/2012/03/968d3-carrot-with-stick.jpg)](http://teachingunderground.blogspot.com/2011/04/catching-carrot-and-breaking-stick.html)</div>
As far as I know, Force.com -- the software development platform for Salesforce CRM -- is the only platform that **requires** unit test coverage for production code. Before an Apex developer can deploy custom code to a production environment, the overall unit test coverage for the environment must be 75% or better.

<span style="font-size:large;">**<span style="font-size:small;">What is Unit Test Coverage?</span>**</span>

Let's look at a simplistic, contrived example of unit test coverage. For demonstration purposes, the <span style="font-family:'Courier New', Courier, monospace;">HelloWorld</span> Class shows an Apex function that tests whether the text of a string matches "Hello World" or not.
<a name="more"></a><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">// HelloWord Class</span></span>

<span style="font-family:'Courier New', Courier, monospace;">public class HelloWorld {</span><span style="font-family:'Courier New', Courier, monospace;">
public static String isHelloWorld(String myString) {</span>
<span style="font-family:'Courier New', Courier, monospace;">    if (myString.equals('Hello World')) return 'Yes it is!'; **// Line 1**</span>
<span style="font-family:'Courier New', Courier, monospace;">    else return 'No it is not!': **// Line 2**</span>
<span style="font-family:'Courier New', Courier, monospace;">  }</span>
<span style="font-family:'Courier New', Courier, monospace;">}</span>

The <span style="font-family:'Courier New', Courier, monospace;">verifyIsHelloWorld</span> test method exercises our gratuitous example

<span style="font-family:'Courier New', Courier, monospace;font-size:x-small;">// verifyIsHelloWorld test method</span>

@isTest
private class TEST_HelloWorld {
static testMethod void verifyIsHelloWorld () {
String outcome = HelloWorld.isHelloWorld ('Hello World');
System.assert(outcome == 'Yes it is!', 'Expected positive message.') ;
}
}

At this point, we have 66% test coverage, because our test exercises only one of the two statements in <span style="font-family:'Courier New', Courier, monospace;">isHelloWorld</span> (line 1).

To bring test coverage up to 100%, we need to add another test method.
<div><span style="font-size:x-small;"> </span></div>
<div>

<span style="font-size:x-small;">// Another test method</span>

static testMethod void verifyIsHelloWorldFalse() {
String outcome = HelloWorld.isHelloWorld ('Hello Kitty');
System.assert(outcome == 'No it is not!', 'Expected negative message.') ;
}

</div>
With both of these test methods in play, our code now has 100% coverage.

**Why is test coverage important?**

Salesforce.com has high standards for its own code and expects custom Apex code to also be robust and error-free. One of the best ways to increase code quality is to encourage developers to write unit tests. [Witness a 2005 study found that unit tests increased both coder productivity and code quality.](http://nparc.cisti-icist.nrc-cnrc.gc.ca/npsi/ctrl?action=shwart&amp;index=an&amp;req=5763742&amp;lang=en)

While it's possible for developers to boost code coverage with pointless tests, hardcore coders see unit tests as a way to release better code sooner -- the keyword being "release". The time spent on proactive unit testing is a trade-off with the time spent on reactive debugging. We can find our own bugs ourselves with unit tests, or wait and fix them later when a feature comes back with a QA ticket attached.

In my own work, I've found that the best way to ensure that code has adequate test coverage to practice test-driven development (TDD).

**What is Test Driven Development (TDD)?**

For the uninitiated, a classic way to bootstrap unit testing (and TDD) is to start with defect reports. Before fixing a bug, a developer first writes a test that proves that the defect exists. For example, if someone reports that <span style="font-family:'Courier New', Courier, monospace;">isHelloWorld</span> fails if we pass in a null string, we could start with a test like the one shown by <span style="font-family:'Courier New', Courier, monospace;">verifyIsHelloWorldNull</span>.

<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">// verifyIsHelloWorldNull</span></span>

static testMethod void verifyIsHelloWorldNull() {
String outcome = HelloWorld.isHelloWorld (null);
System.assert(outcome == 'No it is not!', 'Expected negative message.') ;
}
<div style="font-family:inherit;"></div>
<div style="font-family:inherit;"><span style="font-size:x-small;"> </span></div>
If we run this test, it raises an exception “System.NullPointerException: Attempt to de-reference a null object”.

Since an exception counts as a failing test, we can proceed with the fix, say, by changing the code from:

| <span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">if (myString.equals('Hello World')) return 'Yes it is!'; **// Line 1**</span></span>

to:

| <span style="font-family:'Courier New', Courier, monospace;font-size:x-small;">if 'HelloWorld'.equals('myString') return 'Yes it is!'; **// Line 1**</span>

and maybe, for good measure, including a fourth test case for an empty string.
<div>

<span style="font-size:x-small;">// Test for empty String</span>

final static String EMPTY = '';
static testMethod void verifyIsHelloWorldEmpty() {
String outcome = HelloWorld.isHelloWorld (EMPTY);
System.assert(outcome == 'No it is not!', 'Expected negative message.') ;
}

</div>
<span style="font-family:inherit;">Once all of our tests are passing, we could even refactor the code, and improve the internal design by using a constant and a single comparison.</span>
<div>

<span style="font-size:x-small;">// Code refactored</span>

public class HelloWorld {
public final static String HELLO_WORLD = 'Hello World';
public final static String YES_WORLD = 'Yes it is!';
public final static String NO_WORLD = 'No it is not!';
public static String isHelloWorld(String myString) {
return (HELLO_WORLD.equals(myString)) ? YES_WORLD : NO_WORLD;
}
}

</div>
<span style="font-family:inherit;">If our tests pass (they do), we can be confident that our refactoring did not break the code's external behavior. Passing tests give us the courage to refine existing code and improve the internal design.</span>

The key idea behind TDD is to "never write a line of code without a failing test". If we are going to write the test anyway, better to write it first, code to the test, and receive full benefit for the time we invest.

**How do we test code that doesn't exist?**

In an Apex environment, a unit test usually operates at the class level. To bootstrap testing a class or method that does not exist, we can start coding the test, create a stub class with stub methods, sufficient to compile the test, confirm that it fails, and then fill-in functionality to pass the test.

Let’s start over from scratch. First, we should define our requirements for the <span style="font-family:'Courier New', Courier, monospace;">isHelloWorld</span> method.

<span style="font-family:inherit;"> <span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">// isHelloWorld requirements</span></span></span>

/**
* The isHelloWorld method determines if a String equals 'Hello World'.
* (1) Given the String 'Hello World', the method returns "Yes, it Is."
* (2) Given some other String, the method returns 'No, it is not!'.
* (3) Given a null or empty string, the method returns 'No, it is not!'.
*/
<div style="font-family:inherit;"></div>
<div style="font-family:inherit;"><span style="font-size:small;">Then, we can write a "happy path" test for the first requirement.</span></div>
<div style="font-family:inherit;"></div>
<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">// A happy path test for requirement (1)</span></span>

final static String POSITIVE = 'Expected positive message.';
static testMethod void verifyIsHelloWorld () {
String outcome = HelloWorld.isHelloWorld ('Hello World');
System.assert(outcome == HelloWorld.YES_WORLD,POSITIVE ) ;
}<span style="font-family:inherit;"> </span>
<div style="font-family:inherit;"><span style="font-size:small;">provide a stub Hello World class to compile the test</span></div>
<div style="font-family:inherit;"><span style="font-size:x-small;"> </span></div>
<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">
// A HelloWorld stub class</span></span>

public class HelloWorld {
public static String isHelloWorld(String myString) {
return null;
}
}
<div style="font-family:inherit;"><span style="font-size:small;">add just enough behavior to pass one test for one requirement</span></div>
<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;"><span style="font-size:small;">
</span>// Coding requirement (1)</span></span>

public final static String HELLO_WORLD = 'Hello World';
public final static String YES_WORLD = 'Yes it is!';

public static String isHelloWorld(String myString) {
return HELLO_WORLD.equals(myString) ? YES_WORLD : return null;
<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">}</span></span>
<div style="font-family:inherit;"></div>
<div style="font-family:inherit;"><span style="font-size:small;">then another requirement</span></div>
<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">
// Testing requirement (2)</span></span>

final static String NEGATIVE =’Expected negative message.’;
static testMethod void verifyIsHelloWorldFalse () {
String outcome = HelloWorld.isHelloWorld ('Hello Kitty');
System.assert(outcome == HelloWorld.NO_WORLD,NEGATIVE ) ;
}

// Coding requirements (1) and (2)

public final static String HELLO_WORLD = 'Hello World';
public final static String YES_WORLD = 'Yes it is!';
public final static String NO_WORLD = 'No it is not!';

public static String isHelloWorld(String myString) {
return HELLO_WORLD.equals(myString) ? YES_WORLD : NO_WORLD;
}
<div style="font-family:inherit;"><span style="font-size:small;">and a third</span></div>
<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">  </span></span>
<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">// Testing requirement (3)</span></span>

static testMethod void verifyIsHelloWorldNull() {
String outcome = HelloWorld.isHelloWorld (null);
System.assert(outcome == HelloWorld.NO_WORLD,NEGATIVE );
}

final static String EMPTY = '';
static testMethod void verifyIsHelloWorldEmpty() {
String outcome = HelloWorld.isHelloWorld (EMPTY);
System.assert(outcome == HelloWorld.NO_WORLD,NEGATIVE );
}
<span style="font-family:inherit;font-size:small;"><span style="font-family:'Courier New', Courier, monospace;"> </span> </span>
<span style="font-family:inherit;font-size:small;">For requirement 3, we added two test methods, but did not need to change any code, since the current implementation passed the tests.</span>

To fully test the method, we might also [add a test for a string of maximum length](http://developer.force.com/cookbook/recipe/construct-random-strings-of-large-sizes-in-your-apex-tests), so that we test both boundaries. But, as it stands, we have 100% test coverage, and a test for each stated requirement, which meets my own personal "definition of done".

Three takeaways from this exercise are:

*   <span style="font-family:inherit;font-size:small;"> Never write a line of code without a failing test.</span>
*   <span style="font-family:inherit;font-size:small;"> Test every requirement, one requirement at a time.</span>
*   <span style="font-family:inherit;font-size:small;"> Passing tests give us the courage to refactor.</span>
<span style="font-family:inherit;font-size:small;"> In a followup blog, [TDD with Apex Triggers,](http://tedhusted.blogspot.com/2012/03/test-driven-development-with-apex.html) we look at how Test Driven Development works in practice, with a real-life non-trivial example.</span>
<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;"><span style="font-family:inherit;"></span></span></span>