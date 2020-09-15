---
layout: post
title: "Saving a SharePoint Site as Template"
date: 2012-07-02 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Online", "Classic Sites", "SharePoint Server", "SharePoint 2010", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/saving-a-sharepoint-site-as-template"]
  ---
<!-- more -->
<p>We often need to create sites within a SharePoint Portal, with the same structure of another site. So, when we are in such a situation, we first think about saving the site as template. But unfortunately, when we go to the site settings page, we dont find any link to save the site as template. In MOSS 2007, in the settings page of the list/document library, a link is provided to save it as template, but the same option is not provided for a Site. In SPS 2010, we have a link to save the site as template (its under Look and Feel section), but the same option is not available for a sub-sites.</p>
<p>So, to achieve this simple thing, should we start creating a feature with the site definition we wanted?. Not needed. Here is the solution.</p>
<p>Type the URL of the site (which is to be saved as template), and add '<em><strong> /_layouts/savetmpl.aspx</strong></em>' at the end of the URL. It opens a page from where we can save the site as a template. For example, if we want to save a site at 'http://rams/sales' as a template, then we have to type the URL as 'http://rams/sales/_layouts/savetmpl.aspx' to save it as a template</p>
<p><strong>NOTE</strong>:By saving a site as a template, we get only the structure of that site, its sub-sites are not included in the template</p>
