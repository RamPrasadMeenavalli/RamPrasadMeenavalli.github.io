---
layout: post
title: "PropertyOrFieldNotInitializedException when using .NET Client Object Model for SharePoint Server 2013"
date: 2015-09-02 09:21:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "SharePoint Server", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/SharePoint-PropertyOrFieldNotInitializedException-CSOM-ClientContext", "/post/sharepoint-propertyorfieldnotinitializedexception-csom-clientcontext"]
  ---
<!-- more -->
<p>Recently when I was trying to do develop some business logic which involved reading data from SharePoint lists, I thought of writing a Console Application and use the .NET Client Side Object Model (CSOM) to perform the same. I started with a simple code to print the title of a web as shown below.</p>
<pre class="brush:csharp;auto-links:false;toolbar:false" contenteditable="false">            ClientContext context = new ClientContext("http://my-sharepoint-site-url");
            context.Credentials = System.Net.CredentialCache.DefaultNetworkCredentials;
            Web web = context.Web;
            context.Load(web);
            context.ExecuteQuery();
            Console.WriteLine(web.Title);</pre>
<p>But this returned an error</p>
<p style="color: red;">Unhandled Exception: System.Net.WebException: The remote server returned an error: (503) Server Unavailable. at System.Net.HttpWebRequest.GetResponse() at Microsoft.SharePoint.Client.SPWebRequestExecutor.Execute() at Microsoft.SharePoint.Client.ClientContext.GetFormDigestInfoPrivate() at Microsoft.SharePoint.Client.ClientContext.EnsureFormDigest() at Microsoft.SharePoint.Client.ClientContext.ExecuteQuery()</p>
<p>By looking at the error message, I thought the site is not accessible. But when I tried to browse the site through the browser, it was working fine. I was able to open the pages, lists, libraries and do every other operation.<br /> And then I started debugging the code, and found that the <em>context</em> object is not getting instantiated. I could see the below error for the properties of the context object.</p>
<center><em>'context.ServerVersion' threw an exception of type 'Microsoft.SharePoint.Client.PropertyOrFieldNotInitializedException'</em></center>
<h2>Things to troubleshoot</h2>
<ul class="spd-ul">
<li>Make sure you are using right .NET version in the project properties</li>
<li>The target CPU type should be set to 'Any CPU' or 'x64'</li>
<li>If your SharePoint site is having FBA or Custom Identity Provider configured, Pass the appropriate credentials in the code.</li>
<li>Verify if the client service (/_vti_bin/client.svc) is working fine and response is returned. Even though the site is browsable, the client.svc might not work and return 503 which was the issue in my case.</li>
</ul>
<p>By verifying these things, I was able to read the details using Client Object model on a SharePoint Server 2013 on-premises site which is configured to have Integrated Windows Authentication. Hope this helps.</p>
