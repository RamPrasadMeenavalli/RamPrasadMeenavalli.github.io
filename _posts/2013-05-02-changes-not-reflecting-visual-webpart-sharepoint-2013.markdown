---
layout: post
title: "Common build issues with Visual WebParts in SharePoint 2013"
date: 2013-05-02 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2013"]
permalink: /post/changes-not-reflecting-visual-webpart-sharepoint-2013
---
<!-- more -->
<h2>Problem Description</h2>
<p>Following are some issues observed while developing Visual Web Parts for SharePoint 2013 using Visual Studio 2012</p>
<ul class="spd-ul">
<li>Any changes made to the HTML markup are not reflected on the site</li>
<li>Build errors like <em>'The name InitializeControl does not exist in the current context'</em></li>
</ul>
<h2>Solution</h2>
<p>These errors/issues are caused because of the custom tool called 'SharePointWebPartCodeGenerator' which is used on the visual web part's user control file. All markup changes made to the user control are processed by this custom tool, to include all the design and markup elements in the DLL. In SharePoint 2013, the user control is not copied to the file system.<br /> The errors described above turn up due to some configuration issues of this custom tool. Follow the below steps to solve these errors.</p>
<ul class="spd-ul">
<li>Make sure the 'Site Url' property of your visual studio project is set to a valid SharePoint site's url. Make sure this site is accessible.</li>
<li>Check if the value of custom tool for the user control is set to 'SharePointWebPartCodeGenerator'. Follow the below steps.
<ul class="spd-ul">
<li>Select the user control from your 'Solution Explorer', and click F4 to show the properties of the user control.</li>
<li>In the properties window, check the value for 'Custom Tool'.</li>
<li>It should be set to 'SharePointWebPartCodeGenerator'. If not please set this value and save this.</li>
<li><img src="/assets/images/vwp2013_scr1.jpg" alt="" /></li>
</ul>
</li>
<li>Manually run the custom tool.
<ul class="spd-ul">
<li>From the solution explorer, right click on the user control of your visual web part and click on 'Run Custom Tool'.</li>
<li>Rebuild and deploy the web part.</li>
<li><img src="/assets/images/vwp2013_scr2.jpg" alt="" /></li>
</ul>
</li>
</ul>
<p>Now the changes made to the visual webpart should reflect and work fine after deploying it.</p>
