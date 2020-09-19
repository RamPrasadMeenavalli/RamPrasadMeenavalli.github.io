---
layout: post
title: "Creating custom Page Not Found Page"
date: 2011-03-09 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2007", "SharePoint 2010"]
permalink: /post/custom-page-not-found-page-sharepoint-2007
---
<!-- more -->
<p>If we request for a broken URL or an invalid URL on a SharePoint site, we get the return status as 404 from the server and we get the default 404 page. But, for many sites (on internet/intranet) we need a custom page with a custom UI whenever the return status is 404. Follow the below steps to create a custom 404 page for a SharePoint WebApplication.</p>
<h2>Create a custom 404 page</h2>
<p>Before creating a new custom 404 page, lets understand how the default <em>sps404.html</em> works. There is a html page named '<em>sps404.html</em>' at '\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\LangID' This is a simple html page which is opened when the status is 404 for a requested URL. If we go through the code of this HTML file, we can understand that it redirects to a page at <em>/_layouts/spsredirect.aspx</em>. Now follow the below steps and create a custom 404 page.</p>
<ul class="spd-ul">
<li>Make a copy of the default <em>sps404.html</em> and name it as <em>custom404.html</em></li>
<li>Change the redirection URL (in the copied file, ie., custom404.html) to any other URL where your custom page not found page is created. (this page can be in the pages library or an application page in _layouts</li>
</ul>
<p><em>Note:If we need a simple static page, then we can put that content in the <em>custom404.html</em> file itself.</em></p>
<ul class="spd-ul">
<li>Now copy the custom html file to the below path</li>
</ul>
<p><em>\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\LangID</em>. (1033 is the language ID for U.S. English)<br />So, we have the custom 404 page ready. </p>
<h2>Setting the Page Not Found reference for the Web Application</h2>
<p>Now the reference for Page not Found for the web application should be changed through API. <strong><em>SPWebApplication</em></strong> object has a property called <strong><em>FileNotFoundPage</em></strong>. This property should be set with the name of the newly created page. As a best practice we will write a feature which sets the Custom Page Not Found Page for a web application on activation and resets the page not found on deactivating.<br />The Feature Activated code should be as shown below</p>
<pre class="brush:csharp;auto-links:false;toolbar:false" contenteditable="false">public override void FeatureActivated(SPFeatureReceiverProperties properties)
     {
         SPWebApplication webApp = properties.Feature.Parent as SPWebApplication;
         if (webApp != null)
         {
             webApp.FileNotFoundPage = "custom404.html";
             webApp.Update(true);
         }
     }</pre>
<p>The Feature Deactivating method should have the code as shown below</p>
<pre class="brush:csharp;auto-links:false;toolbar:false" contenteditable="false">public override void FeatureDeactivating(SPFeatureReceiverProperties properties)
     {
         SPWebApplication webApp = properties.Feature.Parent as SPWebApplication;
         if (webApp != null)
         {
             webApp.FileNotFoundPage = "";
             webApp.Update(true);
         }
     }</pre>
<p>After activating this feature, the custom page not found is set for the web application. And on deactivating, it is reset.</p>
<p>If we don't want to write a feature for this task, the code which is there in the FeatureActivated method (with some changes) can be kept in a simple console application and can be executed. The code to be executed through a console application should be as shown below</p>
<pre class="brush:csharp;auto-links:false;toolbar:false" contenteditable="false">Microsoft.SharePoint.Administration.SPWebApplication webapp = 
Microsoft.SharePoint.Administration.SPWebApplication.Lookup(new Uri("URL OF THE SITE"));
            webapp.FileNotFoundPage = "custom404.html";
            webapp.Update();</pre>
<p>This approach works for a SharePoint 2010 site also. But instead of setting the custom error page through SharePoint API, we can use powershell in SharePoint 2010. Read how to do this using powershell in the next <a title="Setting Custom Error Page for SharePoint 2010 Sites using PowerShell" href="http://spdeveloper.co.in/articles/pages/custom-page-not-found-page-sharepoint-2010.aspx">article</a></p>
