---
title: Failing safe with the Apex Data Loader for Salesforce
id: 34
categories:
  - Salesforce
date: 2012-04-07 09:00:00
tags:
---

<div class="separator" style="clear:both;text-align:center;">[
![](http://blog.domaintools.com/wp-content/uploads/2011/08/Alerts1.jpg)](http://blog.domaintools.com/2011/08/how-to-use-alerts-to-get-the-scoop-on-your-competitors/)</div>
<div style="font-family:Arial;">If you have a way to pull and format data from another system, the **Apex Data Loader** is a great, on-demand way to push data into **Salesforce CRM**. Without much effort, you can automate the process and create a hands-free data sync. The tricky part is that while the Data Loader creates excellent logs, there's no obvious way to alert a person when a data sync fails. Once a system is in place, failures are rare, but, in real life, stuff happens, and it's helpful if failsafes are in place.</div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">A key problem is that the Apex Data Loader writes an error log whether there are errors or not. If the pass is 100% successful, the error log will have a header line and no rows.</div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">If the error log was only written when errors occurred, we could just look for the error logs, and process the files whenever they crop up. But, in this case, we first need to look inside the log and count the lines before we can be sure there was a failure.</div>
<div style="font-family:Arial;"></div>
<div><a name="more"></a><span class="Apple-style-span" style="font-family:Arial;">On a Windows-based system, one solution is to use a VB script, like </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">COUNTLN.VBS</span><span class="Apple-style-span" style="font-family:Arial;">, to count the number of lines in a file. </span></div>
<div style="font-family:Arial;"></div>
<div>

<span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">' COUNTLN.VBS</span>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">' Description: Count the number of lines in a file and return as error level (up to 255)</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">' Usage: cscript countln.vbs errors.csv</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">' Concept: [http://blogs.technet.com/b/heyscriptingguy/archive/2004/10/12/how-can-i-count-the-number-of-lines-in-a-text-file.aspx](http://blogs.technet.com/b/heyscriptingguy/archive/2004/10/12/how-can-i-count-the-number-of-lines-in-a-text-file.aspx)</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">Const ForReading = 1</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">Set objFSO = CreateObject("Scripting.FileSystemObject")</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">Set objTextFile = objFSO.OpenTextFile (WScript.Arguments.Item(0), ForReading)</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">objTextFile.ReadAll</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">' Wscript.Echo "Number of lines: " &amp; objTextFile.Line</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">WScript.Quit(objTextFile.Line)</span></div>
</div>
<div style="font-family:Arial;"></div>
<div><span class="Apple-style-span" style="font-family:Arial;">The </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">COUNTLN.VBS</span><span class="Apple-style-span" style="font-family:Arial;"> script sets the exit condition, which can then be tested in a DOS Batch file, like </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">CHECKLOG.BAT</span><span class="Apple-style-span" style="font-family:Arial;">.</span></div>
<div style="font-family:Arial;"></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">@ECHO OFF</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">REM CHECKLOG.BAT</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">REM Count the lines and send the errors, if any</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">ECHO CountLn: %1</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">CSCRIPT ..\countln.vbs %1</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">IF ERRORLEVEL 1 ECHO Line Count: %errorlevel%</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">IF ERRORLEVEL 3 ..\7z.exe a %2 %1</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">ERASE %1</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">ECHO Checklogs process complete.</span></div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">The Apex Data Loader takes as input a common-separated-value (CSV) or a tab-delimited file. The first row of the input file is a header detailing the input columns, with each subsequent row representing a record to be sent to the target Salesforce CRM object.</div>
<div style="font-family:Arial;"></div>
<div><span class="Apple-style-span" style="font-family:Arial;">When the Apex Data Loader runs, it creates an </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">success(ID).log</span><span class="Apple-style-span" style="font-family:Arial;"> and an </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">error(ID).log</span><span class="Apple-style-span" style="font-family:Arial;">. The (ID) is replaced with a timestamp shared by the companion success and error logs. Both logs contain a header row, and a row for each input row, detailing the data processed for each column. The success log contains all the rows that made it into Salesforce. The error logs contains all the rows that did not cross over. </span></div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">Conveniently, the logs can also be used as input files. If the errors can be easily fixed, you can resubmit the error log to the Apex Data Loader, without going back to the external system.</div>
<div style="font-family:Arial;"></div>
<div><span class="Apple-style-span" style="font-family:Arial;">The </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">CHECKLOG.BAT</span><span class="Apple-style-span" style="font-family:Arial;"> file is designed to add the matching error files to a compressed archive file in the widely-supported ZIP format. Here we're using the </span>[open source 7z utility](http://www.7-zip.org/)<span class="Apple-style-span" style="font-family:Arial;"> to create the archive, but others would work as well. In this way, if </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">CHECKLOG</span><span class="Apple-style-span" style="font-family:Arial;"> is is not run as often as the Apex Data Loader itself, the process can support a single file or multiple files. And, if the data is sensitive, most ZIP utilities support encrypting the archive. </span></div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">Given a secure ZIP archive, email can be a great way to get the error logs to an administrator who can address the problems. Staying with our Windows platform theme, here's another VB script that can send an email alert, with an attachment, to a Gmail account.</div>
<div style="font-family:Arial;"></div>
<div>

<span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">'Usage: cscript sendmail.vbs &lt;[email_recipient@example.com](mailto:email_recipient@example.com)&gt; "" "" ""</span>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">'Ex. No attach: cscript sendmail.vbs [example@gmail.com](mailto:example@gmail.com) "test subject line" "test email body"</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">'Ex. W/ attach: cscript sendmail.vbs [example@gmail.com](mailto:example@gmail.com) "test subject line" "test email body" "c:\scripts\log.txt"</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">'Attachment path cannot be relative</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">'Credit: [http://cybernetnews.com/vbscript-send-emails-using-gmail/](http://cybernetnews.com/vbscript-send-emails-using-gmail/)</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">'*** CONFIGURE THE FROM EMAIL ADDRESS AND PASSWORD ***</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">Const fromEmail = "(TBD)@gmail.com"</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">Const password = "(TBD)"</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">'*** END OF CONFIGURATION ***</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">Dim emailObj, emailConfig</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">Set emailObj = CreateObject("CDO.Message")</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">emailObj.From = fromEmail</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">[emailObj.To](http://emailObj.To/) = WScript.Arguments.Item(0)</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">emailObj.Subject = WScript.Arguments.Item(1)</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">emailObj.TextBody = WScript.Arguments.Item(2)</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">If WScript.Arguments.Count &gt; 3 Then</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">emailObj.AddAttachment WScript.Arguments.Item(3)</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">End If</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">Set emailConfig = emailObj.Configuration</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">emailConfig.Fields("[http://schemas.microsoft.com/cdo/configuration/smtpserver](http://schemas.microsoft.com/cdo/configuration/smtpserver)") = "[smtp.gmail.com](http://smtp.gmail.com/)"</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">emailConfig.Fields("[http://schemas.microsoft.com/cdo/configuration/smtpserverport](http://schemas.microsoft.com/cdo/configuration/smtpserverport)") = 465</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">emailConfig.Fields("[http://schemas.microsoft.com/cdo/configuration/sendusing](http://schemas.microsoft.com/cdo/configuration/sendusing)") = 2</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">emailConfig.Fields("[http://schemas.microsoft.com/cdo/configuration/smtpauthenticate](http://schemas.microsoft.com/cdo/configuration/smtpauthenticate)") = 1</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">emailConfig.Fields("[http://schemas.microsoft.com/cdo/configuration/smtpusessl](http://schemas.microsoft.com/cdo/configuration/smtpusessl)") = true</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">emailConfig.Fields("[http://schemas.microsoft.com/cdo/configuration/sendusername](http://schemas.microsoft.com/cdo/configuration/sendusername)") = fromEmail</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">emailConfig.Fields("[http://schemas.microsoft.com/cdo/configuration/sendpassword](http://schemas.microsoft.com/cdo/configuration/sendpassword)") = password</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">emailConfig.Fields.Update</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">emailObj.Send</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">Set emailobj = nothing</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">Set emailConfig = nothing</span></div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;"></div>
</div>
<div style="font-family:Arial;">Of course, the notion is that you create a special Gmail account just to receive the alerts and then forward to an administrator from there.</div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">While we have the pieces in place for handling the error logs, there is also the problem of what to do with the success logs. The Apex Data Loader kindly generates both logs, that between them, contain all of the detail found in the original input file, for each and every pass.</div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">While this level of detail is great (really-really-great), if you are running a sync every five minutes, retrieving a log later can be a real "cant-see-the-forest--for-the-trees" headache.</div>
<div style="font-family:Arial;"></div>
<div><span class="Apple-style-span" style="font-family:Arial;">Long story short, in addition to error alerts, an automated Apex Data Loader sync also needs help with general log management. The </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">ARCHIVE.BAT</span><span class="Apple-style-span" style="font-family:Arial;"> file is designed to be run at least once a day, but could also be run every hour. It sweeps all of the success logs into an archive for the current date, and, as discussed, emails the error logs to a Gmail account. </span></div>
<div style="font-family:Arial;"></div>
<div><span class="Apple-style-span" style="font-family:Arial;">To deploy the system, beneath the current logging folder (usually "Status"), create an Archive folder, and beneath that create Success and Error folders. Put the </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">ARCHIVE.BAT</span><span class="Apple-style-span" style="font-family:Arial;"> and VBS files under the Archive folder (along with the 7z utility), and the </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">CHECKLOG.BAT</span><span class="Apple-style-span" style="font-family:Arial;"> under the </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">Error</span><span class="Apple-style-span" style="font-family:Arial;"> folder. </span></div>
<div style="font-family:Arial;"></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">\Status</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">.\Archive</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">- archive.bat</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">- countln.vbs</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">- sendmail.vbs</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">- 7z.exe</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">- 7z.dll</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">- 7z-zip.dll</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">.\Archive\Error</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">- checklog.bat</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">.\Archive\Success</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">- checklog.bat</span></div>
<div style="font-family:Arial;"></div>
<div><span class="Apple-style-span" style="font-family:Arial;">A call to </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">ARCHIVE.BAT</span><span class="Apple-style-span" style="font-family:Arial;"> can be queued as a Windows Scheduled Task to run every hour, or once a day, depending on how often the Apex Data Loader runs. The system moves the logs out of .\Status and creates a ZIP for each day under </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">.\Status\Archive</span><span class="Apple-style-span" style="font-family:Arial;"> in the format </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">"LOGS-YYMMDD.ZIP"</span><span class="Apple-style-span" style="font-family:Arial;">  (while emailing the error logs to the Gmail account). Temporary copies of the Success and Error logs are also put into the Success and Error folders, for processing by </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">CHECKLOG.BAT</span><span class="Apple-style-span" style="font-family:Arial;">.</span></div>
<div style="font-family:Arial;"></div>
<div>

<span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">@ECHO OFF</span>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM ARCHIVE.BAT</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM Email non-empty Data Loader error logs to a GMail account and</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM archive to a zip file all other log files (*.csv).</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM --</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM Main file to call from a scheduled task.</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM Intended to be launched from a subdirectory directly beneath the log folder.</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM Expects an error\ subdirectory to exist (status\archive\error).</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM --</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM Temporarily stores error logs being emailed in errorlog.zip and</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM calls archivelogs.bat to store logs by date in LOGS-20??-??-??.ZIP</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM Uses as dependencies: checklog.bat, countln.vbs, sendmail.vbs, 7z*.*</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM Checklog.bat is stored in subdirectories .\error and .\success. </span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM All others in status\archive next to this file.</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM --</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM Bring logs down to a working folder, to avoid gaps in process</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM --</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">IF EXIST ..\*.csv MOVE ..\*.csv .</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">IF NOT EXIST *.csv GOTO Done</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM --</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM Buffer errors and archive latest logs (\error must exist)</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM --</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">IF EXIST error*.csv COPY error*.csv error\</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">IF EXIST success*.csv COPY success*.csv success\</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">SET TDate=%date:~10,4%%date:~4,2%%date:~7,2%</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">.\7z.exe a LOGS-%TDate%.zip *.csv</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">IF ERRORLEVEL 1 GOTO Done</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">IF NOT EXIST LOGS-%TDate%.zip GOTO Done</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">ERASE *.csv</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM --</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM Cleanup errorlog.zip before calling errorlogs.bat</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">SET ZIP=(TBD: Full path to a local working directory)\errorlog.zip</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">IF EXIST %ZIP% ERASE %ZIP%</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM --</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM Call checklog for each error log.</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM --</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">CD error</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">FOR %%x IN ( error*.csv ) DO CMD.EXE /C checklog.bat %%x %%ZIP%%</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">ECHO Emailing: %ZIP%</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM Attachment path cannot be relative</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">CSCRIPT ..\sendmail.vbs [ccsa.alert@gmail.com](mailto:ccsa.alert@gmail.com) "Alert! Data Load Failed." "The Data Loader was unable to push records to Salesforce." "%ZIP%"</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">CD ..</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM --</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM Cleanup successlog.zip before calling checklogs.bat</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">SET ZIP=(TBD: Full path to a local working directory)\successlog.zip</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">IF EXIST %ZIP% ERASE %ZIP%</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM --</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM Call checklog for each success log.</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM --</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">CD success</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">FOR %%x IN (success*.csv ) DO CMD.EXE /C checklog.bat %%x %%ZIP%%</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">ECHO Emailing: %ZIP%</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">REM Attachment path (%ZIP%) cannot be relative</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">CSCRIPT ..\sendmail.vbs (TBD@TBD.COM) "Alert! Data Load Succeeded." "The Data Loader was able to push records to Salesforce." "%ZIP%"</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">CD ..</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">:Done</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:xx-small;">ECHO Archive process complete.</span></div>
<div style="font-family:Arial;"></div>
</div>
<div><span class="Apple-style-span" style="font-family:Arial;">The second </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">CHECKLOG</span><span class="Apple-style-span" style="font-family:Arial;"> call to the success folder is optional. When left in place, the system will email the success and error logs separately, in case the administrator wants assurance that the system is running successfully on schedule. </span></div>
<div style="font-family:Arial;"></div>
<div><span class="Apple-style-span" style="font-family:Arial;">Another option would be to check for certain types of fatal errors. One fatal case would be someone renaming a target field in Salesforce without updating the Apex Data Loader configuration. If the mapping file refers to a field that no longer exists, the Data Loader generates a zero-length success log (with not even a header row). The </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">CHECKFATAL.BAT</span><span class="Apple-style-span" style="font-family:Arial;"> alternative only ZIPs for emailing any success logs that are totally empty. </span></div>
<div style="font-family:Arial;"></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">@ECHO OFF</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">REM CHECKFATAL.BAT</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">REM Count the lines and send only fatal errors, if any. Only use in Success folder.</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">ECHO CountLn: %1</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">CSCRIPT ..\countln.vbs %1</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">IF ERRORLEVEL 2 ERASE %1</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">IF EXIST %1 ..\7z.exe a %2 %1</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;">ECHO Check fatal process complete.</span></div>
<div></div>
<div><span class="Apple-style-span" style="font-family:Arial;">The final touch is to prune the logs every so often. The </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">PRUNELOGS.BAT</span><span class="Apple-style-span" style="font-family:Arial;"> batch file can be run once a month, on the first of the month. Based on the date represented by the file name, </span><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">PRUNELOGS.BAT</span><span class="Apple-style-span" style="font-family:Arial;"> retains 2-3 months of logs, based on the current month, and deletes the rest. </span></div>
<div style="font-family:Arial;"></div>
<div>

<span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">@ECHO OFF</span>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">REM PRUNELOGS.BAT</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">REM ZIP delete the archive for month before last (retain 2-3 months of logs)</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">REM Intended to be called on the 1st day of each month</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">SET MM %date:~-10,2</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">IF “%MM%“ == “01” ERASE LOGS-20??-11-??.ZIP</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">IF “%MM%“ == “02” ERASE LOGS-20??-12-??.ZIP</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">IF “%MM%“ == “03” ERASE LOGS-20??-01-??.ZIP</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">IF “%MM%“ == “04” ERASE LOGS-20??-02-??.ZIP</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">IF “%MM%“ == “05” ERASE LOGS-20??-03-??.ZIP</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">IF “%MM%“ == “06” ERASE LOGS-20??-04-??.ZIP</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">IF “%MM%“ == “07” ERASE LOGS-20??-05-??.ZIP</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">IF “%MM%“ == “08” ERASE LOGS-20??-06-??.ZIP</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">IF “%MM%“ == “09” ERASE LOGS-20??-07-??.ZIP</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">IF “%MM%“ == “10” ERASE LOGS-20??-08-??.ZIP</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">IF “%MM%“ == “11” ERASE LOGS-20??-09-??.ZIP</span></div>
<div><span class="Apple-style-span" style="font-family:'Courier New', Courier, monospace;font-size:x-small;">IF “%MM%“ == “12” ERASE LOGS-20??-10-??.ZIP</span></div>
<div style="font-family:Arial;"></div>
</div>
<div style="font-family:Arial;">With these patches in place, the Apex Data Loader can be a safe and reliable way to automatically import data files generated by another system. Though, no data integration system is foolproof, and you should still devise a custom validation report that runs every day to confirm that data is flowing into the system as expected.</div>
<div style="font-family:Arial;"></div>
<div style="font-family:Arial;">

While it would be wonderful if we could originate and retain all of our enterprise data only in Salesforce CRM, in real life, we still need to integrate with other systems, and, when we do, tools like the Apex Data Loader, with a pinch of salt, can help us get the job done.

For more Data Loader tips, drop by the blog [Using the Apex Data Loader with Salesforce](http://www.apexdataloader.com/).

</div>