---
title: Managing a Related Object via a Checkbox in Salesforce CRM
id: 47
categories:
  - Tutorial
date: 2012-03-06 06:00:00
tags:
 - Salesforce
---

<div class="separator" style="clear:both;text-align:center;">[![](http://suvendugiri.files.wordpress.com/2012/02/checkbox.png)](http://suvendugiri.wordpress.com/2012/02/09/android-using-checkbox-with-example/)</div>This Force.com recipe shows how to use a checkbox to manage a related object via an Apex trigger. 

**Problem: **An off-the-shelf integration requires the existence  of a specific Salesforce CRM Opportunity object. When the object is  present, the integration acts on the Account related to the Opportunity.  When the object is not present. the integration bypasses the Account.

**User Story:** As a CRM User, I need to easily manage the Opportunity that signals the  integration, for example, by selecting a checkbox that creates or  removes the related integration object.

**Solution:** Implement an Account trigger that observes the checkbox and inserts or deletes the related integration object.
<div>**<span style="font-size:x-small;"></span>**
<a name='more'></a>**<span style="font-size:x-small;">// trigger class</span>**</div><div><span style="font-size:x-small;">
/** 
 * Manages a2z Opportunity by referring to the 
 * "Is a2z Import" checkbox on the Account object. 
 * When an a2z Opportunity exists, the Account is imported to a2z. 
 * The trigger handles three transitional states: 
 * (1) if insert and doImport, insert opp; 
 * (2) if update and !doImport and hasOpp, delete opp;    
 * (3) if update and doImport and !hasOpp, insert opp.
 * By inference, the trigger also handles:
 * (4) if insert and !doImport, exit; 
 * (5) if update and doImport and hasOpp, exit;
 * (6) if !doImport and !hasOpp, exit.
 */
trigger NU_a2zCreateOpportunity on Account (after insert, after update) {

    // (1) On insert, if isImport, insert opp
    if (Trigger.isInsert) {
        List delta = new List();
        for (Account a : Trigger.new) {
            if (a.NU_isA2zImport__c) {
                delta.add(NU_a2zOpps.newOpp(a));
            }        
        }
        if (delta.size()&gt;0) insert(delta);
    }

    // (2) On update, if not isImport and haveOpp, delete opp
    // (3) On update, if isImport and not haveOpp, insert opp
    if (Trigger.isUpdate) {

        if (Trigger.new.size() == 1) {
            Account a = Trigger.new.get(0);
            Boolean hasOpp = NU_a2zOpps.hasOpp(a);
            if (!a.NU_isA2zImport__c &amp;&amp; hasOpp) {
                delete(NU_a2zOpps.getOpp(a)); // (2)
            }  
            if (a.NU_isA2zImport__c &amp;&amp; !hasOpp) {
                insert(NU_a2zOpps.newOpp(a)); // (3)
            }    

        } else {
            Map opps = NU_a2zOpps.getOpps(Trigger.new);
            Set ids = opps.Keyset();
            Set dels = new Set();
            List deletes = new List();
            List inserts = new List();
            for (Account a : Trigger.new) {
                if (!a.NU_isA2zImport__c &amp;&amp; ids.contains(a.Id)) {
                    deletes.add(opps.get(a.Id)); // (2)
                }  
                if (a.NU_isA2zImport__c &amp;&amp; !ids.contains(a.Id)) {
                    inserts.add(NU_a2zOpps.newOpp(a)); // (3)
                }    
            } 
            if (deletes.size()&gt;0) delete(deletes);
            if (inserts.size()&gt;0) insert(inserts);
        }
    }
}</span></div><div>**
**
**
**</div><span style="font-size:x-small;">**<span style="font-family:'Courier New', Courier, monospace;">// test class</span>**
<span style="font-family:'Courier New', Courier, monospace;"></span></span>

<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">/**
 * Exercises the a2zCreateOpportunity feature set. 
 */
@isTest
private class NU_TEST_a2zCreateOpportunity {

    /** 
     * Generates a key based on system time and offset.
     * Sufficient for unit test purposes only.
     */
    static String newCompanyNumber(Integer offset, String kicker) {
        Datetime n = Datetime.now();
        return kicker + String.valueOf(n.minute()) + String.valueOf(n.millisecond()) + String.valueOf(offset);
    } 

    /**
     * Generates an account with the NU_isA2zImport__c raised or lowered.
     * Assumes default is false. 
     */
    static Account newAccount(Boolean doImport, Integer offset, String kicker) {
        Account a = new Account(Name = 'Test Account', 
            CompanyNumber__c = newCompanyNumber(offset, kicker));
        if (doImport) a.NU_isA2zImport__c = true; // False is default
        return a;
    }

    /**
     * Generates an account with the NU_isA2zImport__c raised or lowered.
     * Assumes default is false. 
     */
    static Account newAccount(Boolean doImport) {
        return newAccount(doImport, 0, '');
    }

    /** 
     * Inserts the given account, and returns it again from a select. 
     */
    static Account doInsert(Account a) {
        insert(a);
        return [</span></span>
<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">            </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">SELECT id, NU_isA2zImport__c, CompanyNumber__c
 </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">            </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">FROM Account
 </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">            </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">WHERE id = :a.id</span></span>
<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">        </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">];
    }

    /** 
     * Exercises "Insert Import".
     */
    static testMethod void testInsImp(){
        Account a = newAccount(true);
        Account a2 = doInsert(a);
        System.Assert(NU_a2zOpps.hasOpp(a2),'Expected opp on insert.');
    }

    /** 
     * Exercises "Insert NoImport". 
     */
    static testMethod void testInsNoImp(){
        Account a = newAccount(false);
        Account a2 = doInsert(a);
        System.Assert(!NU_a2zOpps.hasOpp(a2),'Expected no opps on insert.');
    }

    /**
     * Creates, inserts, and updates an account, 
     * changing the import settings in between.
     */
    static void updateHelper(Boolean insImp, Boolean updImp) {
        Account a = newAccount(insImp);
        Account a2 = doInsert(a);
        a2.NU_isA2zImport__c = updImp;
        update(a2);
        if (updImp) {
            System.Assert(NU_a2zOpps.hasOpp(a2),'Expected opps on update.');
        } else {
            System.Assert(!NU_a2zOpps.hasOpp(a2),'Expected no opps on update.');
        }
    }

    /** 
     * Exercises "Update ImportNoImport". 
     */
    static testMethod void testUpdImpNoImp(){
        updateHelper(true,false);
    }

    /** 
     * Exercises "Update NoImportImport".
     */
    static testMethod void testUpdNoImpImp(){
        updateHelper(false,true);
    }

    /** 
     * Exercises "Update ImportImport".
     */
    static testMethod void testUpdImpImp(){
        updateHelper(true,true);
    }

    /** 
     * Exercises "Update NoImportNoImport".
     */
    static testMethod void testUpdNoImpNoImp(){
        updateHelper(false,false);
    }

    static List getInserted(List rows) {
        return [</span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">            </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">SELECT id, NU_isA2zImport__c, CompanyNumber__c
 </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">            </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">FROM Account </span></span>
<span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">            </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">WHERE id in :rows</span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">
        </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">]; 
    }

    /** 
     * Creates, inserts, and updates an account,
 </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">     * </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">changing the import settings in between, for a batch of accounts.
     */
    static void batchHelper(Boolean insImp, Boolean updImp,
 </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">        </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">        </span></span><span style="font-size:x-small;"><span style="font-family:'Courier New', Courier, monospace;">Boolean contains, String kicker) {
        List rows = new List();
        for (Integer r=0; r&lt;200; r++) {
            rows.add(newAccount(insImp, r, kicker));
        }
        insert(rows);
        List inserted = getInserted(rows);
        // In Apex, anything can be null
        if (updImp != null) {
            for (Account a : inserted) {
                a.NU_isA2zImport__c = updImp;            
            }
        }
        update(inserted);
        Set ids = NU_a2zOpps.getOpps(inserted).Keyset();        
        Boolean success = true;
        if (contains) {
            for (Account a : inserted) {
                success = success &amp;&amp; ids.contains(a.Id);            
            }
            System.Assert(success,'Expected opps on batch delete.');
        } else { 
            for (Account a : inserted) {
                success = success &amp;&amp; !ids.contains(a.Id);            
            }
            System.Assert(success,'Expected no opps on batch delete.');        
        }
     }

    /** 
     * Exercises "Batch has flag set".
     */
    static testMethod void batchHelperHas() {
        batchHelper(true,null,true,'A');
    }

    /** 
     * Exercises "Batch does not have flag set".
     */
    static testMethod void batchHelperHasNot() {
        batchHelper(false,null,false,'B');
    }

    /** 
     * Exercises Batch delete.
     */
    static testMethod void batchHelperDel() {
        batchHelper(true,false,false,'C');
    }

    /** 
     * Exercises Batch insert.
     */
    static testMethod void batchHelperIns() {
        batchHelper(false,true,true,'D');
    }
}</span></span>