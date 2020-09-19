---
layout: post
title: "Setting Different Colors for Custom Status Messages in SharePoint 2010"
date: 2011-02-21 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2010", "SharePoint 2013"]
permalink: /post/custom-status-messages-with-different-colors
---
<!-- more -->
<p>Previous <a href="http://spdeveloper.co.in/articles/pages/notificationsnstatus.aspx">article</a> explained how to create custom status messages in SharePoint 2010. As mentioned in that article, we are allowed to set the colors for custom status messages using the <em>SP.UI.Status.setStatusPriColor()</em> method. But SharePoint supports only four colors by default. In this article we will see how to set different colors (other than the default ones) for status messages.</p>
<p>The four colors which are supported by SharePoint (ie., Red, Yellow, Green and Blue) are set to the status messages through CSS. If you open the core4.css file we can find the following classes.</p>
<ul class="spd-ul">
<li>.s4-status-s1</li>
<li>.s4-status-s2</li>
<li>.s4-status-s3</li>
<li>.s4-status-s4</li>
</ul>
<p>The definition for these classes look like</p>
<pre class="brush:css;auto-links:false;toolbar:false" contenteditable="false">.s4-status-s1{
background:#c9d7e6 url("/_layouts/images/bgximg.png") repeat-x -0px -209px;
color:#3b4652;
border-color:#aaafbe;
}</pre>
<p>By seeing the definition of this class, we can understand that the background of the status message is set using an image (see the background attribute in the CSS class definition). And if the image is not available, then the background is set to the specified color.</p>
<p>To change the color of status messages, we can override, the definition of the class and define the values of background and border-color attributes to the desired colors. An example is shown below</p>
<pre class="brush:css;auto-links:false;toolbar:false" contenteditable="false">.s4-status-s1{
background:Lime;
color:Green;
border-color:Black;
}</pre>
<p>So, by overriding the definitions for these CSS classes we can set different colors to Status Messages. But we can define only four different colors to be set, which is the OOB behavior of SharePoint 2010.</p>
