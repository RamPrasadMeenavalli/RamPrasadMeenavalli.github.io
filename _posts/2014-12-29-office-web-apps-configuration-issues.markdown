---
layout: post
title: "Word Web App error - Sorry, Word Web App can't open this document because the service is busy. Please try again later"
date: 2014-12-29 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2013", "SharePoint 2016"]
permalink: /post/office-web-apps-configuration-issues
---
<!-- more -->

<p>After configuring the Office Web App Server for SharePoint 2013 and creating the bindings on the SharePoint farm, I was able to open the excel files on the browser but not other documents like word, power point etc., When I opened a word document, the Word Web App opens, shows the processing/busy status for some time and throws the below error</p>
<p><em> Sorry, Word Web App can't open this document because the service is busy. Please try again later </em></p>
<p>After some troubleshooting, following is the checklist I prepared which might provide some information about these kind of issues</p>
<table class="spd-table" style="width: 95%;">
<thead>
<tr>
<th>Configuration/Limitation Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Is your Office Web App Server Installed on a non-system drive (mostly other than C drive)? <br /> Check my next <a title="Word Web App Error" href="http://spdeveloper.co.in/sharepoint2013/office-web-server-word-web-app-error.aspx">post</a> for the solution.</td>
</tr>
<tr>
<td>The SharePoint Web Application must use Claims-Based Authentication. Only then the Office Web Apps can open the files.</td>
</tr>
<tr>
<td>Make sure you're not logged in as System Account because you wont be able to open/view files with this account</td>
</tr>
<tr>
<td>If you set up Office Web Apps in a test environment that uses HTTP, make sure you set the AllowOAuthOverHttp setting to True<br /> Refer <a href="http://technet.microsoft.com/en-us/library/ff431687(v=office.15).aspx#oauth">technet</a>.</td>
</tr>
<tr>
<td>Office Web App Server cannot run on a Domain Controller. The server should be part of the domain.</td>
</tr>
<tr>
<td>Make sure you&rsquo;re running SharePoint 2013 build 15.0.4420.1017 or later. If not update your SharePoint farm.</td>
</tr>
<tr>
<td>Make sure the document doesn&rsquo;t exceed 10 megabytes.</td>
</tr>
</tbody>
</table>
<p>&nbsp;</p>
<p>Inspite of trying all the recomended solutions the issue didn't get fixed. I installed the Office Web App Server on D:, which is the culprit. If you installed on a non-system drive, many blogs suggest to uninstall and and re-install on a system drive. <strong>DO NOT</strong> do that. There is a simple and recommended solution for this problem. Check my next <a title="Word Web App Error" href="http://spdeveloper.co.in/sharepoint2013/office-web-server-word-web-app-error.aspx">post</a> to fix this.</p>
