---
layout: post
title: "SharePoint Framework : Provisioning multiple list instances through SPFx solution."
date: 2017-06-13 09:27:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "SharePoint Server", "SharePoint 2016", "SharePoint 2019", "SPFx"]
permalink: /post/sharepoint-framework-provisioning-multiple-list-instances-through-spfx-solution
---
<!-- more -->
<p>SharePoint Asstes, like Fields/Site Columns, Content Types, List Instances can be provisioned or deployed to a SharePoint Site through SharePoint Framework (SPFx) solutions. SPFx solution uses feature framework files through which we can define the XML to provision the necessary assets.</p>
<p>The following resources from SPFx documentation provides more details</p>
<ul class="spd-ul">
<li>Tutorial : <a href="https://dev.office.com/sharepoint/docs/spfx/web-parts/get-started/provision-sp-assets-from-package">https://dev.office.com/sharepoint/docs/spfx/web-parts/get-started/provision-sp-assets-from-package</a></li>
<li>More Details - <a href="https://dev.office.com/sharepoint/docs/spfx/toolchain/provision-sharepoint-assets">https://dev.office.com/sharepoint/docs/spfx/toolchain/provision-sharepoint-assets</a></li>
<li>Webcast - <a href="https://www.youtube.com/watch?v=r-UdJhhHlEQ">https://www.youtube.com/watch?v=r-UdJhhHlEQ</a></li>
</ul>
<h2>How to provision multiple List Instances?</h2>
<p>The key feature framework files to provision a list instance are</p>
<ul class="spd-ul">
<li><strong>elements.xml</strong> : Defines the metadata of the list along with list definition mapped using a CustomSchema property</li>
<li><strong>schema.xml</strong>: Defines the list structure, content types and views</li>
</ul>
<p>To provision more than one list instance, we can create <strong><em>two different schema files</em></strong> (like schemaOne.xml &amp; schemaTwo.xml), create two <strong><em>ListInstance</em></strong> elements in the elements.xml file referring to the respective schema files.</p>
<p><img src="/assets/images/SPFX_MultipleLists_1.png" alt="" /></p>
<p>Also, include all the schema files in the package-solution.json</p>
<p><img src="/assets/images/SPFX_MultipleLists_2.png" alt="" /></p>
