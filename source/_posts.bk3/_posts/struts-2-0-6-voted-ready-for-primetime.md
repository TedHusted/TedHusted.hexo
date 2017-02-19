---
title: Struts 2.0.6 Voted Ready for Primetime
id: 209
categories:
  - Uncategorized
date: 2007-03-02 06:00:00
tags:
---

I may be late to the dance, but, let me cut in and whisper in your ear: "Struts 2.0.6 is General Availability." How's that for a sweet something?

The GA vote went down just over a week ago, on 22 February 2007\. In retrospect, the Struts 2.0.1 release was also ready for primetime. Many people have been joyfully using 2.0.1 in production since September. The holdup was that our major dependency, XWork 2, stubbornly clung to beta status from September to January. The actual code was fine, but the XWork committers wanted to spruce up the documentation and did not want to stamp the distribution "final" yet. Since then, the XWork group has changed to a milestone release system, as Struts uses. Now, we can grade both Struts and XWork releases based on experience rather than expectation.

Whilst XWork 2 twiddled with documentation, Struts 2 did not stand still. Struts 2.0.6 boasts several new heavyweight features, like plugins and a zero-configuration approach (Yes, Virginia, there is no XML!). We even moved the internal dependency injection over to Crazy Bob Lee's new secret project, [Google Guice](http://code.google.com/p/google-guice/).

Of course, by "we", I mean our star committer Don Brown. We were going to hold off on some of these changes until after our first GA, but as an expectant father, Don was worried that the bundle of joy would cut into his committing time. So, Don managed to put aside his Nintendo Wii long enough to commit a stunning array of new features. For more about the many joyful features of Struts 2.0.6, pop by the [Apache Struts website](http://struts.apache.org/).

But, wait, there's more! The Struts 2.0.7 release might be available next week. No earthshaking new features, just the usual successive refinements and odd code fix. Of course, it will be a drop-in binary replacement for Struts 2.0.6, so there's no reason to wait!

For Struts 2.1.x, we planning to migrate both the portlet support and the Ajax/Dojo tags to plugins. Since we're now using the Yahoo User Interface Library (YUI) at work, I'm also thinking we might be able to offer a Ajax/YUI plugin too. But, that's another post. :)

Meanwhile, Struts 1.3.x is ramping up for another release, and there's continued talk of a Struts 1.4\. Test builds for Struts 1.3.7 are available, and a 1.3.8 build should follow on its heels to correct an oversight with Action redirects.

As always, the best is yet to come!