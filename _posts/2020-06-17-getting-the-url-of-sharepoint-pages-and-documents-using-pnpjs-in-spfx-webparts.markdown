---
layout: post
title: "Getting the URL of SharePoint Pages and Documents using PnPjs in SPFx webparts"
date: 2020-06-17 09:06:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "PnPjs", "SPFx"]
permalink: ["/post/getting-the-url-of-sharepoint-pages-and-documents-using-pnpjs-in-spfx-webparts"]
  ---
<!-- more -->
<p>While developing some SPFx webparts or extensions, we might need to find the full URLs of the SharePoint items we read. For example, if we are developing a custom Image Gallery, the SPFx webpart should read all the items from a Image library and also need the full URLs of the images. Similarly when we are showing the list if pages or documents, we might need the full URLs for the pages and documents.</p>
<h2>FileRef and EncodedAbsUrl</h2>
<p>SharePoint provides 2 internal fields which we can use to find the direct URL for a given resource (page, document, image etc.,)</p>
<p>The <strong>FileRef</strong>, provides a relative url of the resource, while <strong>EncodedAbsUrl</strong> provides the full absolute URL for the resource.</p>
<p>Below are the code snippets to read the URLs for pages, documents and images using the PnPjs in your SPFx solutions</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">import { sp } from "@pnp/sp";
import '@pnp/sp/webs';
import '@pnp/sp/items';
.
.
.
// Pages
const pages = await sp.web.lists.getByTitle("Site Pages").items.select("Title, FileRef, EncodedAbsUrl").get()
console.dir(pages);
.
.
//Documents
const docs = await sp.web.lists.getByTitle("Documents").items.select("Title, FileRef, EncodedAbsUrl").get()
console.dir(docs);

//Images
const images = await sp.web.lists.getByTitle("Site Assets").items.select("Title, FileRef, EncodedAbsUrl").get()
console.dir(images);
</pre>
<p>Here is the sample JSON response from these queries.</p>
<p><img src="/image.axd?picture=/FileRef-Example.png" alt="" /></p>
