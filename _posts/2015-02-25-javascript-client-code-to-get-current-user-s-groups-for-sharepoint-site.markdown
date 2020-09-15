---
layout: post
title: "JavaScript Client Code to get current user's groups for SharePoint Site"
date: 2015-02-25 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "Classic Sites", "SharePoint Server", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/javascript-client-code-to-get-current-user-s-groups-for-sharepoint-site"]
  ---
<!-- more -->
<p>Below is a sample code snippet using JavaScript client object model, which gets the list of SharePoint groups the current user is part of.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">&lt;script type="text/javascript"&gt;
	
	var clientContext = new SP.ClientContext.get_current();
	var oWeb = clientContext.get_web();
	var currentUser = oWeb.get_currentUser();
	var allGroups = currentUser.get_groups();
	clientContext.load(allGroups);

	clientContext.executeQueryAsync(OnSuccess, OnFailure);

	function OnSuccess(sender, args)
	{
		var grpsEnumerator = allGroups.getEnumerator();
		while(grpsEnumerator.moveNext())
		{
			//This object will have the group instance
			//which the current user is part of. 
			//This is can be processed for further 
			//operations or business logic.
			var group = grpsEnumerator.get_current();

			//This gets the title of each group
			//while traversing through the enumerator.			
			var grpTitle = group.get_title();
		}
	}

	function OnFailure(sender, args)
	{
		console.log(args.get_message());
	}

&lt;/script&gt;</pre>
<p>&nbsp;</p>
