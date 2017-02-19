---
title: 'MVC Convergence: Mr Action meet Ms ASP.NET'
id: 185
categories:
  - Uncategorized
date: 2007-03-26 05:00:00
tags:
---

Over the years, there's been a steady trend within Microsoft example applications. The trend has been away from what Java geeks calls Model 1 and toward Model 2 development. It's been a forced, dragged-by-the-hair trend. At every step, the architect seems to be looking for alternatives to MVC/Model 2, and every time, the alternative comes up short, and the next iteration is one step closr to Java-style MVC.

The trend is clear if you spend a day reviewing the various ASP.NET application that were developed for ASP.NET 1.x. The trend is even more evident in ASP.NET 2.X. The platforms now offers an actual business logic component that can also be used to separate the data access code from the rest of the application. The only problem is that the component is still embedded in the web application, and, as far as I can tell, these "business logic" components can't be tested outside of the web application. Though, it is most definitely another baby step in the right direction.

Rumor now has it that Microsoft is ready for another step toward MVC/Model 2\. In his CodeBetter blog, Jeffrey Palmero outlines a new feature that Scott Guthrie is supposedly developing for the next generation of ASP.NET.

[According to the blog](http://codebetter.com/blogs/jeffrey.palermo/archive/2007/03/16/Big-News-_2D00_-MVC-framework-for-ASP.NET-in-the-works-_2D00_-level-300.aspx):
> * * *
> 
> A url might look like http://localhost/myApp/ShoppingCart.mvc/CheckOut where ShoppingCart is the Controller class and "CheckOut" is a method like:
> 
>     [Action]
>     public void CheckOut(){
>      //do so
> 
> }
> 
> The MVC model can be used with regular control-based pages. The url determines the handler that's activated, so it can be mixed an matched. In fact, a Controller could dispatch a view that _uses_ some controls. i.e. Telerik controls could still be used without Telerik modifying them. From what I saw, no capabilities are taken away, just more options are added.
> 
> In this prototype, the controller loads the appropriate view by relative path to the .aspx. The view can be only markup - pure template, or it can take advantage of controls. This can work without postbacks, or you can use postbacks if you prefer.
> 
> The view has to inherit from PageView or if it needs properties from the controller (like the Model to display), it can inherit from PageView where T is the type of the controller it depends on. Then the view can grab specific properties off of the controller.
> 
> I would recommend making "T" an interface so that the view doesn't bind directly to the controller type. I've been collaborating with Scott Guthrie to work out the kinks. There is still a lot of work to do. This is only in prototype form.
> 
> * * *
Of course, Microsoft has been warming up to [MVC and front controllers](http://msdn2.microsoft.com/en-us/library/ms998516.aspx) patterns for some time. This is not [the first time we've heard that MVC or front controller will be in the "next" version of ASP.NET](http://msdn2.microsoft.com/en-us/library/aa478961.aspx).

But, not to worry. If Microsoft comes out with a MVC technology, we will do what we always do ... reinvent it as a JSR.