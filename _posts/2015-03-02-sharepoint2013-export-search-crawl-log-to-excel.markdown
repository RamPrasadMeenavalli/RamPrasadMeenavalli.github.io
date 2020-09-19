---
layout: post
title: "SharePoint Server 2013 : Exporting Search Crawl Logs using PowerShell"
date: 2015-03-02 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2013", "SharePoint 2016"]
permalink: /post/sharepoint2013-export-search-crawl-log-to-excel
---
<!-- more -->
<p>Search crawl logs play a crucial role while troubleshooting search issues in SharePoint Server 2013. But its very difficult for a SharePoint Administrator to go through the logs, 50 at a time and navigate between different sets of logs to understand the issue and observe a pattern for the errors or warnings. Its very easy to sort and filter the logs if we have all the details in an excel. The UI of Search Service Application doesn't provide any interface for exporting the logs, but the logs can be retrieved from the Search Service Application using the '<em>Microsoft.Office.Server.Search.Administration.CrawlLog</em>' class. This class has a method called '<em>GetCrawledUrls</em>' which can retrieve the logs. The 2013 documentation says that this method is obsolete, so be cautious if you are planning to use this script for SharePoint Server 2016 or later.</p>
<h3>PowerShell Script to export Crawl Logs</h3>
<p>Below is a PowerShell script which retrieves the crawl logs and exports them to an excel.</p>
<pre class="brush:ps;auto-links:false;toolbar:false" contenteditable="false">$errorsFileName = "D:\Ram\CrawlLogs.csv"
$ssa = Get-SPEnterpriseSearchServiceApplication -Identity "Search Service Application"
$logs = New-Object Microsoft.Office.Server.Search.Administration.CrawlLog $ssa
$logs.GetCrawledUrls($false,10000,"",$false,1,2,-1,[System.DateTime]::MinValue,[System.DateTime]::MaxValue) | export-csv -notype $errorsFileName</pre>
<h3>Explaining the method's Parameters</h3>
<p>The '<em>GetCrawledUrls</em>' method has the following parameters</p>
<p class="ac">GetCrawledUrls(bool getCountOnly,long maxRows,string urlQueryString,bool isLike,int contentSourceID,int errorLevel,int errorID,DateTime startDateTime,DateTime endDateTime)</p>
<ul class="spd-ul">
<li><em><strong>Return Value</strong></em> - DataTable</li>
<li><em><strong>getCountOnly</strong></em> - If true, returns only the count of URLs matching the given parameters.</li>
<li><em><strong>maxRows</strong></em> - This parameter specifies the number of rows to be retrieved.</li>
<li><em><strong>urlQueryString</strong></em> - The prefix value to be used for matching the URLs</li>
<li><em><strong>isLike</strong></em> - If true, all URLs that start with 'urlQueryString' will be returned.</li>
<li><em><strong>contentSourceID</strong></em> - This is the ID of the content source for which crawl logs should be retrieved. If -1 is specified, URLs will not be filtered by content source. How to get the <a href="http://spdeveloper.co.in/sharepoint2013/export-search-crawl-log-to-excel.aspx#getcid">Content Source ID</a>?</li>
<li><em><strong>errorLevel</strong></em> - Only URLs with the specified error level will be returned.Possible Values - <br />-1 : Do not filter by error level.<br />0 : Return only successfully crawled URLs.<br />1 : Return URLs that generated a warning when crawled.<br />2 : Return URLs that generated an error when crawled.<br />3 : Return URLs that have been deleted.<br />4 : Return URLs that generated a top level error.</li>
<li><em><strong>errorID</strong></em> - Only URLs with this error ID will be returned. If -1 is supplied, URLs will not be filtered by error ID.</li>
<li><em><strong>startDateTime</strong></em> - Start Date Time. Logs after this date are retrieved.</li>
<li><em><strong>endDateTime</strong></em> - End Date Time. Logs till this date are retrieved.</li>
</ul>
<h3 id="getcid">Get Content Source ID</h3>
<ul class="spd-ul">
<li>Open the Search Service Application.</li>
<li>Click on the Content Sources from the left navigation menu.</li>
<li>On this page, all the Content Sources are shown. Click on your required Content Source and check the URL in the address bar. The value of the query string parameter <strong>cid</strong> is the content source id.</li>
</ul>
<p class="ac">http://&lt;server-url&gt;/_admin/search/editcontentsource.aspx?<strong>cid=2</strong>&amp;appid={GUID}</p>
<p>Based on this PowerShell snippet, we can even write a small utility which will send a daily mail with the list of errors occurred in the previous days crawling. Check my next <a href="http://spdeveloper.co.in/sharepoint2013/email-alerts-search-crawl-errors-powershell.aspx">post</a> for details on how to do this.</p>
