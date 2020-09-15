---
layout: post
title: "C# .NET Client API code snippet to search/query for people in SharePoint Server 2013/2016"
date: 2017-01-19 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/c-net-client-api-code-snippet-to-search-query-for-people-in-sharepoint-server-2013-2016"]
  ---
<!-- more -->
<p><strong> In this post I show how to query/search for people within a .Net application (windows application or a console application) using the .Net Client API for SharePoint Server.</strong></p>
<p>Though we have REST API to search for people and Client People picker control (JS based), it is difficult or impossible to use these within a windows application. Also, there might be scenarios where User Profile Service is not available within your farm, and you have to query for people within your client application. For example, let's say you might want to assign a task to an SP user. In such scenarios, we can use the <strong><em>'ClientPeoplePickerWebServiceInterface'</em></strong> and <strong><em>'ClientPeoplePickerQueryParameters'</em></strong> classes from <em><strong>Microsoft.SharePoint.ApplicationPages.ClientPickerQuery namespace.</strong></em> These are the same ones used by the out of the box people picker. So, the results would be identical. These classes are available within the 'Microsoft.SharePoint.Client.dll' assembly.</p>
<p>Below is the code snippet to query for people or groups.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">String strSearchPhrase = "Ram Meenavalli"

ClientContext spCtx;
spCtx = new ClientContext("http://ramspc/sites/teamsite");

ClientPeoplePickerQueryParameters query = new ClientPeoplePickerQueryParameters();
query.AllowEmailAddresses = true;
query.MaximumEntitySuggestions = 30;
query.PrincipalType = PrincipalType.SecurityGroup | PrincipalType.User;
query.PrincipalSource = PrincipalSource.All;
query.QueryString = strSearchPhrase;
query.WebApplicationID = new Guid("00000000-0000-0000-0000-000000000000");
ClientResult&lt;String&gt; result = ClientPeoplePickerWebServiceInterface.ClientPeoplePickerSearchUser(spCtx, query);
spCtx.ExecuteQuery();

Console.Write(result.Value);</pre>
<p>The result.Value will give the results in JSON format, which will have all the required properties for each user. Sample format is shown below</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">[

{"Key" : "XX\\XXXXXXXX", "Description" : "XXXXXXXXX", "DisplayText" : "XXXXXXXXXXX", "EntityType" : "User",
"ProviderDisplayName" : "Active Directory", "ProviderName" : "AD", "IsResolved" : true, 
"EntityData" : {"Title" : "", "MobilePhone" : "", "SIPAddress" : "XXXXXXXXXXXXX", "Department" : "XXXXXXXXXX", 
"Email" : "XXXXXXXXXXXXX"}, "MultipleMatches" : []}, 

{"Key" : "XXXXXXXX", "Description" : "XXXXXXXXXX", "DisplayText" : "XXXXXXXXXXXX", "EntityType" : "User", 
"ProviderDisplayName" : "Active Directory", "ProviderName" : "AD", "IsResolved" : true, 
"EntityData" : {"Title" : "", "MobilePhone" : "", "SIPAddress" : "", 
"Department" : "XXXXXXXXXXXX", "Email" : ""}, "MultipleMatches" : []}

]</pre>
<p>There are many utility classes available using which the JSON can be de-serialized and access the required properties of a user or security group.</p>
