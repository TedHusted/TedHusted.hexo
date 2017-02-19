---
title: Toggling PDF Mode In a Visualforce Apex Page
id: 44
categories:
  - Uncategorized
date: 2012-03-15 05:00:00
tags:
---

<div class="separator" style="clear:both;text-align:center;">[![](http://screenshots.en.sftcdn.net/blog/en/2009/02/pdfbotoncico.png)](http://onsoftware.en.softonic.com/top-free-tools-to-open-create-and-edit-pdf-documents)</div>Salesforce.com has made it easy to blend custom Visualforce pages into a standard Salesforce CRM site. Using the standard stylesheets and a smattering of markup, we can cobble up a custom Account, Contact, or Opportunity page, with an absolute minimum of effort.  

It's also easy to construct a wholly custom page resemble a native Salesforce CRM page. While the page looks great, it may not be printer-friendly, and users may have trouble sharing the output with their non-Salesforce brethren. One solution is to render the pages as a very-printable PDF. Salesforce.com has also made the PDF option blazingly easy. Just include a "renderas=PDF" attribute at the top of your page and -- voilà -- instant PDF.   

<a name='more'></a>Of course,  we don't want to maintain two versions of the page, or give either our pretty or printable versions, and so the next thing is be able to switch between the HTML and PDF versions.  

Happily, there is a [venerable blog posting](http://blogs.developerforce.com/developer-relations/2008/06/dynamically-cho.html) that provides a great leg up, but I ran into an issue where I wanted to offer another option on top of PDF/HTML.  

Aside from PDF printing, we also wanted to support two flavors of the same database query. One version aggregated rows with a status column set to either "Value A" or "Value B", and a second version aggregated rows set to "Value C". Our case was different, but a common case might a report that showed "Cold or Warm" leads, and another that showed only "Hot" leads.  

Bottom line is that we wanted to present the page as either HTML or PDF, using either Query A or Query B, and keep all the markup and source code together.  

Here's my solution:  

<div class="separator" style="clear:both;text-align:center;"></div><div class="separator" style="clear:both;text-align:center;">[![](https://tedhusted.files.wordpress.com/2012/03/9173f-pdf-foo.png?w=300)](https://tedhusted.files.wordpress.com/2012/03/9173f-pdf-foo.png)</div>
<span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;white-space:pre;">// and the backend </span>
<pre><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;"><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">
/**
 * Computes and transforms data for use by the Campaign Profile page.
 */
public class Foo_Ctrl {

    // PDF parameter.
    private final static String PDF = 'p';

    // HTML parameter.
    private final static String MODE = 'm';

    // Stores Account to use with the Campaign Profile.
    private Account myAccount = null;

    // Stores set of query constraints based on Mode parameter. 
    // "AND My_Mode__c IN :myMode"
    private Set myMode = null;    

    // Assembles and returns URI reflecting current parameter settings.
    private String doEnable(Boolean doPDF, Boolean doRollup) {
        String parameters = '/apex/Foo?';
        if (myAccount!=null) parameters += 'id=' + MyID;
        if (doPDF) parameters += '&amp;' + PDF + '=true';
        if (doRollup) parameters += '&amp;' + MODE + '=true';
        return parameters;
    }

    /**
     * Stores current or desired state "Status".
     */
    public boolean IsModeB { get; set; }

    /**
     * Reflects absence of "Rollup" parameter.
     */
    public boolean IsModeA {
        get {return !IsModeB;}
        set {IsModeB = !value;}
    }   

    /**
     * Handles runtime "PDF" parameter 'p'.
     */
    public boolean IsPDF { get; set; }

    /**
     * Reflects absence of "PDF" parameter.
     */
    public boolean IsHTML {
        get {return !IsPDF;}
        set {IsPDF = !value;}
    }   

    /**
     * Returns current account ID, or null.
     */
    public Id MyId {
        get {return (myAccount==null) ? null : myAccount.Id;}
    }

    /**
     * Returns updated URI including PDF parameter,
     * and the current value of MODE parameter.
     */
    public String doEnablePDF {
        get{return doEnable(true,IsModeB);}
    }

    /**
     * Returns updated URI excluding PDF parameter,
     * and the including current value of MODE parameter.
     */
    public String doEnableHTML {
        get{return doEnable(false,IsModeB);}
    }

    /**
     * Returns updated URI including MODE parameter,
     * and the current value of PDF parameter.
     */
    public String doEnableModeB {
        get{return doEnable(IsPDF,true);}
    }

    /**
     * Returns updated URI excluding MODE parameter,
     * and including the current value of PDF parameter.
     */
    public String doEnableModeA {
        get{return doEnable(IsPDF,false);}
    }

    /**
     * Constructs the controller object, capturing the current Account and year. 
     */
    public Foo_Ctrl(ApexPages.StandardController controller) {
        if (controller.getId() == null) {
            ApexPages.addMessage(new ApexPages.Message(ApexPages.Severity.Error, 'Id is required'));
        } else {
            if (myAccount == null) myAccount = (Account) controller.getRecord();
        }
        // Instatiate the rest, to avoid runtime errors ...
        IsPDF = (ApexPages.currentPage().getParameters().get(PDF)=='true');
        IsModeB = (ApexPages.currentPage().getParameters().get(MODE)=='true');
        myMode = new Set();
        if (IsModeB) {
            myMode.add('Status B');
            myMode.add('Status C');
        } else {
            myMode.add('Status A');
        }
    }
}

// And, of course, a test suite

@IsTest
public class NU_TEST_Foo_Page {

    static Account myAccount = null;
    static Foo_Ctrl myCtrl = null;

   static private void mySetUp() {
        Test.setCurrentPageReference(Page.Foo);
        // Constructor Scaffolding
        if (myAccount==null) {
            myAccount = new Account(name='Alpha Bravo',
            NU_Andar_Account_Number__c='1');
            insert myAccount;
            myCtrl = new Foo_Ctrl(new ApexPages.StandardController(myAccount));
        }
    }

    static testMethod void PDF_Test() {
        mySetUp();
        String nextPage = myCtrl.doEnablePDF;
        System.assertEquals('/apex/Foo?id=' + myAccount.Id + '&amp;' + Foo_Ctrl.PDF + '=true',nextPage);
    }

    static testMethod void HTML_Test() {
        mySetUp();
        String nextPage = myCtrl.doEnableHTML;
        System.assertEquals('/apex/Foo?id=' + myAccount.Id + '',nextPage);         
    }

    static testMethod void ModeB_Test() {
        mySetup();
        String nextPage = myCtrl.doEnableModeB;
        System.assertEquals('/apex/Foo?id=' + myAccount.Id + '&amp;' + Foo_Ctrl.MODE + '=true',nextPage);
    }

    static testMethod void ModeA_Test() {
        mySetUp();
        String nextPage = myCtrl.doEnableModeA;
        System.assertEquals('/apex/Foo?id=' + myAccount.Id + '',nextPage);         
    }

    static testMethod void PDF_ModeB_Test() {
        mySetUp();
        ApexPages.currentPage().getParameters().put(Foo_Ctrl.PDF, 'true');
        Foo_Ctrl ctrlPDF = new Foo_Ctrl(new ApexPages.StandardController(myAccount));
        String nextPage = ctrlPDF.doEnableModeB;
        System.assertEquals('/apex/Foo?id=' + myAccount.Id + '&amp;' + Foo_Ctrl.PDF + '=true' + '&amp;' + Foo_Ctrl.MODE + '=true',nextPage);
        System.assertEquals(true,ctrlPDF.isPDF);
        System.assertEquals(false,ctrlPDF.isHTML);
        ctrlPDF.isHTML = true;
        System.assertEquals(false,ctrlPDF.isPDF);
    }

    static testMethod void ModeB_PDF_Test() {
        mySetUp();
        ApexPages.currentPage().getParameters().put(Foo_Ctrl.MODE, 'true');
        Foo_Ctrl ctrlMode = new Foo_Ctrl(new ApexPages.StandardController(myAccount));
        String nextPage = ctrlMode.doEnablePDF;
        System.assertEquals(true,ctrlMode.isModeB);
        System.assertEquals(false,ctrlMode.isModeA);       
        System.assertEquals('/apex/Foo?id=' + myAccount.Id + '&amp;' + Foo_Ctrl.PDF + '=true' + '&amp;' + Foo_Ctrl.MODE + '=true',nextPage);
    }

     static testMethod void PDF_Flag_Test() {
        mySetUp();
        ApexPages.currentPage().getParameters().put(Foo_Ctrl.PDF, 'true');
        Foo_Ctrl ctrlPDF = new Foo_Ctrl(new ApexPages.StandardController(myAccount));
        System.assertEquals(true,ctrlPDF.isPDF);
        System.assertEquals(false,ctrlPDF.isHTML);
        ctrlPDF.isHTML = true;
        System.assertEquals(false,ctrlPDF.isPDF);
    }

     static testMethod void Mode_Flag_Test() {
        mySetUp();
        ApexPages.currentPage().getParameters().put(Foo_Ctrl.MODE, 'true');
        Foo_Ctrl ctrlMode = new Foo_Ctrl(new ApexPages.StandardController(myAccount));
        System.assertEquals(true,ctrlMode.isModeB);
        System.assertEquals(false,ctrlMode.isModeA);
        ctrlMode.isModeA = true;
        System.assertEquals(false,ctrlMode.isModeB);
    }

}</span></span></pre><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">
</span>
<span class="Apple-style-span" style="font-family:inherit;">For more Visualforce PDF trickery, [see this nifty blog](http://force.siddheshkabe.co.in/2011/04/some-pdf-tricks-on-visualforce.html) on May The Force Be With You.  </span>