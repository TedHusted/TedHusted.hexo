---
title: Test Driven Development with Apex Triggers on Force.com
id: 48
categories:
  - Uncategorized
date: 2012-03-05 06:00:00
tags:
---

<div class="separator" style="clear:both;text-align:center;">[![](http://upload.wikimedia.org/wikipedia/en/b/ba/DrawingHands.jpg)](http://en.wikipedia.org/wiki/Drawing_Hands)</div>[In an earlier blog](http://tedhusted.blogspot.com/2012/03/test-driven-development-with-apex-on.html), we examined a simple example of Test Driven Development (TDD). Here, we dive into a real-life example of using TDD to develop production Apex code for Salesforce CRM. 
**
How does TDD work in practice?**

Let's walk through a non-trivial example of using test-driving development to craft a complex Apex trigger.

**Problem: **An off-the-shelf integration requires the existence of a specific Salesforce CRM Opportunity object. When the object is present, the integration acts on the Account related to the Opportunity. When the object is not present. the integration bypasses the Account.

<a name='more'></a>**User Story:** As a CRM User, I need to easily manage the Opportunity that signals the integration, for example, by selecting a checkbox that creates or removes the related integration object.

**Solution:** Provide an Account trigger that observes the checkbox and inserts or deletes the related integration object.

**How do we test an Apex trigger?**

In the case of an Apex trigger, we can't invoke the behavior directly. The purpose of a trigger is to create a side effect when we insert, update, or delete objects. Basically, we do something like

| <span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">Account a = new Account(name = 'test',);</span></span>
| <span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">insert(a);</span></span>

and observe the state changes.

(Note: Another approach is to have the trigger call a worker class, and then write tests against the worker class. Here, we are using the direct approach, and keeping the trigger code in the trigger.)

As we saw in Part 1, a good place to start any coding exercise is to clearly define the expected state change. We can then code tests against the expected state changes, call the relevant DML operations (insert, update, and/or delete), and observe the outcome. (DML = Database Manipulation Language.)

Let's express our expectations (or requirements) in the form of a documentation comment for the trigger under test. 
<div><span style="font-size:x-small;">
</span></div><div><span style="font-size:x-small;">// Integration Account trigger requirements

/**
 * Manages a2z Opportunity by referring to the "Do Import" checkbox on the Account object (NU_isA2zImport__c).
 * When an a2z Opportunity exists, the Account is imported to a2z.
 * The trigger handles three transitional states:
 * (1) if insert and doImport, insert opportunity;
 * (2) if update and !doImport and hasOpp, delete opportunity;    
 * (3) if update and doImport and !hasOpp, insert opportunity.
 * By inference, the trigger also handles:
 * (4) if insert and !doImport, exit; 
 * (5) if update and doImport and hasOpp, exit;
 * (6) if !doImport and !hasOpp, exit.
 */</span></div>
To start coding the trigger that implements this logic, according to test driven development, we need a failing test. Let's start with (1) and write a test for the Insert case. Since this is a non-trivial example, there is some scaffolding for the test.

<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">// Test for Insert case

final static Id A2Z_RECORD_TYPE_ID = Schema.SObjectType.Opportunity.
  getRecordTypeInfosByName().get('a2z').getRecordTypeId();
final static String A2Z_STAGE_NAME = 'Closed Won';

/**
 * Exercises "Insert Import" (1) by inserting an new Account with the checkbox set,
 * and observing whether a corresponding Opportunity is created.
 */
static testMethod void testInsImp() {
       // Bootstrap a test account, passing in values for needed fields.
       Account a = new Account(name = 'test', NU_isA2zImport__c=true);
       // Insert and Select the test Account
       insert(a);
       // Do we have an a2z opp?
       Integer opps = [
                SELECT COUNT()
                FROM Opportunity
                WHERE AccountId = :a.id
                      AND  RecordTypeId = :A2Z_RECORD_TYPE_ID
                      AND StageName = :A2Z_STAGE_NAME
           ];
       Boolean hasOpp = (opps&gt;0);
       System.assert(hasOpp,'Expected opp on insert.');
}</span></span>
<div style="font-family:inherit;">
</div><div style="font-family:inherit;"><span style="font-size:small;">When we run this test, it fails, because no one has written a trigger to insert the opportunity when the checkbox is true. Next!</span></div><div style="font-family:inherit;">
</div><div style="font-family:inherit;"><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">// Code for the Insert case

/**
 * Manages a2z Opportunity by referring to the "Do Import" checkbox 
 * on the Account object.
 * …
 */
trigger NU_a2zCreateOpportunity on Account (after insert, after update) {

    final static Id A2Z_RECORD_TYPE_ID = Schema.SObjectType.Opportunity.
      getRecordTypeInfosByName().get('a2z').getRecordTypeId();
    final static String A2Z_STAGE_NAME = 'Closed Won';

    // (1) On insert, if isImport, insert opp
    if (Trigger.isInsert) {
            List delta = new List();
            for (Account a : Trigger.new) {
                if (a.NU_isA2zImport__c) {
                    Opportunity o = new Opportunity(
                            AccountId = a.Id,
                            Name = 'a2z',
                            RecordTypeId = A2Z_RECORD_TYPE_ID,
                            StageName = A2Z_STAGE_NAME,
                            CloseDate = Date.Today()
                    );
                    delta.add(o);
               }       
       }
       if (delta.size()&gt;0) insert(delta);
}</span></span></div><div style="font-family:inherit;">
</div><div style="font-family:inherit;">When we run the test again, it succeeds, because we now have the trigger code that inserts the a2z Opportunity.

(Note: If you haven't written Apex triggers, the for-loop might seem odd. As a performance tweak, Apex triggers work with batches. Most times, it's a batch of one. But, if data is being imported, a batch could contain 200, or even 2000, objects to insert, update, or delete.)

Since unit testing is baked into Apex, we don't need to undo any database operations that occur with an actual testMethod. While the testMethod is running, the trigger can insert all the opportunities it likes, but when the test ends, Force.com rolls it all back for us. No fuss. No muss. No left-over cruft.

Looking back at the code, I see things I don't like. There are redundant constants, and we are also doing a lot of heavy lifting inline, obscuring the flow of the code. Since we have a passing test, let's improve the design, and run the test again.

First, since we will need to share constants and helpers between the tests and the code-under-test, let's extract existing code into a static helper class.</div><div style="font-family:inherit;">
</div><div style="font-family:inherit;"><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">// Extracting a utility class to share test and domain code.

/**
 * Encapsulates tools used by a2z Opportunity test and domain code.
 */
public Class NU_a2zOpps {

    /**
     * Defines an a2z Opp with a particular Record Type ID and Stage Name.
     * (TODO: Transfer to custom settings and expose to validation rule.)
     */
    final static public String A2Z_STAGE_NAME = 'Closed Won';
    final static public Id A2Z_RECORD_TYPE_ID = Schema.SObjectType.Opportunity.
      getRecordTypeInfosByName().get('a2z').getRecordTypeId();

    /**
     * Declares a default name for generated opportunities.
     */
    static public String A2Z_NAME = 'a2z Import';

    /**
     * Returns a ready-to-use a2z Opportunity.
     */
    static public Opportunity newOpp(Account a) {
            return new Opportunity(
                    AccountId = a.Id,
                    RecordTypeId = A2Z_RECORD_TYPE_ID,
                    Name = A2Z_NAME,
                    StageName = A2Z_STAGE_NAME,
                    CloseDate = Date.Today()           
            );
    }

    /**
     * Determines if a given Account has an A2z Opportunity.
     */
    static public Boolean hasOpp(Account a) {
            Integer opps = [
                  SELECT COUNT()
                  FROM Opportunity
                  WHERE AccountId = :a.id
                      AND RecordTypeId = :A2Z_RECORD_TYPE_ID
                      AND StageName = :A2Z_STAGE_NAME
                ];
            return (opps&gt;0);
    }
}</span></span></div><div style="font-family:inherit;"></div><div style="font-family:inherit;">
Replacing the extracted code with references to the static class, our test and trigger are now easier to follow.</div><div style="font-family:inherit;">
</div><div style="font-family:inherit;"><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">// Test and domain classes refactored to use new utility class.

@isTest
private class NU_TEST_a2zCreateOpportunity {

    /**
      * Generates an account with the NU_isA2zImport__c raised or lowered.
      * Assumes default is false.
      */
    static Account newAccount(Boolean doImport) {
            Account a = new Account(Name = 'Test Account',
            CompanyNumber__c = ‘1’;
            if (doImport) a.NU_isA2zImport__c = true; // False is default
            return a;
    }

    /**
      * Inserts the given account, and returns it again from a select.
      */
    static Account doInsert(Account a) {
            insert(a);
            return [
                SELECT id, NU_isA2zImport__c, CompanyNumber__c 
                FROM Account 
                WHERE id = :a.id
            ];
    }

    /**
     * Exercises "Insert Import".
     */
    static testMethod void testInsImp() {
            Account a = newAccount(true);
            Account a2 = doInsert(a);
            System.assert(NU_a2zOpps.hasOpp(a2), 'Expected opp on insert.');
    }
}

trigger NU_a2zCreateOpportunity on Account (after insert, after update) {
    // (1) On insert, if isImport, insert opp
    if (Trigger.isInsert) {
            List delta = new List();
            for (Account a : Trigger.new) {
                    if (a.NU_isA2zImport__c) {
                        delta.add(NU_a2zOpps.newOpp(a));
                    } 
            }
            if (delta.size()&gt;0) insert delta ;
    }
}</span></span></div><div style="font-family:inherit;"><span style="font-size:small;">
</span></div>We implemented the first requirement using a classic TDD pattern:
<div style="font-family:inherit;">

*   Create a failing test that proves a desired behavior is not present.
*   Write just enough code to pass the test.
*   Once the test succeeds, improve the design (refactor) so that it's easy to maintain.Let's continue to follow the TDD pattern with our second requirement: "if update and !doImport and hasOpp, delete opp".
First, the failing test:

<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">// Test deleting a related opportunity </span></span>

<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">static testMethod void testUpdImpNoImp() {</span>
<span style="font-family:'Courier New', Courier, monospace;">    Account a = newAccount(true); // Import</span>
<span style="font-family:'Courier New', Courier, monospace;">    Account a2 = doInsert(a); **// Line 2**</span>
<span style="font-family:'Courier New', Courier, monospace;">    a2.NU_isA2zImport__c = false; // No Import</span>
<span style="font-family:'Courier New', Courier, monospace;">    update a2; **// Line 4**</span>
<span style="font-family:'Courier New', Courier, monospace;">    System.assert(!NU_a2zOpps.hasOpp(a2),'Expected no opps on update.');</span>
<span style="font-family:'Courier New', Courier, monospace;">}</span></span>

When  we run the test, it fails, because our trigger inserts a new a2z  Opportunity (at line 2) but does not delete the Opportunity (at line 4).

OK,  let's update the trigger to provide the behavior expected by line 4\. As  before, we need to write the trigger to loop through a batch, while  minimizing database calls to stay within governor limits.
<span style="font-size:x-small;">
<span style="font-family:'Courier New', Courier, monospace;">// (2) On update, if not isImport and haveOpp, delete opp</span><span style="font-family:'Courier New', Courier, monospace;"> </span></span>
<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">if (Trigger.isUpdate) {</span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">           </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">Map opps = NU_a2zOpps.getOpps(Trigger.new);</span>
<span style="font-family:'Courier New', Courier, monospace;">           Set ids = opps.Keyset();</span>
<span style="font-family:'Courier New', Courier, monospace;">           List omega = new List();</span>
<span style="font-family:'Courier New', Courier, monospace;">           for (Account a : Trigger.new) {</span>
<span style="font-family:'Courier New', Courier, monospace;">                   if ( !a.NU_isA2zImport__c &amp;&amp; ids.contains(a.Id)) {</span>
<span style="font-family:'Courier New', Courier, monospace;">                       omega.add(opps.get(a.Id)); // (2)</span>
<span style="font-family:'Courier New', Courier, monospace;">                   }  </span>
<span style="font-family:'Courier New', Courier, monospace;">           }</span>
<span style="font-family:'Courier New', Courier, monospace;">           if (omega.size()&gt;0) delete omega;</span>
<span style="font-family:'Courier New', Courier, monospace;">}</span></span> 

The  crux of the code change is determining if we have a related opportunity  to delete. Easy enough with one Account, but in the case of a trigger,  we might have to check 200 accounts, and [our query limit is only 100](http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_gov_limits.htm).  Since the trigger passes us the set of Accounts in the batch, it’s not  difficult to retrieve the set of Opportunities related to those  Accounts. Though, now that we have a utility class, we should keep the  query details encapsulated behind another helper method. 

The <span style="font-family:'Courier New', Courier, monospace;">getOpps</span> helper method returns a Map itemizing the accounts  in our batch that have related a2z opportunities. 

<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">// The getOpps helper method </span>

<span style="font-family:'Courier New', Courier, monospace;">static public Map getOpps(List accounts) {    </span>
<span style="font-family:'Courier New', Courier, monospace;">        List oppsList = new List();</span>
<span style="font-family:'Courier New', Courier, monospace;">        Map opps = new Map();</span>
<span style="font-family:'Courier New', Courier, monospace;">        Integer count = [</span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">                </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">SELECT COUNT() </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">                </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">FROM Opportunity </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">                </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">WHERE RecordTypeId = :A2Z_RECORD_TYPE_ID </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">                    </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">AND StageName = :A2Z_STAGE_NAME </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">                    </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">AND AccountId IN :accounts</span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">        </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">];</span>
<span style="font-family:'Courier New', Courier, monospace;">        if (count&gt;0) {</span>
<span style="font-family:'Courier New', Courier, monospace;">                oppsList = [</span>
<span style="font-family:'Courier New', Courier, monospace;">                    SELECT AccountId FROM Opportunity</span>
<span style="font-family:'Courier New', Courier, monospace;">                    WHERE RecordTypeId = :A2Z_RECORD_TYPE_ID</span>
<span style="font-family:'Courier New', Courier, monospace;">                        AND StageName = :A2Z_STAGE_NAME</span>
<span style="font-family:'Courier New', Courier, monospace;">                        AND AccountId IN :accounts</span>
<span style="font-family:'Courier New', Courier, monospace;">                ];</span>
<span style="font-family:'Courier New', Courier, monospace;">                for (Opportunity o : oppsList) {</span>
<span style="font-family:'Courier New', Courier, monospace;">                    opps.put(o.AccountId,o);</span>
<span style="font-family:'Courier New', Courier, monospace;">                }</span>
<span style="font-family:'Courier New', Courier, monospace;">        }</span>
<span style="font-family:'Courier New', Courier, monospace;">        return opps;</span>
<span style="font-family:'Courier New', Courier, monospace;">}    </span></span>

A critical clause in the helper's SELECT statement is "<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">AccountId IN :accounts</span></span>".  This clause ensures that we only retrieve the Opportunities that are  related to Accounts in the current batch. Without this clause, we could  retrieve more Opportunities than allowed by the Force.com governor  (50,000). The helper also makes a point of returning an empty Map if  there are no matching opportunities, simplifying life for the caller.

While  we've been coding the trigger to act on a batch, our tests have not  been passing a batch of objects to the trigger. Let's add a test to be  sure batch mode is working.

<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">// Test to verify that insert works with batches of records</span>

<span style="font-family:'Courier New', Courier, monospace;">/**</span>
<span style="font-family:'Courier New', Courier, monospace;"> * Exercise Insert Import in batch mode.</span>
<span style="font-family:'Courier New', Courier, monospace;"> */</span>
<span style="font-family:'Courier New', Courier, monospace;">static testMethod void verifyBatchInsert() {</span>
<span style="font-family:'Courier New', Courier, monospace;">        List rows = new List();</span>
<span style="font-family:'Courier New', Courier, monospace;">        for (Integer r=0; r&lt;200; r++) {</span>
<span style="font-family:'Courier New', Courier, monospace;">                rows.add(newAccount(true, r));</span>
<span style="font-family:'Courier New', Courier, monospace;">        }</span>
<span style="font-family:'Courier New', Courier, monospace;">        insert(rows);</span>
<span style="font-family:'Courier New', Courier, monospace;">        List inserted = [</span>
<span style="font-family:'Courier New', Courier, monospace;">              SELECT id, NU_isA2zImport__c, CompanyNumber__c</span>
<span style="font-family:'Courier New', Courier, monospace;">              FROM Account</span>
<span style="font-family:'Courier New', Courier, monospace;">              WHERE id in :rows</span>
<span style="font-family:'Courier New', Courier, monospace;">        ];</span>
<span style="font-family:'Courier New', Courier, monospace;">        Set ids = NU_a2zOpps.getOpps(inserted).Keyset();</span>
<span style="font-family:'Courier New', Courier, monospace;">          Boolean  success = true;</span>
<span style="font-family:'Courier New', Courier, monospace;">          for (Account a : inserted) {        </span>
<span style="font-family:'Courier New', Courier, monospace;">                   success = success &amp;&amp; ids.contains(a.Id);            </span>
<span style="font-family:'Courier New', Courier, monospace;">          }</span>
<span style="font-family:'Courier New', Courier, monospace;">          System.assert(success,'Expected opps on batch insert.');</span>
<span style="font-family:'Courier New', Courier, monospace;">}</span></span>

Running  the test, initially, we hit a problem with a helper method. This  particular organization includes an external ID that must be unique for  each record. In batch mode, our external IDs are not unique, and so we  hit a validation error. A quick fix is to pass in the counter from the  loop, creating a serial number for each Account. 

<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">// newAccount with offset parameter </span></span>

<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">static Account newAccount(Boolean doImport, **Integer offset**) {</span>
<span style="font-family:'Courier New', Courier, monospace;">        Account a = new Account(</span>
<span style="font-family:'Courier New', Courier, monospace;">                Name = 'Test Account',</span>
<span style="font-family:'Courier New', Courier, monospace;">                CompanyNumber__c = String.valueOf(offset)</span>
<span style="font-family:'Courier New', Courier, monospace;">        );</span>
<span style="font-family:'Courier New', Courier, monospace;">        if (doImport) a.NU_isA2zImport__c = true; // False is default</span>
<span style="font-family:'Courier New', Courier, monospace;">        return a;</span>
<span style="font-family:'Courier New', Courier, monospace;">}</span>

<span style="font-family:'Courier New', Courier, monospace;">// for backward-compatibility</span>
<span style="font-family:'Courier New', Courier, monospace;">static Account newAccount(Boolean doImport) {</span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">        </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">return newAccount(doImport, 0);</span>
<span style="font-family:'Courier New', Courier, monospace;">}</span></span>

And, a batch delete test.

<div><span style="font-size:x-small;">// batchDelete </span></div><div>
</div><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">static testMethod void batchDelete() {</span>
<span style="font-family:'Courier New', Courier, monospace;">        List rows = new List();</span>
<span style="font-family:'Courier New', Courier, monospace;">            for (Integer r=0; r&lt;200; r++) {</span>
<span style="font-family:'Courier New', Courier, monospace;">                rows.add(newAccount(true, r));</span>
<span style="font-family:'Courier New', Courier, monospace;">        }</span>
<span style="font-family:'Courier New', Courier, monospace;">        insert rows;</span>
<span style="font-family:'Courier New', Courier, monospace;">        List inserted = [</span>
<span style="font-family:'Courier New', Courier, monospace;">               SELECT id, NU_isA2zImport__c, CompanyNumber__c</span>
<span style="font-family:'Courier New', Courier, monospace;">               FROM Account</span>
<span style="font-family:'Courier New', Courier, monospace;">               WHERE id in :rows</span>
<span style="font-family:'Courier New', Courier, monospace;">        ];</span>
<span style="font-family:'Courier New', Courier, monospace;">        for (Account a : inserted) {</span>
<span style="font-family:'Courier New', Courier, monospace;">                a.NU_isA2zImport__c = false;</span>
<span style="font-family:'Courier New', Courier, monospace;">        }</span>
<span style="font-family:'Courier New', Courier, monospace;">        update inserted;</span>
<span style="font-family:'Courier New', Courier, monospace;">        Set ids = NU_a2zOpps.getOpps(inserted).Keyset();        </span>
<span style="font-family:'Courier New', Courier, monospace;">        Boolean success = true;</span>
<span style="font-family:'Courier New', Courier, monospace;">        for (Account a : inserted) {</span>
<span style="font-family:'Courier New', Courier, monospace;">                success = success &amp;&amp; !ids.contains(a.Id);            </span>
<span style="font-family:'Courier New', Courier, monospace;">        }</span>
<span style="font-family:'Courier New', Courier, monospace;">        System.assert(success,'Expected no opps on batch delete.');        </span>
<span style="font-family:'Courier New', Courier, monospace;">}</span></span>

Both  of the new tests are passing, but they seem to repeat a lot of code,  and we have a third requirement coming up that will also need batch mode  testing. Let's see if we can create a helper class that can serve both  tests.

<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">// batchHelper method</span></span>
<span style="font-size:x-small;">
<span style="font-family:'Courier New', Courier, monospace;">static void batchHelper(Boolean insImp) {</span>
<span style="font-family:'Courier New', Courier, monospace;">       List rows = new List();</span>
<span style="font-family:'Courier New', Courier, monospace;">       for (Integer r=0; r&lt;200; r++) {</span>
<span style="font-family:'Courier New', Courier, monospace;">               rows.add(newAccount(insImp, r));</span>
<span style="font-family:'Courier New', Courier, monospace;">       }</span>
<span style="font-family:'Courier New', Courier, monospace;">       insert rows;</span>
<span style="font-family:'Courier New', Courier, monospace;">       List inserted = [</span>
<span style="font-family:'Courier New', Courier, monospace;">               SELECT id, NU_isA2zImport__c, CompanyNumber__c</span>
<span style="font-family:'Courier New', Courier, monospace;">               FROM Account</span>
<span style="font-family:'Courier New', Courier, monospace;">               WHERE id in :rows</span>
<span style="font-family:'Courier New', Courier, monospace;">       ];</span>
<span style="font-family:'Courier New', Courier, monospace;">       // For delete, we need to update the flag</span>
<span style="font-family:'Courier New', Courier, monospace;">       if (!insImp) {</span>
<span style="font-family:'Courier New', Courier, monospace;">               for (Account a : inserted) {</span>
<span style="font-family:'Courier New', Courier, monospace;">                       a.NU_isA2zImport__c = false;            </span>
<span style="font-family:'Courier New', Courier, monospace;">               }</span>
<span style="font-family:'Courier New', Courier, monospace;">               update inserted;</span>
<span style="font-family:'Courier New', Courier, monospace;">       }</span>
<span style="font-family:'Courier New', Courier, monospace;">       Set ids = NU_a2zOpps2.getOpps(inserted).Keyset();        </span>
<span style="font-family:'Courier New', Courier, monospace;">       Boolean success = true;</span>
<span style="font-family:'Courier New', Courier, monospace;">       if (insImp) {</span>
<span style="font-family:'Courier New', Courier, monospace;">               for (Account a : inserted) {</span>
<span style="font-family:'Courier New', Courier, monospace;">                       success = success &amp;&amp; ids.contains(a.Id);            </span>
<span style="font-family:'Courier New', Courier, monospace;">               }</span>
<span style="font-family:'Courier New', Courier, monospace;">               System.assert(success,'Expected opps on batch insert.');</span>
<span style="font-family:'Courier New', Courier, monospace;">       } else {</span>
<span style="font-family:'Courier New', Courier, monospace;">               for (Account a : inserted) {</span>
<span style="font-family:'Courier New', Courier, monospace;">                       success = success &amp;&amp; !ids.contains(a.Id);            </span>
<span style="font-family:'Courier New', Courier, monospace;">               }</span>
<span style="font-family:'Courier New', Courier, monospace;">               System.assert(success,'Expected no opps on batch delete.');        </span>
<span style="font-family:'Courier New', Courier, monospace;">       }</span>
<span style="font-family:'Courier New', Courier, monospace;">}</span>

<span style="font-family:'Courier New', Courier, monospace;">/**</span>
<span style="font-family:'Courier New', Courier, monospace;"> * Exercises Batch insert.</span>
<span style="font-family:'Courier New', Courier, monospace;"> */</span>
<span style="font-family:'Courier New', Courier, monospace;">static testMethod void batchHelperIns() {</span>
<span style="font-family:'Courier New', Courier, monospace;">    batchHelper(false);</span>
<span style="font-family:'Courier New', Courier, monospace;">}</span>

<span style="font-family:'Courier New', Courier, monospace;">/**</span>
<span style="font-family:'Courier New', Courier, monospace;"> * Exercises Batch delete.</span>
<span style="font-family:'Courier New', Courier, monospace;"> */</span>
<span style="font-family:'Courier New', Courier, monospace;">static testMethod void batchHelperDel() {</span>
<span style="font-family:'Courier New', Courier, monospace;">    batchHelper(true);</span>
<span style="font-family:'Courier New', Courier, monospace;">}    </span></span>

By passing a flag, we are able to use one utility for both cases, and share 90% of the code.

Repeating  the patterns we've seen, we can test and refactor our way into a  robust, reliable trigger to manage our integration object.

For the complete production source  code (without the play-by-play), see [Managing a Related Object via a  Checkbox in Salesforce CRM](http://tedhusted.blogspot.com/2012/03/managing-related-object-via-checkbox-in.html). 
<div style="font-family:inherit;">
</div>Key takeaways are: 

*   Use a utility class to share code between test and domain classes.
*   Use helper methods to share code between similar test methods.
*   Use the SELECT-IN-SET pattern to keep triggers within governor limits.Test Driven Development is a rigorous, structured approach that  helps us create robust and reliable code. Since TDD uses successive  refinement, we can easily extend and improve the code over time.</div><div style="font-family:inherit;">
</div>