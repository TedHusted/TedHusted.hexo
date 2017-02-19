---
title: Unloading Salesforce CRM Data with Outbound Messages
id: 41
categories:
  - Uncategorized
date: 2012-03-22 05:00:00
tags:
---

<div class="separator" style="clear:both;text-align:center;">[![](http://www.clker.com/cliparts/6/9/f/3/11971239071443233733nicubunu_Message_in_a_Bottle.svg.med.png)](http://www.clker.com/)</div>No software application is an island, and Salesforce provides two great mechanisms for getting data in and out of your org. To get bits in, there's the Data Loader and other tools that use the same APIs. To get data out, Salesforce provides a system for pushing outbound messages to an external web service.

Whenever data is edited, or some other internal event occurs, a Salesforce workflow can queue an outbound message to another system.

While there are several off-the-shelf tools for loading data, implementing an outbound message listener is still a custom affair that requires database and web development skills. If you don't mind exposing an end-point to pass data to a stored procedure, then read on ..  

<a name='more'></a>There are plenty of devils in the details, but here's the basic 1-2-3-4: 

1.  A workflow rule queues an outbound message. 
2.  The outbound message contains a bundle of fields from one of your Salesforce objects. 
3.  A web service endpoint (that you create) receives the message and updates an external system (that you control). 
4.  Optionally, the service responds and updates the original recordThe linch pin is step 3\. While there are tools that simplify writing web services, you still need to write some code. The Salesforce API Developers Guide provides an [example of a simple listener class](http://www.salesforce.com/us/developer/docs/api/Content/sforce_api_om_outboundmessaging_listener.htm), but it's really a bit too simple.
<div>
<pre>classMyNotificationListener : INotificationBinding
{
    publicnotificationsResponse notifications(notifications n)
    {
        notificationsResponse r = newnotificationsResponse();
        r.Ack = true;
        return r;
    }
}
</pre>
The notificationsResponse method is the end-point for a SOAP web services call. The message bundle is passed over as a "notification".  If records are being updated in bulk (say by a data loader), Salesforce may include up to 100 notifications in a single SOAP envelope.   A better example of a notificationResponse would be: 

<pre><span class="Apple-style-span">classMyNotificationListener : INotificationBinding
{
    publicnotificationsResponse notifications(notifications n)
    {
        foreach (AccountNotification message in n.Notification)
        { 
            // … handle message …</span><span class="Apple-style-span">
        }
        notificationsResponse r = newnotificationsResponse();
        r.Ack = true;
        return r;
    }
}
</span></pre>
Something to consider when handling a message is that Salesforce may send the same message multiple times. Each message has an ID, and if handling the same message twice would be a bad thing, you should cache the ID and check it before processing. Also, in real life, we should catch exceptions and log errors. 

<pre>log4net.ILog log = log4net.LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);
foreach (AccountNotification message in n.Notification)
{ 
  sf = message.sObject;
  try
  {
    // -- Have we met? --
    if (HttpRuntime.Cache[message.Id] != null)
    {
      log.Warn(string.Format("Warn: SKIPPING (sf.id=\"{0}\") in message (id=\"{1}\")", sf.Id, message.Id));
      continue;
    }
    lock (cacheLock)
    {
      // The cache will only last as long as the App Pool
      HttpRuntime.Cache.Insert(message.Id, sf.Id, null, System.Web.Caching.Cache.NoAbsoluteExpiration, TimeSpan.FromHours(2));
      log.Info(string.Format("Info: PROCESSING (sf.id=\"{0}\") in message (id=\"{1}\")",sf.Id, message.Id));
    }

    // … handle message … 
  } 
  catch (Exception ex)
  {
    // Catch any exceptions raised in foreach loop
    String errorMessage = ex.Message + " --- " + ex.StackTrace;
    log.Error(String.Format("Error: EXCEPTION (sf.id=\"{0}\", error=\"{1}\"", sf.Id, ex.Message + " --- " + ex.StackTrace));
        }
} // end foreach loop
</pre>
When developing an outbound message service, you should test it under maximum load. What will happen if someone uses the Apex Dataloader to touch every record in an object, potentially creating an outbound message for every record? If that happens, Salesforce is going to collect the bundles into envelopes of 100 records each, and send them down to your web service as quickly as it can. If your service can't keep up, Salesforce will try again later, in increasing intervals, for up to 24 hours.  

Under "Setup/Monitor/Outbound Messages", Salesforce provides a display where you can watch the records pass through in real time. If in your tests, you see Salesforce binding up, it's a good idea to queue the records, and process them at your system's pace.  

For .NET, one approach would be to spread-out the work using a ThreadPool, but a better approach can be to use a Producer/Consumer Queue. (The example code is written for .NET 3.5\. A 4.0 implementation may differ. See [C# 4.0 in a Nutshell](http://www.amazon.com/gp/product/0596800959/ref=as_li_ss_tl?ie=UTF8&amp;tag=husteddotcom-20&amp;linkCode=as2&amp;camp=1789&amp;creative=390957&amp;creativeASIN=0596800959) for more examples and background information.) 

<pre>public notificationsResponse notifications(notifications n)
{
  log4net.ILog log = log4net.LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);
  if (myQueue == null) lock (queueLock)
  {
    // Double-check pattern
    if (myQueue == null) myQueue = new AccountQueue();
  }
  log.Info("Info: Enqueuing and acknowledging notifications");
  myQueue.EnqueueTask(n);
  notificationsResponse r = new notificationsResponse();
  r.Ack = true;
  return r;
}

// Producer/Consumer Queue to throttle account upserts notifications 
// and prevent swamping of server during DataLoader updates.  
// http://www.albahari.com/threading/part2.aspx#_WaitHandle_Producer_Consumer_Queue
class AccountQueue : IDisposable
{
  EventWaitHandle _wh = new AutoResetEvent(false);
  Thread _worker;
  readonlyobject _locker = newobject();
  Queue _tasks = new Queue();
  public AccountQueue()
  {
    _worker = new Thread(Work);
    _worker.Start();
  }
  publicvoid EnqueueTask(notifications task)
  {
    lock (_locker) _tasks.Enqueue(task);
    _wh.Set();
  }
    publicvoid Dispose()
    {
        EnqueueTask(null);     // Signal the consumer to exit.
        _worker.Join();        // Wait for the consumer's thread to finish.
        _wh.Close();           // Release any OS resources.
    }
    void Work()
    {
    while (true)
    {
      notifications task = null;
      lock (_locker)
        if (_tasks.Count &gt; 0)
        {
           task = _tasks.Dequeue();
           if (task == null) return;
        }
        if (task != null)
        {
          doWork(task);    // Handle message here
        }
        else
          _wh.WaitOne();   // No more tasks - wait for a signal
      }
   }
}

static readonly private object cacheLock = newobject();
static public void doWork(notifications n) {
     // ... code from prior example ...
}
</pre>
Along the way, we slipped in a reference to Log4Net. Configuring Log4Net is outside the scope of this article, but you can dig into the gory details at [logging.apache.org](http://logging.apache.org/). Highly recommended! 

While our non-trivial example has grown quickly, it does provide the backbone for a robust Listener that can keep pace with Salesforce, even when you are stuck with a pokey API or under-powered server.

In a [followup blog](http://tedhusted.blogspot.com/2012/03/catch-me-on-flip-side-responding-to.html), we close the loop by showing how a listener can respond back to Salesforce.

</div>