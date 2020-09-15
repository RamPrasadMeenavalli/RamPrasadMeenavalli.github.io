---
layout: post
title: "SharePoint Server 2013 : Mail Alerts for Search Crawl Errors using PowerShell Script"
date: 2015-03-03 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2013", "SharePoint 2016"]
permalink: ["/post/sharepoint2013-email-alerts-search-crawl-errors-powershell"]
  ---
<!-- more -->
<p>As a SharePoint Admin for a search intensive SharePoint 2013 application, I had to check the Crawl Logs everyday to see if there are any crawl errors for an item, and if any item is missed in the crawl. I always thought if we can simply receive a daily mail whenever there is an error in the crawl. This made me write this simple Powershell Script which sends the mail with the error details, and then schedule this to run daily. Now I don't need to open the Search Service Application everyday and check the crawl logs, I have the details in my mail the moment I start my day, which saves some time for me.. :)</p>
<h3>PowerShell Script</h3>
<p>As explained in my previous <a href="http://spdeveloper.co.in/sharepoint2013/export-search-crawl-log-to-excel.aspx">post</a> I have used the '<em>GetCrawledUrls()</em>' method of the '<em>Microsoft.Office.Server.Search.Administration.CrawlLog</em>' class. This script retrieves all the crawl error logs for the previous day and sends these details in a mail as an excel attachment. Below is the complete script and steps to achieve this.</p>
<pre class="brush:ps;auto-links:false;toolbar:false" contenteditable="false">#Powershell Script which to Send a dialy Alert Mail when an error occurs during Search Crawl.
#This solution is for SharePoint Server 2013 (on-Premises)
#Author : Ram Prasad Meenavalli

#----------------------------------------------------
#Parameters Required
#----------------------------------------------------
$ssaName = "Search Service Application"
$errorsFileName = "errors.csv"
$topErrorsFileName = "toplevelerrors.csv"

#$logMaxRows - This specifies the number of errors to be sent in the mail attachment. 
$logMaxRows = 10000

#$contentSourceID - This specifies the Content Source ID where we need to check for Crawl Errors.
#-1 will give crawl errors from all content sources.
$contentSourceID = -1



#----------------------------------------------------
# Email Variables
#----------------------------------------------------
$smtp = "smtp-server-address" 
$to = "ram@yourmail.com","prasad@yourmail.com"
$from = "no-reply@yourmail.com"
$subject = "Search Crawl Errors"
$body = "Your Search Service has encountered some errors while crawling the content. Please check the attachment/s for more details."

#----------------------------------------------------
# Constants
#----------------------------------------------------
$errorID = -1
$currentDate = Get-Date
$startDate = $currentDate.AddDays(-1)
$endDate = (($startDate.AddHours(23)).AddMinutes(59)).AddSeconds(59)


if ((Get-PSSnapin "Microsoft.SharePoint.PowerShell" -ErrorAction SilentlyContinue) -eq $null)
{
    Add-PSSnapin Microsoft.SharePoint.PowerShell
}

$ssa = Get-SPEnterpriseSearchServiceApplication -Identity $ssaName
$logs = New-Object Microsoft.Office.Server.Search.Administration.CrawlLog $ssa

$errors = $logs.GetCrawledUrls($true,$logMaxRows,"",$false,$contentSourceID,2,$errorID,$startDate,$endDate)
$topErrors = $logs.GetCrawledUrls($true,$logMaxRows,"",$false,$contentSourceID,4,$errorID,$startDate,$endDate)

if(($errors.Rows[0]["DocumentCount"] -gt 0) -or ($topErrors.Rows[0]["DocumentCount"] -gt 0))
{
	$logs.GetCrawledUrls($false,$logMaxRows,"",$false,$contentSourceID,2,$errorID,$startDate,$endDate) | export-csv -notype $errorsFileName
	$logs.GetCrawledUrls($false,$logMaxRows,"",$false,$contentSourceID,4,$errorID,$startDate,$endDate) | export-csv -notype $topErrorsFileName	

	send-MailMessage -SmtpServer $smtp -To $to -From $from -Subject $subject -Body $body -BodyAsHtml -Attachments $errorsFileName,$topErrorsFileName
}</pre>
<ul class="spd-ul">
<li>Copy the script above and change the parameters/values appropriately to suit your needs.</li>
<li>Save this script as <em>errorMailAlerts.ps1</em> or any other relevant name with .ps1 extension and save it on a SharePoint Server.</li>
</ul>
<h3>Schedule the script</h3>
<p>This powershell script should be scheduled to run daily using the windows Task Scheduler. Follow the below steps for scheduling a task</p>
<ul class="spd-ul">
<li>Login to the SharePoint Server where the .ps1 file is saved.</li>
<li>Open the Windows Task Scheduler and select the Create Task option.</li>
<li>Enter a name for the task, and give it a description.</li>
<li>In the General tab, go to the Security options heading and specify the user account that the task should be run under. Change the settings so the task will run if the user is logged in or not.</li>
<li>In the Triggers tab add a new trigger for the scheduled task. Select the Start Date and the frequency to run once everyday at 1:00 AM (or any desired time).</li>
<li>In the Actions tab, add a new Action and set it to Start a program.</li>
<li>In the Program/script box enter "PowerShell."</li>
<li>In the Add arguments box enter the value ".\errorMailAlerts.ps1."</li>
<li>In the Start in box, add the complete path of the folder that contains your PowerShell script.</li>
<li>Click OK when all the desired settings are made.</li>
</ul>
<p>This runs the powershell script daily and the admins will receive a mail with the crawl error details if any.</p>
