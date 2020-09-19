---
layout: post
title: "Url Properties of SPSite and SPWeb"
date: 2012-12-14 11:51:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2010", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: /post/values-for-spsite-spweb-url-properties
---
<!-- more -->
<p>There are some properties for <em>SPSite</em> and <em>SPWeb</em> class using which we can get the URL values for the sites and webs. The two most used of these properties are - '<em>ServerRelativeUrl</em>' and '<em>Url</em>'</p>
<p>As a developer we use the values of the properties for writing some logic for satisfying the business needs. Whenever I used to write a OM code to use these properties, the first question which flashes in my mind - "<em>What values are returned by these properties?</em>". I immediately create a console application, and check the values returned by these properties and continue with the actual work. I have seen many other developers doing this. So, for a quick reference I prepared the below table which has the url values returned by these properties.</p>
<p>Some people may find this easy, but some (like me ;)) get confused when the sites have managed paths. These properties return the managed path if the site has one. Here goes the list</p>
<p><strong>SCENARIO 1:</strong> A Site Collection (SPSite) without any managed path.</p>
<table class="spd-table">
<thead>
<tr>
<th colspan="2"><strong>URL : 'http://rams'</strong></th>
</tr>
<tr>
<th>Property</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr>
<td>SPSite.ServerRelativeUrl</td>
<td>/</td>
</tr>
<tr>
<td>SPSite.Url</td>
<td>http://rams</td>
</tr>
<tr>
<td>SPWeb.ServerRelativeUrl</td>
<td>/</td>
</tr>
<tr>
<td>SPWeb.Url</td>
<td>http://rams</td>
</tr>
<tr>
<th colspan="2"><strong>Url - http://rams/about ('about' is a sub site)</strong></th>
</tr>
<tr>
<td>SPSite.ServerRelativeUrl</td>
<td>/</td>
</tr>
<tr>
<td>SPSite.Url</td>
<td>http://rams</td>
</tr>
<tr>
<td>SPWeb.ServerRelativeUrl</td>
<td>/about</td>
</tr>
<tr>
<td>SPWeb.Url</td>
<td>http://rams/about</td>
</tr>
</tbody>
</table>
<p>This looks pretty simple. We get confused when sites are created using managed paths. Below are the samples values for site created with managed paths</p>
<div class="gad" style="height: 15px; min-width: 468px;">&nbsp;</div>
<p><strong>SCENARIO 2:</strong> A Site Collection with a managed path (of type Explicit inclusion). For the below URL '/my' is the managed path</p>
<table class="spd-table">
<thead>
<tr>
<th colspan="2"><strong>URL : 'http://rams/my'</strong></th>
</tr>
<tr>
<th>Property</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr>
<td>SPSite.ServerRelativeUrl</td>
<td>/my</td>
</tr>
<tr>
<td>SPSite.Url</td>
<td>http://rams/my</td>
</tr>
<tr>
<td>SPWeb.ServerRelativeUrl</td>
<td>/my</td>
</tr>
<tr>
<td>SPWeb.Url</td>
<td>http://rams/my</td>
</tr>
<tr>
<th colspan="2"><strong>Url - http://rams/my/about ('about' is a sub site)</strong></th>
</tr>
<tr>
<td>SPSite.ServerRelativeUrl</td>
<td>/my</td>
</tr>
<tr>
<td>SPSite.Url</td>
<td>http://rams/my</td>
</tr>
<tr>
<td>SPWeb.ServerRelativeUrl</td>
<td>/my/about</td>
</tr>
<tr>
<td>SPWeb.Url</td>
<td>http://rams/my/about</td>
</tr>
</tbody>
</table>
<p>&nbsp;</p>
<p><strong>SCENARIO 3:</strong> A Site Collection with a managed path (of type Wildard inclusion). For the below URL '/sites' is the managed path, and a site collection is created at '/blog'</p>
<table class="spd-table">
<thead>
<tr>
<th colspan="2"><strong>URL : 'http://rams/sites/blog'</strong></th>
</tr>
<tr>
<th>Property</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr>
<td>SPSite.ServerRelativeUrl</td>
<td>/sites/blog</td>
</tr>
<tr>
<td>SPSite.Url</td>
<td>http://rams/sites/blog</td>
</tr>
<tr>
<td>SPWeb.ServerRelativeUrl</td>
<td>/sites/blog</td>
</tr>
<tr>
<td>SPWeb.Url</td>
<td>http://rams/sites/blog</td>
</tr>
<tr>
<th colspan="2"><strong>Url - http://rams/sites/blog/about ('about' is a sub site)</strong></th>
</tr>
<tr>
<td>SPSite.ServerRelativeUrl</td>
<td>/sites/blog</td>
</tr>
<tr>
<td>SPSite.Url</td>
<td>http://rams/sites/blog</td>
</tr>
<tr>
<td>SPWeb.ServerRelativeUrl</td>
<td>/sites/blog/about</td>
</tr>
<tr>
<td>SPWeb.Url</td>
<td>http://rams/sites/blog/about</td>
</tr>
</tbody>
</table>
