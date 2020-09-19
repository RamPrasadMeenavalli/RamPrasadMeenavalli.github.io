---
layout: post
title: "Referencing JS Files using Custom Action element in SharePoint"
date: 2012-11-19 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2010", "SharePoint 2013", "SharePoint 2016"]
permalink: /post/referencing-js-files-using-custom-action-element-in-sharepoint
---
<!-- more -->
<p style="text-align: justify;">JavaScript plays an important role in a SharePoint web application. So, as we customize a SharePoint site, we might across a situation where we need to add our custom JS files to SharePoint pages. There are number of ways to refer a JS file on a SharePoint Page.</p>
<p style="text-align: justify;"><strong>Adding a Reference in the Master Page</strong>: Add a &lt;script&gt; tag on your master page which points to your custom master page. But in this method, we need to create a custom master page just for referencing the custom JS files.</p>
<p style="text-align: justify;"><strong>Using Delegate Control through Feature</strong>: We can also add a control in the delegate control section through a feature. But for this we need to create a Server control or a User Control to refer the JS file.</p>
<p style="text-align: justify;"><strong>Using <em>ScriptSrc</em> and <em>ScriptBlock</em> of <em>CustomAction</em></strong>: The above two methods are widely used in SharePoint 2007. SharePoint 2010 introduced two new attributes for <em>CustomAction</em> element. Using these attributes we can refer a JS file easily. Below is an sample CustomAction tag which is to be used in the elements.xml file of a feature</p>
<pre class="brush:xml;auto-links:false;toolbar:false" contenteditable="false">  &lt;CustomAction
    ScriptSrc="path of your JS file"
    Location="ScriptLink"
    Sequence="100"&gt;
  &lt;/CustomAction&gt;</pre>
<p>Using this approach we can add the JS files from <em>_layouts</em> directory, a <em>library</em> in a SharePoint site or write the script inline.</p>
<p><strong>Referring JS file from _layouts directory</strong>: The below example adds a JS file named 'spdutils.js' from the 1033 folder in _layouts directory</p>
<pre class="brush:xml;auto-links:false;toolbar:false" contenteditable="false">  &lt;CustomAction
    ScriptSrc="1033/spdutils.js"
    Location="ScriptLink"
    Sequence="100"&gt;
  &lt;/CustomAction&gt;</pre>
<p><strong>Referring JS File from a SharePoint Library</strong>: The below example adds a JS file from 'Documents' library (Root Site's library) of the SharePoint site</p>
<pre class="brush:xml;auto-links:false;toolbar:false" contenteditable="false">  &lt;CustomAction
    ScriptSrc="~SiteCollection/Documents/spdutils.js"
    Location="ScriptLink"
    Sequence="100"&gt;
  &lt;/CustomAction&gt;</pre>
<p><strong>Adding the JS code directly in the feature</strong>: This shows how to add the JS code directly in the CustomAction element using SrciptBlock attribute</p>
<pre class="brush:xml;auto-links:false;toolbar:false" contenteditable="false">  &lt;CustomAction
    Location="ScriptLink"
    ScriptBlock="
	function JSFX_StartEffects()
	{
		JSFX.Fire(200, 100, 100);
	}
	var windowOnload=window.onload||function(){};window.onload=function(){JSFX_StartEffects();};"
    Sequence="103"&gt;
  &lt;/CustomAction&gt;</pre>
<p>With the addition of ScriptSrc and ScriptBlock elements in CustomAction element, we can easily refer JS files on a SharePoint site. And as this has to be done through a feature, it is more manageable and easy to control.</p>
