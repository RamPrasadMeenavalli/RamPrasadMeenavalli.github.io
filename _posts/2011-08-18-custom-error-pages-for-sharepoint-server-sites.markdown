---
layout: post
title: "Mapping Custom Error Pages for SharePoint Server Sites"
date: 2011-08-18 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2010", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/custom-error-pages-for-sharepoint-server-sites"]
  ---
<!-- more -->
<p>We often create internet/intranet portals using SharePoint Server. In all the projects which I worked, one common requirement is to set <em>custom error pages</em> for the application. We were asked to display some custom error page instead of the default ERROR (HTTP STATUS - 500) page, ACCESS DENIED (HTTP STATUS - 401) page etc.,</p>
<p>In SharePoint 2007, we used to accomplish this requirement by implementing a HTTP Module, and we used to redirect to the appropriate pages based on the Http Status Code. Implementing this task in SharePoint 2010 is pretty easy now. The <em> SPWebApplication</em> object has a method called <em>UpdateMappedPage</em> which accepts two parameters. First one is an enum constant of type <em>SPWebApplication.SPCustomPage</em> which defines the type of error page to be mapped, and the second parameter is the URL of the custom error page. So, we can update many custom pages by calling this method of the SPWebApplication object. The <em>SPWebApplication.SPCustomPage</em> has the following values</p>
<ul class="spd-ul">
<li>AccessDenied</li>
<li>Confirmation</li>
<li>Error</li>
<li>Login</li>
<li>RequestAccess</li>
<li>Signout</li>
<li>WebDeleted</li>
</ul>
<div class="gad" style="height: 15px; width: 468px;">&nbsp;</div>
<p>So, using this method we can set custom error pages for all the above pages.<br /> Lets see this in action. Follow the below steps</p>
<ul class="spd-ul">
<li>Create a new Project in Visual Studio and select <em>'Empty SharePoint Project'</em> template.</li>
<li>Name the project as <em>'CustomErrorPages'</em>, and click on OK</li>
<li>Give the URL of your SharePoint site, and select <em>'Deploy as a farm solution'</em> when asked for the deployment type</li>
<li>After the solution is created, right click on the project and add the <em>"SharePoint Layouts mapped folder"</em>. This creates a mapped folder in the solution with a sub-folder named 'CustomErrorPages'</li>
<li>Right Click on this sub folder and click on Add-&gt;New Item.</li>
<li>From the templates, select <em>'Application Page'</em>. And add the required content on this page with the desired UI</li>
<li>Now, right-click on the features folder and click <em>'Add Feature'</em></li>
<li>Make the scope of the feature as <em>"WebApplication"</em> and add a feature receiver for this</li>
<li>The FeatureActivated and FeatureDeactivating methods should have the code as shown below</li>
</ul>
<pre class="brush:csharp;auto-links:false;toolbar:false" contenteditable="false">public override void FeatureActivated(SPFeatureReceiverProperties properties)
        {
            SPWebApplication webApp = properties.Feature.Parent as SPWebApplication;
            if (null != webApp)
            {
                if(!webApp.UpdateMappedPage(SPWebApplication.SPCustomPage.Error, CustomErrorPage))
                {
                    throw new ApplicationException("Cannot create new error page mapping !!");
                }
                webApp.Update(true);
            }
        }


        public override void FeatureDeactivating(SPFeatureReceiverProperties properties)
        {
            SPWebApplication webApp = properties.Feature.Parent as SPWebApplication;
            if (null != webApp)
            {
                if (!webApp.UpdateMappedPage(SPWebApplication.SPCustomPage.Error, null))
                {
                    throw new ApplicationException("Cannot reset error page mapping");
                }
                webApp.Update(true);
            }
        }</pre>
<p>Deploy this solution on your SharePoint site. Install the feature using stsadm command or through powershell. Now type the url as 'http://siteurl/page~s/default.html'. This should open the custom error page which we created in our solution.<br /> Note: If the Custom error page doesn't open, then re-install the feature, recycle the Application Pool and try again.</p>
<p>Using the above code we can map custom pages for other scenarios as well, by passing different <em>SPWebApplication.SPCustomPage</em> values to the <em>UpdateMappedPage</em> method.</p>
