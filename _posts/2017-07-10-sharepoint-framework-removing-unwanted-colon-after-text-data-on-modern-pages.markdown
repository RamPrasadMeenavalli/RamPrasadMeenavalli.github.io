---
layout: post
title: "SharePoint Framework : Removing unwanted colon after text/data on Modern Pages"
date: 2017-07-10 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "SharePoint Server", "SharePoint 2016", "SharePoint 2019", "SPFx"]
permalink: /post/sharepoint-framework-removing-unwanted-colon-after-text-data-on-modern-pages
---
<!-- more -->
<p>A Client webpart from a SharePoint framework solution displays unwanted colons (:) after the data. The colons appear only when the SPFx client webpart is added on a Modern page, and the colon is not displayed when the webpart is accessed on a local/SharePoint Online workbench.</p>
<p><img src="/assets/images/index.png" alt="" /></p>
<p>This unwanted colon appears only for text with a &lt;label&gt; element. It comes from the CSS class which gets added on the SharePoint Modern page and not on workbench. This class is responsible for adding the extra colon after the text.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">Webpart TypeScript Code
public render(): void {
	this.domElement.innerHTML = `&lt;label&gt;Good Morning&lt;/label&gt; 
	${this.context.pageContext.user.displayName}`;
}

CSS
label::after {

content: ":";
}</pre>
<h2>Solution</h2>
<p>This unwanted colon can be removed by adding custom class to our &lt;label&gt; elements and overriding the CSS. Below is an example</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">Webpart TypeScript Code
public render(): void {
	this.domElement.innerHTML = `&lt;label class='myCustomLabel'&gt;
	Good Morning&lt;/label&gt;
	${this.context.pageContext.user.displayName}`;
}

CSS
label.myCustomLabel::after {
    
content: "" !important;

}</pre>
<p>&nbsp;</p>
