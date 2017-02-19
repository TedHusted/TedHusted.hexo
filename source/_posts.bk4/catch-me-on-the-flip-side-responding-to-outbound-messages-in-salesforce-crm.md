---
title: 'Catch me on the flip-side: Responding to Outbound Messages in Salesforce CRM'
id: 39
categories:
  - Uncategorized
date: 2012-03-29 05:00:00
tags:
---

<div class="separator" style="clear:both;text-align:center;">[![](https://farm2.staticflickr.com/1130/1084139169_e272bc1f81_z.jpg)](https://www.flickr.com/photos/spacemanbob/1084139169)</div>
In a [previous blog](http://tedhusted.blogspot.com/2012/03/unloading-salesforce-crm-data-with.html), we walked through example code for an Outbound Message Listener. We covered the essentials, but Salesforce CRM offers a nifty twist on just sending an outbound message to an external system. Not only can a system receive a message, it can send back a response. For example, if the outbound message inserts a record, the external system can send back the record's key.

<a name="more"></a>Our example Listener called a worker method that handled the messages in a foreach loop. Using the case of returning an external key, we can add the keys to a hashtable, and send back a batch at the end all of the responses. We'll pickup the live action towards the end of the foreach loop from the [previous example](http://tedhusted.blogspot.com/2012/03/unloading-salesforce-crm-data-with.html).
<pre> **Hashtable responsePayload = newHashtable();  // Returns IDs to SF**
 foreach (AccountNotification message in n.Notification)
 {
     sf = message.sObject;</pre>
<pre>     // -- Check cache --
     // …
     // -- handle message --
     // …
     // Queue ID for return to SF
     **responsePayload.Add(sf.Id, my.Id);**
 } // end foreach loop
 // -- Return IDs to SF --
 **log.Info("Info: Sending response to Salesforce ...");**
 **Response response = new Response();**</pre>
<pre> **log.Info(response.Send("Account", responsePayload));**
 return;
}</pre>
The Response class is a wrapper around a <span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">SForceService</span> instance which returns a String we can use as a logging statement.
<pre>class Response
{ 
    private SforceService service;
    public Response() { }
    public String Send(String objectName, Hashtable payload)
    {
        return this.Send(objectName, payload, "External_Id__c");
    }
    public String Send(String objectName, Hashtable payload, String syncFieldName)
    {
        if (payload == null || payload.Count == 0) return</pre>
<pre>            "No records updated. Response.Send() not needed.";
        try
        {
            service = new SforceService();
            LoginResult loginResult = service.login(
                ConfigurationManager.AppSettings["Salesforce.Username"],
                ConfigurationManager.AppSettings["Salesforce.Password"] + 
                ConfigurationManager.AppSettings["Salesforce.Token"]
            );
            service.SessionHeaderValue = new SessionHeader();
            service.SessionHeaderValue.sessionId = loginResult.sessionId;
            service.Url = loginResult.serverUrl;
            GetUserInfoResult userInfo = loginResult.userInfo;
            List sObjects = new List();
            sObject s = new sObject();
            XmlDocument xmlDocument = new XmlDocument();
            List elements;
            XmlElement element;
            foreach (String sfid in payload.Keys)
            {
                s = new sObject();
                s.Id = sfid;
                s.type = objectName;
                elements = newList();
                element = xmlDocument.CreateElement(syncFieldName);
                element.InnerText = ht[sfid].ToString();
                elements.Add(element);
                s.Any = elements.ToArray();
                sObjects.Add(s);
            }
            SaveResult[] saveResults = service.update(sObjects.ToArray());
            String output = String.Empty;
            foreach (SaveResult sr in saveResults)
            {
                if (sr.success)
                {
                    output += String.Format(</pre>
<pre>"{0} Response Sync Success : sf.id=\"{1}\"", objectName, sr.id) + "\n";
                }
                else
                {
                    foreach (Error err in sr.errors)
                    {
                        output += String.Format(</pre>
<pre>"{0} Response Sync Error : sf.id=\"{1}\", {2}", objectName, sr.id, err.message) +</pre>
<pre>"\n";
                    }
                }
            }
            return output;
        }
        catch (Exception ex)
        {
            return ex.Message + " --- " + ex.StackTrace;
        }
    }
}</pre>
The credentials and API service endpoint are declared in the Web.Config file.
<pre></pre>
https://test.salesforce.com/services/Soap/u/24.0

(In a production environment, replace "test.salesforce.com" with "www.salesforce.com".)

Some caveats are

(1) The response needs credentials to login, and you might need to setup a system account for the response.

(2) To keep the message from recursing, the Response User needs to have outbound messaging disabled, or be specifically excluded from the Outbound Messaging Workflow.

(3) If your listener implements a producer/consumer queue, the response may not be immediate. (If tens of thousands of records are being updated by a slow system, it could be an hour or more!).

(4) An outbound message response may update the record while it's being viewed by another user. If the user makes another change without refreshing the record, Salesforce will decline the second change, and the human user will have to refresh and repeat.

By allowing a response to an outbound message, Salesforce gives us the opportunity to close the loop. For example, if a record is being inserted into an external system, the response could return the external ID, for use with future updates.

While not a "point-and-click" product, like the Apex Dataloader, the outbound messaging API provides developers all the hooks we need to create a full-service data connection.