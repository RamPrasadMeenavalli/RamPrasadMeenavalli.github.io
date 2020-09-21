---
layout: post
title: "Showing the correct DateTime value from UTC value in an SPFx webpart"
date: 2020-07-31 00:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "PnPjs", "SPFx"]
permalink: /post/spfx-webpart-convert-datatime-value-time-zone-with-pnpjs
---
<!-- more -->

<p>When we fetch the values of any DateTime fields within a SharePoint list or library, the values are returned in UTC format, as they are stored in UTC format internally. When this values is shown with a SharePoint view or any default page, the UTC format is then converted to the correct Time Zone based on the User's &amp; Site's Regional Settings. But within an SPFx webpart it just displays the UTC time.</p>
<p><img src="/assets/images/spfx-webpart-date-format.png" alt="" /></p>
<p>Within an SPFx webpart, after querying the list, if we want to find the correct time zone to display the DateTime value, we will have to make at least 2 REST calls to find the User's regional settings and the Site's regional settings and then convert the UTC value to the correct time zone.</p>
<p>We can use the '<strong>FieldValuesAsText</strong>' property of the list items, to avoid making these extra REST calls and find the correct display value of the DateTime field. Below is the sample code snippet to expand this property and get the correct Start and End Date values from the list.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">import { sp } from '@pnp/sp';
import '@pnp/sp/webs';
import '@pnp/sp/lists';
import '@pnp/sp/items';

const listName = "Events";
let items = await sp.web.lists.getByTitle(listName)
.items
.select("Title","Description","StartDate","EndDate","FieldValuesAsText/StartDate","FieldValuesAsText/EndDate")
.expand("FieldValuesAsText")
.get();

console.dir(items);

</pre>
<p><img src="/assets/images/spfx-webpart-date-format-1.png" alt="" /></p>
<p>Check out this Video to understand how the Regional Settings effect the DateTime Field values in SharePoint sites.</p>
<div class="ytEmbed"><iframe src="https://www.youtube.com/embed/OGytQVwidyU" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen"></iframe></div>
