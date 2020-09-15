---
layout: post
title: "Office Web App Server Installation on a non-system drive might cause issues with the Office Web Apps"
date: 2014-12-29 16:08:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2013", "SharePoint 2016"]
permalink: ["/post/office-web-server-word-web-app-error"]
  ---
<!-- more -->
<h3>Word Web App error - <em>"Sorry, Word Web App can't open this document because the service is busy. Please try again later"</em></h3>
<p>This is the most common error message from the word web app which pops up on a SharePoint Server 2013 Farm configured with a Office Web App Server. As described in the previous <a title="Word Web App Error" href="http://spdeveloper.co.in/sharepoint2013/office-web-apps-configuration-issues.aspx">article</a>, please verify the checklist if your Office Web Server or SharePoint farm has any of the mentioned configuration issues. Even after that if you are facing this error, check if you have installed the Office Web App Server on a non-system drive. If yes, then most likely this might be the problem.</p>
<p>When you install Office Web App Server on a non-system drive which is a security hardened (permissions removed) drive, Office Web Apps may not work properly. For a non-system drive the following accounts needs to be added to the intallation folders with the premissions as shown in the screens.</p>
<ul class="spd-ul">
<li>CREATOR OWNER</li>
<li>SERVERNAME\Users</li>
</ul>
<p><img src="/image.axd?picture=/owa-permsns.png" alt="" /></p>
<p><em>NOTE:</em> While adding the "CREATOR OWNER" account, in the permissions section 'Special Permissions' might be grayed out or you may not be able to select it. Just select the 'Read' permission and click on Apply and Ok. This will automatically set the 'Special Permission' for the account. After assigning these accounts the required permissions on the Office Web App Server installation folder, this error should be resolved.</p>
<p>If your Server is built using VMWare, then check this Microsoft Support <a href="http://blogs.technet.com/b/office_web_apps_server_2013_support_blog/archive/2014/03/26/office-web-apps-2013-amp-vmware.aspx">article</a> which will fix the issue.</p>
<p>In the worst case, un-install and clean your Office Web App Servers and re-install on a System Drive.</p>
