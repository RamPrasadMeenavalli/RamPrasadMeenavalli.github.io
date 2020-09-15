---
layout: post
title: "Video Series on Bulding an Image Search Center in SharePoint using Azure Computer Vision & Power Automate"
date: 2020-04-24 08:43:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "Power Automate", "SharePoint Online"]
permalink: ["/post/bulding-an-image-search-center-in-sharepoint-using-azure-computer-vision-and-power-automate"]
  ---
<!-- more -->
<p>Most of the organizations share the corporate images, brand logos and other media assets at a central location which can be used by employees. With increasing number of assets its difficult to find an appropriate image when needed. A Image Search Center would provide the channel where one can search for images and quickly find the relevant assets.</p>
<p><img src="/image.axd?picture=/image-search.gif" alt="" /></p>
<p>To design a modern search center, we use a SharePoint Library to store all the images and use a Power Automate flow to automatically trigger when a image is created. The flow then calls the Computer Vision API and extracts the metadata for the image, which is then updated back to the columns in the library. We then configure the SharePoint Search with custom Managed Properties and use PnP Modern Search Webparts to create the custom modern search center.</p>
<p><img src="/image.axd?picture=/Image-Search.png" alt="" /></p>
<p>Below are the videos explaining the different components of this design on who to create a modern image search center in your tenant.</p>
<ul style="list-style-type: disc;">
<li><strong>Introduction</strong> - SharePoint Image Search &ndash; Using Azure Cognitive Services &amp; Power Automate<br /><iframe src="https://www.youtube.com/embed/KoeJdmWfNP8" width="280" height="158" frameborder="0" allowfullscreen="allowfullscreen"></iframe><br /><br /></li>
<li><strong>Part 1</strong> - Using Azure Computer Vision to extract information from images<br /><iframe src="https://www.youtube.com/embed/HhK_VtcvpYQ" width="280" height="158" frameborder="0" allowfullscreen="allowfullscreen"></iframe><br /><br /></li>
<li><strong>Part 2</strong> &ndash; Creating a Power Automate Flow to update image metadata extracted from Computer Vision API<br /><iframe src="https://www.youtube.com/embed/kQ3pi10NCK4" width="280" height="158" frameborder="0" allowfullscreen="allowfullscreen"></iframe><br /><br /></li>
<li><strong>Part 3</strong> - Configure Managed Properties in SharePoint Search Schema<br /><iframe src="https://www.youtube.com/embed/5To84LJaAD8" width="280" height="158" frameborder="0" allowfullscreen="allowfullscreen"></iframe><br /><br /></li>
<li><strong>Part 4</strong> - Using PnP Modern Search Webparts to create the Search Center<br /><iframe src="https://www.youtube.com/embed/tJu6ultMni0" width="280" height="158" frameborder="0" allowfullscreen="allowfullscreen"></iframe><br /><br /></li>
</ul>
