---
layout: post
title: "SharePoint 2013: Showing Page Views within a SharePoint Page"
date: 2014-10-14 15:47:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/page-views-hit-counter-on-a-sharepoint-page"]
  ---
<!-- more -->
<h2>Description</h2>
<p>Showing the Page Views within a SharePoint page is a commonly expected feature by many of my clients. It looks good and useful to show the number of times a news/event page is viewed in one&rsquo;s corporate intranet. In SharePoint 2010, this was achieved by reading the Request Usage tables from the WSS Logging database and the consolidated count for each page/item is processed and stored within an application (mostly a custom SQL table). But the analytics in SharePoint 2013 makes this easy as they store the consolidated view count within the Search Service Application. This article describes how to show the 'Views Count' for a page using JavaScript client object model.</p>
<h2>The Blueprint</h2>
<p>So, how are we going to design this?<br /> Let&rsquo;s make use of the SharePoint 2013 Search Infrastructure to achieve this. First, we have to fire a query on the Search Service so that only the current page (or the page in question) is returned in the result. Now, from the properties of this result which is returned by search service, there will be a property named &lsquo;ViewsLifeTime&rsquo;, which gives the life time view count of a page. For querying, use either the page &lsquo;url&rsquo; or the &lsquo;guid&rsquo; to get the search result of the current page.</p>
<h2>The Building Blocks</h2>
<p>In this article I will be using the GUID of the page for querying the search index. <em>Note: We can also use the URL instead of GUID, but I prefer GUID for the following reasons</em></p>
<ul class="spd-ul">
<li><em>If we change the URL, our search query may not return results until the next crawl is done.</em></li>
<li><em>If we have Minimal Download Strategy feature enabled for a site, the page URL (which is shown in the browser address bar) may not be same as the URL stored in the search index. We will end up writing JSOM code to read the current page&rsquo;s URL.</em></li>
</ul>
<h3>Create a Managed Property</h3>
<p>We need a managed property to query based on the page's GUID. Within the Search Schema of your SharePoint Farm, create a managed property with the following details.</p>
<table class="spd-table">
<tbody>
<tr>
<td>Property Name</td>
<td>PageGuid</td>
</tr>
<tr>
<td>Type</td>
<td>Text</td>
</tr>
<tr>
<td>Queryable</td>
<td>Yes (Checked)</td>
</tr>
<tr>
<td>Complete Matching</td>
<td>Yes (Checked)</td>
</tr>
<tr>
<td>Mappings to crawled properties</td>
<td>Add mapping to 'ows_UniqueID'</td>
</tr>
</tbody>
</table>
<p>After creating this Managed Property, start a full crawl of the appropriate content source/s. For SharePoint Online, set the appropriate Site Collections to be re-indexed</p>
<h3>Get the GUID of the current page</h3>
<p>There is a JavaScript variable called &ndash; <em>'_spPageContextInfo'</em> available within a SharePoint page using which we can get the GUID of the current page. Below is a JS CSOM snippet to fetch the GUID</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">var context;
    var web;
    var list;
    var currentItem;
 
$(document).ready(function () { 
  context = new SP.ClientContext.get_current();
    web = parentContext.get_web();
    list = web.get_lists().getById(_spPageContextInfo.pageListId);
    currentItem = list.getItemById(_spPageContextInfo.pageItemId)
    context.load(currentItem);
    context.executeQueryAsync(onQuerySucceeded, onQueryFailed);
});
     
function onQuerySucceeded() {
    var guid = currentItem.get_fieldValues("UniqueId").UniqueId.toString();
}
 
function onQueryFailed(sender, args) {
    //Error Logging
}</pre>
<h3>Query for the Page</h3>
<p>Using the JS CSOM, fire a query with this GUID to get the page details.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">function onQuerySucceeded() {
    var keywordQuery = new Microsoft.SharePoint.Client.Search.Query.KeywordQuery(context);
    keywordQuery.set_queryText('PageGuid:"{' + currentItem.get_fieldValues("UniqueId").UniqueId.toString() + '}"');
    var searchExecutor = new Microsoft.SharePoint.Client.Search.Query.SearchExecutor(context);
    results = searchExecutor.executeQuery(keywordQuery);
    context.executeQueryAsync(SearchDone, SearchFailed)
}
 
function SearchDone() {
    var viewCount = results.m_value.ResultTables[0].ResultRows[0].ViewsLifeTime;
   //Use JQuery to show the viewCount on the page
}
 
function SearchFailed(sender, args) {
    //Error Logging
}</pre>
<h2>The Build</h2>
<p>All the blocks explained above can be merged as a working code and can be added to a page through a Script Editor Web Part, or can be added directly into a page layout which reflects in all the pages created using this layout. Also, reference to the following files needs to be added</p>
<ul class="spd-ul">
<li>init.js</li>
<li>sp.runtime.js</li>
<li>sp.js</li>
<li>sp.search.js</li>
</ul>
<h2>Conclusion</h2>
<p>By using the JS COM, we can assemble the blocks defined above and show the page views for a page. Thank you SharePoint 2013 for making it so easy :)</p>
