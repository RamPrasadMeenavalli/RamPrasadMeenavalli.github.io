---
layout: post
title: "How to migrate Infopath Path form with code from MOSS (SharePoint 2007) to SharePoint Server 2013"
date: 2015-09-02 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2013", "SharePoint 2016"]
permalink: /post/InfoPath-Form-With-Code-Migrate-From-MOSS-To-SharePoint2013
---
<!-- more -->
<p>In one of the migrations from MOSS to SharePoint 2013, I had a application which is using InfoPath (2007) form with code. In this blog I would share the difficulties we faced While migrating this application to SharePoint 2013, and solutions we followed to get the InfoPath form with code working.</p>
<p>I followed the <em>Database Upgrade</em> approach for migrating the MOSS applications to SharePoint Server 2013. The Content Databases were moved to a SharePoint Server 2010 environment, upgraded and then moved to SharePoint 2013 environment. In the SharePoint 2013 environment, I was unable to access the InfoPath form. Following are the symptoms I observed</p>
<ul class="spd-ul">
<li>I tried to open the InfoPath form using the InfoPath Designer 2013 and received the below error<br /><center><em><strong>InfoPath has encountered an error. The operation failed</strong></em></center></li>
<li>I tried to download the form from the 'Forms Template' library on the site, so that I can open in designer and publish the form. This also failed, as the attempt to download the XSN file resulted in an error on the site.</li>
<li>Tried to download the form (XSN) file from the MOSS site and opened it with the InfoPath Designer 2013. Received the same error.<br /><center><em><strong>InfoPath has encountered an error. The operation failed</strong></em></center></li>
</ul>
<h2>Solution</h2>
<p>After many trails, with the following solution I was able to make the InfoPath form with code work in a migrated SharePoint Server 2013 site.</p>
<ul class="spd-ul">
<li>Delete/Remove the existing InfoPath from the site (migrated SharePoint 2013 site)</li>
<li>Upload the Published InfoPath from to the Central Admin Form Templates and Activate it to the appropriate Site Collection</li>
</ul>
<p>Below are the detailed steps</p>
<h3>Delete/Remove the existing InfoPath from</h3>
<p>Follow the below steps to delete the existing InfoPath form</p>
<ul class="spd-ul">
<li>Open the Form Templates library on your SharePoint 2013 Site.</li>
<li>Select the InfoPath form and delete it. This operations results in an error, and the error messages says that this form cannot be deleted. And we can delete this form by deactivating a feature. The feature ID is displayed in the error message</li>
<li>Note the feature ID in the error message and run the following PowerShell command to deactivate the feature.<center><em><strong>Disable-SPFeature -Identity "Feature-ID-Noted-From-The-Error-Message" -Url "http://site-url"</strong></em></center></li>
<li>After the feature is deactivated, Open the Forms Library and make sure the form is removed from the library.</li>
</ul>
<h3>Upload the Published InfoPath from to the Central Admin Form Templates</h3>
<ul class="spd-ul">
<li>Get the published InfoPath form (XSN) file from your MOSS environment.</li>
<li>Open the <strong>Central Administration</strong> site on you SharePoint Server 2013 farm</li>
<li>From the left navigation section, click on <strong>General Application Settings</strong></li>
<li>Under the <strong>InfoPath Form Services</strong> section, click on <strong>Upload Form Template</strong></li>
<li>In this page, browse to the published infopath form file, and upload it</li>
<li>After the form is uploaded, it shows the list of Forms available in the farm.</li>
<li>From this page, select the uploaded form, open the <strong>Item Menu</strong> for this form and click on <strong>Activate to Site Collection</strong>.</li>
<li>Choose the appropriate Site Collection and activate this form</li>
</ul>
<p>Now browse to the site and open the Form Templates library. You should be able to see the published InfoPath form and it should work as it was working in MOSS environment.</p>
