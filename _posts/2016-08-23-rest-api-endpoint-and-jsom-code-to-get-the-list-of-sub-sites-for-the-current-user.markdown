---
layout: post
title: "REST API Endpoint and JSOM Code to get the list of sub sites for the current user"
date: 2016-08-23 09:19:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "SharePoint Server", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/rest-api-endpoint-and-jsom-code-to-get-the-list-of-sub-sites-for-the-current-user"]
  ---
<!-- more -->
<p>While working with any SharePoint site, there might be a need to display the list of sub sites to which the current user has access, using JSOM or REST APIs. One might end up writing the below code for getting the list of sub sites for the current user.</p>
<h4>JSOM</h4>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">	var ctx = SP.ClientContext.get_current();
	var parentWeb = ctx.get_web();
        var subWebs = parentWeb.get_webs();</pre>
<h4>REST API ENDPOINT</h4>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">http://&lt;site url&gt;/_api/web/webs</pre>
<p>The above code works perfectly when all the sub sites do not have unique permissions and are inheriting permissions from its parent. But if any of the sub sites are having unique permissions and if the current user do not have access to one of these sub sites, the above code will throw an error (shown below).</p>
<p style="text-align: center;"><strong> Access denied. You do not have permission to perform this action or access this resource <br />Access is denied. (Exception from HRESULT: 0x80070005 (E_ACCESSDENIED)) </strong></p>
<p>To get the list of sub sites available for the current user, one has to use the following JSOM or REST API code.</p>
<h4>JSOM</h4>
<p>Use the <em><strong>getSubwebsForCurrentUser</strong></em> method of SP.Web class to get the collection of child sites for the current user.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">	var ctx = SP.ClientContext.get_current();
	var parentWeb = ctx.get_web();
        var subWebs = parentWeb.getSubwebsForCurrentUser(null);</pre>
<p>Reference : <a href="https://msdn.microsoft.com/en-us/library/office/jj246242.aspx" target="_blank">MSDN</a></p>
<p>This works for both SharePoint Server 2013 &amp; SharePoint online.</p>
<h4>REST API ENDPOINT</h4>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">http://&lt;site url&gt;/_api/web/getsubwebsfilteredforcurrentuser(nwebtemplatefilter=-1,nconfigurationfilter=0)</pre>
<p>Reference: <a href="https://msdn.microsoft.com/en-us/library/office/dn499819.aspx#bk_WebGetSubwebsFilteredForCurrentUser" target="_blank">MSDN</a></p>
<p>At the time of writing this article, this endpoint is available only for SharePoint Online.</p>
<p>With the above snippets, we can get the list of subsites for the current user in any scenario i.e., sub sites with unique permissions and ones which are inheriting permissions from parent.</p>
