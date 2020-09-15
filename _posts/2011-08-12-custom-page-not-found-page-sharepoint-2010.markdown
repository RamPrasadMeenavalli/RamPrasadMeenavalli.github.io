---
layout: post
title: "Custom Page Not Found Page for SharePoint 2010 site"
date: 2011-08-12 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2010"]
permalink: ["/post/custom-page-not-found-page-sharepoint-2010"]
  ---
<!-- more -->
<p>Setting a custom <em>Page Not Found</em> page for a SharePoint 2010 site works the same way as we did for a SharePoint 2007 site in the previous <a title="Creating a Custom Page Not Found Page for SharePoint 2007 Site" href="http://spdeveloper.co.in/articles/pages/custom-page-not-found-page-2007.aspx">article</a>. Earlier we used to change the <em>Page Not Found</em> reference for a Web Application using SharePoint API. But in SharePoint 2010, we can do this easily using PowerShell.</p>
<p>Create the <em>custom404.html</em> file as described in the previous <a title="Creating a Custom Page Not Found Page for SharePoint 2007 Site" href="http://spdeveloper.co.in/articles/pages/custom-page-not-found-page-2007.aspx">article</a>, and put in the below location instead of the 12 hive.<br />\Program Files\Common Files\Microsoft Shared\Web Server Extensions\14\TEMPLATE\LAYOUTS\LangID<br /> (1033 is the language ID for U.S. English)<br />Now run the following commands in a PowerShell window.</p>
<pre class="brush:ps;auto-links:false;toolbar:false" contenteditable="false">$webapp =Get-SPWebApplication http://&lt;SiteUrl&gt;
$webapp.FileNotFoundPage = "custom404.html"
$webapp.update()</pre>
<p>The above code is self explanatory. We created a powershell variable which holds a reference to the WebApplication of your site. And using this variable, we can set the FileNotFoundPage property of the webApplication to the custom page's filename.<br />So, for a SharePoint 2010 site we can change the <em>Page Not Found</em> page reference without writing any console application or feature activation code.</p>
