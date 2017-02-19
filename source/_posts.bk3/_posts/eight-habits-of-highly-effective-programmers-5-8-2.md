---
title: Eight Habits of Highly Effective Programmers (5-8)
id: 146
categories:
  - Uncategorized
date: 2007-08-13 05:00:00
tags:
---

[Last time](http://www.jroller.com/TedHusted/entry/habits), we covered habits 1-4: **Estimate** (Be Proactive), **Test-First** (Begin with the End in Mind), **Iterate** (Put First Things First), and **Narrate** (Seek first to understand, and then to be understood).

*   Refine (Think Win/Win)
Stable requirements are the holy grail of software development. If we do pursue the myth of stable requirements, clients and developers both lose. We may ship what the requirements describe, but we probably won't ship what the client really wants and needs.

When building business applications, a better approach is successive refinement. Rather than try to define in detail the application all at once, we first define and implement a key workflow. Through a series of iterations, we continue to extend and refine the workflows that comprise the application. The goal of each iteration is to provide client with functioning application that does useful work.

Successive refinement encourages clients and developers to act as collaborators, rather than adversaries, wrangling over requirements.

*   Reuse (Synergize)
A worker is only as good as his or her tools. As we create software that works, we look for ways to reuse that code in other projects. Few applications have totally unique requirements. Every application overlaps with the next. If arrange a codebase so that reusable components are separated from unique components, we usually increase cohesion and decrease coupling, creating software that is easier to change, extend, and reuse. By writing software so that it is easy to use more than once, we often create software that is easier to use even once.

*   Refactor (Sharpen the Saw)
Techniques like Test-First and Iterate help us write good code quickly, but great code still takes time and effort. As we verify that code is effective and relative, we can use our tests to help us refactor, or improve the design of existing code.

Well-designed code is easy to test and easy to change. Because we know new features will be easy to add, well-designed code doesn't need "hooks" for future development. Code becomes an asset that pays dividends today and tomorrow. Refactoring is another win/win, since it helps us create better code today and happier clients tomorrow.

*   Mentor
It's said that we never truly understand a task until we teach it to someone else. Teaching creates a dialog between instructor and student. When the dialog is healthy, both student and instructor benefit. Teaching forces us to organize and prioritize our own knowledge, clarifying the task in our own minds. Clever students often ask questions we never thought to ask, and in finding the answers, we ourselves come to learn more. For me, writing this article is a case in point!

* * *

To learn more about programming best practices, see [ The Pragmatic Programmer](http://www.amazon.com/exec/obidos/tg/detail/-/020161622X/husteddotcom-20) and [Software Craftsmanship](http://www.amazon.com/exec/obidos/tg/detail/-/0201733862/husteddotcom-20). Be guided by the humble observation of an extreme programmer: "I'm not a great programmer. I'm a good programmer with great habits."

For a similar treatment of the "12 Steps", see "[12 steps to stellar software (usability) design](http://www.computerworld.com/action/article.do?command=viewArticleBasic&amp;articleId=9049262&amp;intsrc=news_ts_head)".