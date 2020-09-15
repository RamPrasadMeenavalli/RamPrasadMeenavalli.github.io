---
layout: post
title: "Developer Dashboard in SharePoint 2010"
date: 2011-04-07 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2010"]
permalink: ["/post/developer-dashboard-in-sharepoint-2010"]
  ---
<!-- more -->
<p>The <em><strong>Developer Dashboard</strong></em> is a new feature of SharePoint 2010 that is designed to give details and reports about the operations going on behind the scenes when a page is loaded. It breaks the statistics into various components and gives detailed information. Using the developer dashboard we can get the time taken for the request to be completed. This is further divided into various method levels in the request cycle, like 'BeginRequestHanlder', 'PostAuthenticateRequestHandler' etc., and shows the time taken by each of these methods. It also shows the execution time taken; Page checked out level etc., it also lists all the Stored Procedures which were executed and the time taken by each procedure.</p>
<p>Developer dashboard can be in three states - "On", "Off" or "OnDemand". If it is On, then the dashboard is always displayed in a SharePoint page, at the bottom. If its set to "OnDemand", then we get a dashboard icon at the top right corner of the page, and on clicking this we will get the report generated at the bottom of the page. And we can set this off by changing the Dashboard display level to Off. This can be enabled/disabled through Object Model, PowerShell commands or by using STSADM.</p>
<div class="gad" style="height: 15px; min-width: 468px;">&nbsp;</div>
<p>As a developer we should enable this and check the performance as we develop code for SharePoint 2010. Following are the methods for enabling/disabling developer dashboard.</p>
<h2>Enabling Developer Dashboard using PowerShell</h2>
<pre class="brush:ps;auto-links:false;toolbar:false" contenteditable="false">$devdashboard =[Microsoft.SharePoint.Administration.SPWebService]::ContentService.DeveloperDashboardSettings;
$devdashboard.DisplayLevel = 'OnDemand';
$devdashboard.Update()
Write-Host ("Developer Dashboard Level: " + $contentService.DeveloperDashboardSettings.DisplayLevel)</pre>
<h2>Enabling Developer Dashboard using Object Model</h2>
<pre class="brush:csharp;auto-links:false;toolbar:false" contenteditable="false">SPWebService service = SPWebService.ContentService;
service.DeveloperDashboardSettings.DisplayLevel=Microsoft.SharePoint.Administration.SPDeveloperDashboardLevel.OnDemand;
service.Update();</pre>
<h2>Enabling Developer Dashboard using STSADM</h2>
<pre class="brush:bash;auto-links:false;toolbar:false" contenteditable="false">STSADM.exe -o setproperty -pn
developer-dashboard -pv OnDemand</pre>
<p>The Developer Dashboard can be disabled by setting the display level value to "Off". As needed the display level can be set to "On", "Off" or "OnDemand" in the above code snippets. This operation requires a farm administrator permission. Make sure you have farm admin access before enabling/disabling developer dashboard.</p>
