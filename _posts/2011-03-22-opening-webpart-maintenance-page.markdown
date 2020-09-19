---
layout: post
title: "Opening WebPart Maintenance Page in a SharePoint site"
date: 2011-03-22 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "SharePoint Server", "SharePoint 2007", "SharePoint 2010", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: /post/opening-webpart-maintenance-page
---
<!-- more -->
<h2>What is WebPart Maintenance Page?</h2>
<p style="text-align: justify;">WebPart Maintenance page is an OOB page, which is used to manage the webparts in a webpart page. A webpart which is causing a problem in a webpart page can be removed using this page. Basically, a WebPart Maintenance page provides the list of all webparts added in a particular page and options to close or remove these webparts.</p>
<p style="text-align: justify;">Sometimes after adding a webpart to the page, we get an error page with some message. But in this error page we wont get any SharePoint controls (like the SiteActions, Ribbon Menu etc.,), so we cannot edit the page and remove the webpart. Then we usually go to the edit form of the page (through page library) and then click on the 'Open Web Part Page in maintenance view' link, which opens the WebPart Maintenance page to remove the webpart which is causing the problem</p>
<h2>Shortcut to open</h2>
<p style="text-align: justify;">Instead of going to the Edit Properties form of the page, we can easily open the WebPart Maintenance page by just adding the following query string to the page URL<br /> <em><strong>?contents=1</strong></em><br /> So, if your page URL is 'http://rams/pages/default.aspx' then after appending the query string it should look like ' <em><strong>http://rams/pages/default.aspx?contents=1</strong></em>'</p>
<p style="text-align: justify;">Using this approach, we can open the webpart maintenance page for all the forms pages (like /pages/forms/allitems.aspx?contents=1). But its not safe to remove or add any webparts in these pages, as they may effect the normal functionality.</p>
<p style="text-align: justify;">This approach works on both MOSS 2007, SPS 2010, SharePoint Server 2013 and SharePoint Server 2016</p>
