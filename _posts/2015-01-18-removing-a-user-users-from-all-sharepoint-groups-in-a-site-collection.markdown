---
layout: post
title: "Removing a user/users from all SharePoint Groups in a Site Collection"
date: 2015-01-18 16:15:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "SharePoint Server", "SharePoint 2007", "SharePoint 2010", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/removing-a-user-users-from-all-sharepoint-groups-in-a-site-collection"]
  ---
<!-- more -->
<p>For some SharePoint implementation, we might end up creating many SharePoint groups and add users to these groups based on the business needs. Later we might need to remove a user from all these groups or the entire site collection. If we are having many groups spread across various sub sites within the site collection, it will be a tedious task to manually remove the users from all the SharePoint groups. There are many blogs which talk about writing some PowerShell script to traverse through all the SharePoint Groups and remove the user/users. If you are trying to remove the user/users completely from the site collection and all its groups, you can do this is in a simple way through UI. No need to write any code or PowerShell. Follow the below steps.</p>
<ul class="spd-ul">
<li>Login as Site Collection Administrator, click <em>'Site Actions'</em> -&gt; <em>'Site Settings'</em>.</li>
<li>Click on any exiting SharePoint Groups (eg., Memebers). This opens a new page, observe the URL of this page which will be something like below.<br /> http://&lt;&lt;siteurl&gt;&gt;/_layouts/people.aspx?MembershipGroupID=5</li>
<li>Now, change the query string value of <em>'MembershipGroupID'</em> to 0 (zero). This will show All People within the site collection.</li>
<li>Select the user/users, and from the Actions menu click on <em>'Delete Users from Site Collection'</em></li>
</ul>
<p>This will remove the selected user/users from all the SharePoint groups, libraries, lists and sub sites in the Site Collection.</p>
