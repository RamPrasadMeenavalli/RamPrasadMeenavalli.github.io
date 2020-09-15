---
layout: post
title: "Adding BreadCrumb to a SharePoint Page"
date: 2014-11-28 15:57:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/adding-breadcrumb-publishing-page"]
  ---
<!-- more -->
<p>Breadcrumb navigation on landing pages of websites is very useful for navigating within the site. For SharePoint sites this can be easily achieved using the SiteMapPath control. Below is a sample BreadCrumb which I added to my SharePoint 2013 master page</p>
<p><img src="/image.axd?picture=/sp2013_brdcrmb.png" alt="" /></p>
<p>Add the below control snippet within your SharePoint master page to display the breadcrumb.</p>
<pre class="brush:xml;auto-links:false;toolbar:false" contenteditable="false">&lt;!--CS: Start Create Snippets From Custom ASP.NET Markup Snippet--&gt;
&lt;!--SPM:&lt;asp:SiteMapPath ID="SiteMapPath1"
     runat="server"
     SiteMapProviders="SPSiteMapProvider,SPXmlContentMapProvider"
     RenderCurrentNodeAsLink="false"
     NodeStyle-CssClass="breadcrumbNode"
     CurrentNodeStyle-CssClass="breadcrumbCurrentNode"
     RootNodeStyle-CssClass="breadcrumbRootNode"
     HideInteriorRootNodes="true"
     SkipLinkText=""/&gt;--&gt;
&lt;!--CE: End Create Snippets From Custom ASP.NET Markup Snippet--&gt;</pre>
<p>The UI of this breadcrumb can be completely customized using CSS. Set the appropriate CSS classes for <em>CurrentNodeStyle</em>, <em>RootNodeStyle</em>, <em>NodeStyle</em> properties of the SiteMapPath control, and define the CSS definitions for each type of the node.</p>
