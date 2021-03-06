---
title: Simulating Action
id: 186
categories:
  - Review
date: 2007-03-25 05:00:00
tags:
 - Ajax
---

Since client-side JavaScript is event-driven, Ajax libraries follow suit. For many developers, Ajax is their first time around the event-driven maypole.

When event-driven controls for the first time, it's common for a noobe to attach some application-specific behavior to a control's onClick event. As the application grows more complex, we often want to invoke the same behavior without the help of the control. A common question then becomes "How can we send the control a change event, as if the user had himself clicked on the option?" As with many programming problems, the solution is to add a layer of abstraction. :)

Instead of attaching our behavior directly to primitive events, like onClick, we should create our own idiomatic events to describe what happens when the user selects the option. Selecting the option is a means to an end. If we create own custom event for that end, the select control change event can turn around and raise our event. Then, when our custom event needs to be raised from another point in the workflow, we can just go ahead and raise it progmatically.

It's not about clicking the control, it's about the action that the control triggers.

In essence, user-interface widgets raise primitive events, like "change" and "click". This is a Good Thing, because those events represent what widgets do (or appear to do). Applications don't "click". Applications update state and run reports. An event-driven application should create its own layer of idiomatic events that describes what the application does in its own terms. In that way, we are not chained to the controls, and we can call our own events whenever we like. Architectually, idiomatic events [increase cohesion](http://en.wikipedia.org/wiki/Cohesive) and [lower coupling](http://en.wikipedia.org/wiki/Coupling_%28computer_science%29). Also Good Things.

An excellent introduction to event-driven design is Christian Heilmann's [Event-Driven Web Application Design](http://yuiblog.com/blog/2007/01/17/event-plan/) (which I like to call "Brace yourself, Bridget!).