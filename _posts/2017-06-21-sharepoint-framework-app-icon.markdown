---
layout: post
title: "SharePoint Framework App Icon"
date: 2017-06-21 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "SharePoint Server", "SharePoint 2016", "SharePoint 2019", "SPFx"]
permalink: ["/post/sharepoint-framework-app-icon"]
  ---
<!-- more -->
<p>SharePoint Framework solutions offer <strong>Client Side Webparts</strong> and <strong>SPFx Extensions</strong> to add custom functionality to your SharePoint sites. When we package these components, they are bundled as a SharePoint Framework App Package with an extension <strong>.sppkg</strong>. These packages are installed (like the SharePoint Add-Ins) and shown as custom Apps in your Site Contents page. Showing a custom App Icon on this page for your App is important, as it can differentiate your App easily from other custom ones.</p>
<p><img src="/image.axd?picture=/SPFX_AppICON_1.png" alt="" /></p>
<h2>Create an AppIcon image</h2>
<p>Prepare your AppIcon image with a dimension of <strong>96X96</strong>. This is important, as images without this dimension are not applied as App Icons.</p>
<h2>Set the Image as App Icon</h2>
<ul class="spd-ul">
<li>Copy the image to the sharepoint folder within your SharePoint framework solution directory.</li>
<li>Open the <strong>package-solution.json</strong> file from the <strong>config</strong> folder and specify the App Icon using the <strong>iconPath</strong> property. (this accepts a path relative from the sharepoint folder).</li>
<li>Build and package the app using gulp bundle &amp; gulp package-solution commands.</li>
<li>Add the app to your App Catalog and install in a site collection. The App will now have a custom App Icon.</li>
</ul>
<p><img src="/image.axd?picture=/SPFX_AppICON_2.png" alt="" /></p>
<p>However, this icon is not used in the Modern Pages, and only shown in the Site Contents page on a Classic view.</p>
<p>Its also a good idea to show a custom icon for client side webparts within your SPFx package. Steps to do this in my next post - <a title="SharePoint Framework: Set custom icons for Client Side Webparts" href="http://spdeveloper.co.in/sharepoint2016/sharepoint-framework-client-webparts-custom-icons.aspx">SharePoint Framework: Set custom icons for Client Side Webparts</a></p>
