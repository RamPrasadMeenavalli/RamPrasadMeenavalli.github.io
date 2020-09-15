---
layout: post
title: "What's New in SharePoint Server 2016 - Insights revealed during Ignite 2015"
date: 2015-05-14 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2016"]
permalink: ["/post/whats-new-sharepoint-server-2016-ignite-2015"]
  ---
<!-- more -->
<p style="text-align: justify;">With the announcement of SharePoint Server 2016, the most trending discussion in the space of SharePoint is about the new features and capabilities that are expected to be part of SharePoint Server 2016. Microsoft Ignite sessions conducted in the first week of May 2015, have revealed many interesting insights about the new version.</p>
<p style="text-align: justify;">As we all know, since its evolution in 2001 till SharePoint Server 2013, the on-premises version played a major role in defining the architecture and setting the guidance for its respective Cloud versions. However, this is going to change with SharePoint Server 2016, where Office 365 is set as a base and to define how the on-premises version works. Few features which are expected to come with SharePoint 2016 are already part of Office 365, like Delve, Office Graph, Cloud Search Service Applications etc.,. One significant design change we should note is that SharePoint 2016 is being designed to support more seamless hybrid implementations and scenarios. Before we wait for the Beta which is planned to be available in Q4 2015, and the RTM in Q2 2016, here are few features or functionalities revealed in the Ignite Sessions that grabbed my attention.</p>
<h2>Delve People Profile</h2>
<p style="text-align: justify;">Delve is a machine learning technology which is familiar to many Office 365 users and developers. SharePoint Server 2016 is going to replace the 'My Profile' page with the Delve People Profile. The new profile page will be a one stop place which shows all the details about a person, their contact details, conversations and organization chart. While most of these are available in the earlier versions of SharePoint Server, the interesting changes are, the ability for people to promote their information and provision to write blogs through their profile page. The new profile page shows a blogs section, which allows people to read others' blogs or to post their own. The new blogging engine has an authoring canvas which is very different from the existing blog sites. Its gives a very clean and interactive interface for people to easily add headings, images, attachments to a post. And also the blogs are auto saved as when you type and gives an option for the author to publish it. The authoring interface is little similar (not exactly same though) to Microsoft <a href="https://sway.com/">Sway</a>, which you can explore.</p>
<h2>Files Storage Limits</h2>
<p style="text-align: justify;">With SharePoint Server 2016, one can upload a file of up to 10 GB. This change is already rolled out to many Office 365 users, and the personal space for each user is increased to 1 TB (at the time of writing this article).</p>
<h2>Hybrid Implementations</h2>
<p style="text-align: justify;">SharePoint Server 2016 is being designed to support more hybrid scenarios. Though SharePoint Server 2013 supports a few, they are not seamless. For example, we can provision a hybrid search center in SharePoint 2013 using Query Federation, which shows the search results from Cloud and your on premises intranet. But the results are shown as two different sets which cannot be refined or filtered simultaneously. With SharePoint Server 2016, the same scenario can be implemented with a Hybrid Search Center, which gives a unified (single) results set. The content from both the cloud and on premises intranet are shown as a single result set and one can refine or filter easily. </p>
<h2>MinRole Topology</h2>
<p style="text-align: justify;">Building a good SharePoint farm involves assigning and defining the roles on different servers in the farm, which is followed for various versions of SharePoint now. Till SharePoint Server 2013, these roles and the services to be activated for each role are shared as a guidance and are well documented by Microsoft for various scenarios. With SharePoint 2016's MinRole, we do not need to manually activate or start the services to define the role of a server, the Product and Configuration Wizard (psconfig.exe) comes with additional parameters where we can select the type of role for the server, and it then programmatically activates the services for that role. Following are the different roles that will be available.</p>
<table class="spd-table" style="width: 90%;">
<thead>
<tr>
<th>Role Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Special Load</td>
<td>Reserved for services to be isolated from other services, I.e. 3rd party, Performance Point, etc</td>
</tr>
<tr>
<td>Web Front End</td>
<td>Services end user requests, optimized for low latency.</td>
</tr>
<tr>
<td>Single Server Farm</td>
<td>Provisions all services on the server for a single server deployment. This role is provided for evaluation and development purposes.</td>
</tr>
<tr>
<td>Search</td>
<td>Reserved for Search services.</td>
</tr>
<tr>
<td>Application</td>
<td>Services the back-end jobs or the requests triggered by back-end jobs, optimized for high throughput.</td>
</tr>
<tr>
<td>Distributed Cache</td>
<td>Services distributed cache for the farm. Optionally, the server assigned to this role can load balance end user requests among the web front ends.</td>
</tr>
</tbody>
</table>
<p><small>Reference - Bill Baer's <a href="http://blogs.technet.com/b/wbaer/archive/2015/05/12/what-s-new-in-sharepoint-server-2016-installation-and-deployment.aspx">Blog</a></small></p>
<p style="text-align: justify;">Though, technically this was available in the earlier versions of SharePoint, any wrong configuration done increases the latency of the farm and reduces the overall user response time. With the automated MinRole role based installation the probability of this risk is reduced.</p>
<h2>Patching Improvements</h2>
<p style="text-align: justify;">The Cumulative Updates (CUs) which are released on a monthly basis now (bi-monthly earlier) usually come as very large files (sometime more than 2GB) and it always involves a down time for applying these patches. SharePoint 2016 promises to release path files which will be smaller in size and the most interesting part is, it allows a <em>Zero Downtime Patching</em>, which means our end users will never know that we are applying patches on the servers.</p>
<p style="text-align: justify;">This is not all. There are other big features of SP2016, which I will be sharing in my next blog. I know it's too difficult to wait till Q4 15 to have a hands-on, but we have to ... 😉</p>
