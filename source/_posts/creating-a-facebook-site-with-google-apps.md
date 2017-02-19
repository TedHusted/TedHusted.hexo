---
title: Creating a Facebook Site with Google Apps
id: 64
categories:
  - Tutorials
date: 2011-01-07 10:00:00
tags:
 - Programming
---

[![](https://tedhusted.files.wordpress.com/2011/01/296dd-gook.png?w=300)](https://tedhusted.files.wordpress.com/2011/01/296dd-gook.png)This holiday season saw a landmark event. High profile companies,[ like Mattel](http://www.facebook.com/barbie), posting a Facebook site URL in prime-time advertising. As it turns out, you don’t need to be a big name company to have your own Facebook site. Anyone can do it, it’s free, and it’s simple as pie.

The reason it’s free and easy is that Facebook doesn’t actually host the site. Facebook provides a namepace and an iframe that points to a site you control. The pages are loaded from your server, wrapped in a Facebook pane.

There is a [Facebook JavaScript API](http://developers.facebook.com/docs/reference/javascript/) that you can use from any site. With the API, you can like pages, authenticate against Facebook, write to your wall, and such. But with a Facebook site, you can do anything that your own website can do.

If you don’t already have a website of your own, you can create a static site using [published Google Docs](http://docs.google.com/demo/%20). Just create a document, and share it with the world, and you can wrap a Facebook site around it.

Here’s how to get started.

1.  If you don’t have a Google Docs account, you can sign up at [http://docs.google.com/demo/ ](http://docs.google.com/demo/%20)and create a page to share.

    *   Select **Share **and change the visibility to public
    *   Then **Share **and **Publish **to the Web

2.  Login to Facebook. (As if you ever logout!)
3.  [Register an App Name with Facebook](http://www.facebook.com/developers/createapp.php). (It’s free!)

    *   Your Facebook account name might be a good one for starters.

4.  Under** Facebook Integration**, enter your App Name as the as the **Canvas Page**, and your public, published Google Doc page as the **Canvas URL**. . Under **IFrame Size**, select **Auto-Resiz**e.
5.  Open <u>apps.facebook.com/{Your-Canvas-URL}</u>
6.  Voila!

    *   If the apps isn’t found right away, check the URL, and wait a few minutes for it to propagate.

    *   If t still doesn’t show, open **Advanced **in your [**Apps Settings**](http://www.facebook.com/developers/apps.php), and **Disable** all the options.
For more tips about using Google Docs as a Facebook Canvas, see this Metaprime blog - [http://blog.metaprime.at/how-to-build-a-facebook-landing-page-with-google-docs/](http://blog.metaprime.at/how-to-build-a-facebook-landing-page-with-google-docs/).

You can also use a [Google Site as a Facebook Canvas](http://apps.facebook.com/ted-husted/). Just keep the site layout within a 870px width or a 740px page with a 130px sidebar. (Though, to appease the API, you might have to add an extra “?” at the end of the URL. )

Google Docs and Google Sites are a great way to maintain a static site within Facebook. For some ideas about what you can do with a full web server, visit the [World Wildlife Fund Gift Center](http://apps.facebook.com/wwfgiftcenter/) and the [Facebook Platform Showcase](http://developers.facebook.com/showcase/). Common Knowlege also has a great webinar on [Facebook Sites and Pages](http://www.commonknow.com/html/rsrcs/webinarrecordings/101117_FacebookApplications/index.htm).

Facebook integration is a great example of how difficult problems can be solved with simple interfaces. Fire up your Facebook account, and give it a try!