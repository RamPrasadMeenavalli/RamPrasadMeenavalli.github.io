---
layout: post
title: ""Open Tool Pane" link in a custom webpart"
date: 2012-01-29 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2010", "SharePoint 2013"]
permalink: ["/post/open-tool-pane-link-in-a-custom-webpart"]
  ---
<!-- more -->
<p>Most of the times when we create custom webparts, we define custom properties in it. These properties are used to store some configuration details for the web part to function. In many custom web parts I saw, developers use custom properties in a webpart to store the data source for the web part. Data sources can be a List name, Document Library name or a Connection string for SQL Server database etc.,</p>
<p>While creating pages and adding web parts, if authors leave the properties empty, the web part may throw some error.</p>
<p>If you add a Content Query Web Part in your page, and leave the properties empty, then it displays a link to the user, which says <em><strong>Open Tool Pane</strong></em>. On clicking this link the tool pane of the webpart is opened. Its a good practice to provide this type of link on our custom web part as well, when the properties are left empty. Lets see how this can be achieved</p>
<p><img src="/image.axd?picture=/open_tool_pane.png" alt="" /></p>
<p>Before processing with the logic/data in the custom webpart, check if the required properties are empty. If they are found to be empty, then render an anchor tag in your webpart whose HREF should be set to the below JavaScript function</p>
<p><em><strong>MSOTlPn_ShowToolPane2Wrapper()</strong></em></p>
<p>The prepared anchor tag should look as shown below</p>
<pre class="brush:html;auto-links:false;toolbar:false" contenteditable="false">&lt;a href="javascript:MSOTlPn_ShowToolPane2Wrapper('Edit',this,'ID of the webpart');" title="Open The Tool Pane"&gt;Open The Tool Pane&lt;/a&gt;</pre>
<p>I created a sample webpart to show this working, you can download it from <a title="Sample Code" href="http://spdeveloper.co.in/documents/TextScroller.zip">here</a>. In this sample the <em>Open Tool Pane</em> link is displayed only when the page is in design mode.</p>
