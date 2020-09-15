---
layout: post
title: "Adding Custom Font Styles to SharePoint Server Rich Text Editors"
date: 2014-10-27 15:53:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Online", "Classic Sites", "SharePoint Server", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/adding-custom-font-styles-to-sharepoint-server-rich-text-editors"]
  ---
<!-- more -->
<h3>Description</h3>
<p style="text-align: justify;">The rich text editors (RTE) in SharePoint Server 2013 shows various editing options using which a web page can be formatted. You can select the font, size, color, alignment etc., while publishing content through a rich text editor. However, various companies have their corporate fonts to be used in their websites. In such cases we cannot restrict the authors/users to use the default fonts. We have to add the custom font/s in the rich text editor options.</p>
<h3>How It Works?</h3>
<p style="text-align: justify;">The RTEs in SharePoint sites reads the definitions for all the styles (shown in the ribbon) from a CSS file. By default, the RTEs look for style definitions with the prefix <em>'.ms-rte'</em>. Using this same naming convention we can define new/custom fonts, font-size, text-alignment etc., to be added to the RTEs.</p>
<h3>How To Add A Font?</h3>
<p>Create a custom CSS file with the following style definition</p>
<pre class="brush:css;auto-links:false;toolbar:false" contenteditable="false">.ms-rteFontFace-custom1
{
	-ms-name:"My Own Font";
	font-family:"Custom Font Face Name";
}</pre>
<p><img src="/image.axd?picture=/sp2013_rte.png" alt="" /></p>
<p style="text-align: justify;">If you look at the style definition, the attribute '-ms-name' tells the RTE that this is the display name for the font (which comes in the font drop down). And the 'font-family' is the traditional CSS attribute, which applies this font family to the selected object. All other components can be added using style definitions similar to the one described above. Following are the available CSS style definitions which we can use to add custom font-styles or element-styles to the RTEs.</p>
<ul class="spd-ul">
<li>.ms-rtestyle</li>
<li>.ms-rteelement</li>
<li>.ms-rtefontsize</li>
<li>.ms-rteforecolor</li>
<li>.ms-rtebackcolor</li>
<li>.ms-rteimage</li>
<li>.ms-rtetable</li>
<li>.ms-rteposition</li>
</ul>
<p style="text-align: justify;">Save the custom CSS file at a location accessible to the SharePoint sites. Add a reference to this CSS file on the SharePoint pages where ever you need the custom styles/fonts for the RTEs. It's ideal to add this CSS reference to the SharePoint master page/s so that it's applied to all the pages in the site/s.<br /> <em>Note: This approach doesn't include the custom fonts in the SharePoint site. Any custom font which is not part of the client's system fonts, should be included in the SharePoint site using the <a href="http://www.w3schools.com/cssref/css3_pr_font-face_rule.asp">@font-face</a> CSS rule.</em></p>
