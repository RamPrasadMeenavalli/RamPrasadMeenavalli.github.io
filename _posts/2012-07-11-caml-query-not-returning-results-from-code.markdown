---
layout: post
title: "Using CAML query for SPQuery in Object Model"
date: 2012-07-11 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2010", "SharePoint 2013", "SharePoint 2016"]
permalink: ["/post/caml-query-not-returning-results-from-code"]
  ---
<!-- more -->
<p>CAML queries are often used to query list items from a sharepoint list. There are many tools which are used to generate CAML queries which can be used in the Object Model (like webparts, controls, event receivers etc.,). Below is a sample <em>&lt;OrderBy&gt;</em> query generated from U2U CAML query builder.</p>
<pre class="brush:xml;auto-links:false;toolbar:false" contenteditable="false">&lt;Query&gt;
 &lt;OrderBy&gt;
  &lt;FieldRef Name="Title" Ascending="FALSE"/&gt;
 &lt;/OrderBy&gt;
&lt;/Query&gt;</pre>
<p>While using a CAML query in the object model, the most common mistake done is to assign this CAML query as it is to the <em>SPQuery.Query</em> property. This doesn't reutrn any results! </p>
<p>The tags '&lt;Query&gt;' and '&lt;/Query&gt;' shouldn't be used while assigning the CAML query to <em>SPQuery.Query</em> property. The query works if we remove the starting and ending &lt;Query&gt; element. Below is a sample code which used CAML query to order items by title.</p>
<pre class="brush:csharp;auto-links:false;toolbar:false" contenteditable="false">using (SPWeb spWeb = SPContext.Current.Site.AllWebs["SubSite"])
{
    SPList spList = spWeb.Lists.TryGetList("Employees");
    SPQuery spQuery = new SPQuery();

    spQuery.Query = @"&lt;OrderBy&gt;
    &lt;FieldRef Name=""Title"" Ascending=""FALSE""/&gt;
    &lt;/OrderBy&gt;";
    SPListItemCollection items = spList.GetItems(spQuery);
}</pre>
<p><strong>WHY SHOULDN'T WE ADD &lt;QUERY&gt; IN THE OBJECT MODEL</strong><br /> The SPQuery has many properties, like Query, RowLimit, ProjectedFields etc. Internally a CAML query is prepared using all these properties which will be within a &lt;Query&gt; element. So, in the object model we should only specify the actual query for the querying to work.</p>
