---
layout: post
title: "How to Provision Subscription Settings Service Application and Proxy for SharePoint Server 2013"
date: 2015-09-03 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/how-to-provision-subscription-settings-service-application-and-proxy-for-sharepoint-server-20131"]
  ---
<!-- more -->
<p style="text-align: justify;">Subscription Settings Service Application and the proxy are two of the prerequisites for developing SharePoint Add-Ins (SharePoint Apps). But this service application cannot be provisioned through the UI. A Subscription Settings Service Application can be created only through PowerShell and below is the script to provision the Service Application and its proxy.</p>
<p style="text-align: justify;">Change the Display Names and Account details as per your environment in the below script and run it through SharePoint Server 2013 Management Shell (opened as a Farm Administrator)</p>
<pre class="brush:ps;auto-links:false;toolbar:false" contenteditable="false">	Update these values as per your environment
	$login = Get-SPManagedAccount RAMS\ram-svc
	$AppPoolName = "SubscriptionServiceAppPool"
	$SAName = "Subscription Settings Service Application"
	$DBName = "SubscriptionSettingsDataBase"


	#Create a New Application Pool
	$appPool = New-SPServiceApplicationPool -Name $AppPoolName 
	-Account $login

	#Provision a New Service Application
	$serviceApp = New-SPSubscriptionSettingsServiceApplication 
	-ApplicationPool $appPool -name $SAName -DatabaseName $DBName 

	#Create a Proxy
	$serviceAppProxy = New-SPSubscriptionSettingsServiceApplicationProxy 
	-ServiceApplication $serviceApp</pre>
<p style="text-align: justify;">&nbsp;</p>
