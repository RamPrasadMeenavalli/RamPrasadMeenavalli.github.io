---
layout: post
title: "Querying a list from another site using PnPjs in SPFx webparts"
date: 2020-06-03 20:40:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "PnPjs", "SPFx"]
permalink: ["/post/querying-a-list-from-another-site-using-pnpjs-in-spfx-webparts"]
  ---
<!-- more -->
<p>While most of the SPFx webparts or extensions interact with the current site for querying a list or creating list items etc., there could be scenarios where a custom SPFx webpart or an application customizer might have to query a list from another site collection or a sub web of the current site collection.</p>
<p>One common scenario could be a Announcements webpart, which queries the list from a root site collection (or any other site) to show the announcements across multiple sites.</p>
<p>Here is how you can <strong>query a list from another site / site collection / sub site using PnPjs</strong>.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">import { sp, Web } from '@pnp/sp';
.
.
// Setup the context
sp.setup({
 spfxContext: this.context
});
.
.
//Get the absolute url of other site
const webUrl = `${window.location.protocol}//${window.location.hostname}/sites/intranet`;
const _web = new Web(webUrl);
var items = await _web.lists
        .getByTitle("Alerts")
        .items.get()
.
.</pre>
<p>The _web constant on the above snippet can be used to perform any other operation like finding all lists in another site, creating items in another site or any other REST operation using PnPjs. The user should have necessary permissions on other site / site collection to perform these operations.</p>
<p>Using the same PnPjs context within your SPFx component, you can perform your REST operations on the current web using sp.web object and on other site using the _web object.</p>
