---
layout: post
title: "Manage Checked Out Files in SharePoint"
date: 2011-01-14 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Online", "Classic Sites", "SharePoint Server", "SharePoint 2007", "SharePoint 2010", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: /post/manage-checked-out-files-in-sharepoint
---
<!-- more -->
<h2>Problem Description</h2>
<p>In most of the site templates provided by SharePoint Server we have a default '<em>Pages</em>' library, which is used for holding the content pages for the site . Sometimes while creating pages, we get an error saying 'The page name already exists' or 'its checked out for editing by another user'. Below is the actual error message we get</p>
<p><em>The file 'http://ram/pages/spdeveloper.aspx' is checked out or locked for editing by XXXX\UserName</em></p>
<p>But when we go to pages library and try to find that page, it may not be there. It will only be visible from that user's login which is displayed in the error page. Once that user checks-in the page, then everyone can find that page in the Pages library and can start editing it. This happens because there is no checked in version of the page, the user might have created it and left it without checking in. What if that user is not available or if we cannot access that account. Are we in trouble? No..SharePoint gives us the control of taking ownership of such pages.</p>
<h2>Solution</h2>
<p>We can to go the 'Manage Checked Out Files' page of the respective page library and can take the ownership. This is available in both SP 2010 and MOSS 2007 . Below are the detailed steps for doing this.</p>
<ul class="spd-ul">
<li>Go to the '<strong>Document Library Settings</strong>' page of the respective '<em>Pages</em>' library.</li>
<li>Under the '<strong>Permissions and Management</strong>' group, there is a link named '<strong>Manage files which have no checked in version</strong>' (in SPS 2010) or '<strong>Manage checked out files</strong>' (in MOSS 2007). On clicking this link, a page like below will be opened.</li>
</ul>
<p><img src="/assets/images/mngchckdoutfls.png" alt="" /></p>
<ul class="spd-ul">
<li>As shown in the above screenshot, all the pages which are checked out (including the pages which are not having a checked in version) are shown.</li>
<li>We can select the required pages, and click on the 'Take Ownership of selection' link to get the control of the selected page/s.</li>
</ul>
<h2>Permission Level</h2>
<p>Only Site Collection Administrators or users with permission levels of 'Full Control', 'Design' or 'Manage Hierarchy' can perform this task.</p>
<p>Note: The 'Manage Checked Out Files' page shows the User account to which the page is checked out, under the 'Checked Out To' column. In the above screenshot, I have removed the user account details for privacy</p>
