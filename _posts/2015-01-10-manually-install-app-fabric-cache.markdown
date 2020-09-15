---
layout: post
title: "Products Prepartion Tool - Application Server Role. Windows Server (IIS) Role: Configuration Error & Windows Server AppFabric: Intallation Skipped"
date: 2015-01-10 16:12:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2013", "SharePoint 2016"]
permalink: ["/post/manually-install-app-fabric-cache"]
  ---
<!-- more -->
<h3>Application Server Role. Windows Server (IIS) Role: Configuration Error</h3>
<p>This issues occurs in some server due a problem in the Microsoft SharePoint Server 2013 Products Preparation Tool. This is issue has been confirmed by Microsoft and follow this <a title="The Products Preparation Tool in SharePoint Server 2013 may not progress past &quot;Configuring Application Server Role, Web Server (IIS) Role&quot;" href="https://support.microsoft.com/kb/2765260?wa=wsignin1.0">KB Article</a> on steps to solve this error.</p>
<h3>Windows Server AppFabric: Intallation Skipped</h3>
<p>Even after enabling the IIS Server role as described in the Microsoft Support Article, and running the Products Preparation Tool, you might get the error for App Fabric Cache installation. Inspite of running the Products Preapration Tool, the Windows Server AppFabric Installation is always skipped and cannot proceed further with SharePoint installation. To solve this manually install the App Fabric Cache using the following command in PowerShell.</p>
<pre class="brush:ps;auto-links:false;toolbar:false" contenteditable="false">$appfabric_setup = "&lt;&lt;Full Folder Path&gt;&gt;\WindowsServerAppFabricSetup_x64.exe"
&amp; $appfabric_setup /i CacheClient","CachingService","CacheAdmin /gac</pre>
<p>After manually installing the Window App Fabric Cache, run the products prepartion tool again which completes the pre-requisites installation successfully and you can continue with the SharePoint intallation.</p>
