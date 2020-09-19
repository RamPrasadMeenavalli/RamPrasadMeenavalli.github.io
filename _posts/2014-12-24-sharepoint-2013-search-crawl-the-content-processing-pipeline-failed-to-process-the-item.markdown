---
layout: post
title: "SharePoint 2013 Search Crawl - The content processing pipeline failed to process the item"
date: 2014-12-24 16:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2013"]
permalink: /post/sharepoint-2013-search-crawl-the-content-processing-pipeline-failed-to-process-the-item
---
<!-- more -->
<p>Recently I observed the following error in our SharePoint Farm's crawl log for multiple items in the site collections.</p>
<p><strong><em>The content processing pipeline failed to process the item. ( Index was out of range. Must be non-negative and less than the size of the collection. Parameter name: index; ; SearchID = XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXX)</em></strong></p>
<h3>Root Cause</h3>
<p>After some troubleshooting I found that this crawl error occurs due to some erroneous settings of Managed Properties in your Search Schema and the Managed Metadata Site Columns. When the setting for &lsquo;Allow Multiple Values&rsquo; is different between a Managed Metadata Site Column and its appropriate Managed Property/ies, this error occurs and the SharePoint items will not get crawled.</p>
<h3>Solution</h3>
<p>Make sure the value for &lsquo;Allow Multiple Values&rsquo; is set as same for the Managed Metadata Site Column and the Managed Property (which is having the Crawled property of the site column added). Both should be either set to Yes/No.</p>
<p>Make these changes and run a full crawl. For some the issue might be solved, but if the issue is still not resolved, check the ULS logs and you might find an error message as shown below</p>
<p><strong><em>[Microsoft.CrawlerFlow-XXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX] Microsoft.Ceres.ContentEngine.Processing.BuiltIn.AttributeMapperEvaluator+AttributeMapperProducer : Failed to map values to field ExcludeFromSummary</em></strong></p>
<p>If you see this in the log, then your &lsquo;ExcludeFromSummary&rsquo; Managed Property is having an erroneous setting. Even though your site column has &lsquo;Allow Multiple Values&rsquo; set to Yes, the &lsquo;ExcludeFromSummary&rsquo; managed property should have &lsquo;Allow Multiple Values&rsquo; set to NO.</p>
<p><img src="/assets/images/crwlissue_mp.png" alt="" /></p>
<p>Make sure the &lsquo;ExcludeFromSummary&rsquo; Managed Property has the correct settings as shown above and run a full crawl to solve this error. Now all your items in the Site Collection/s should be crawled successfully.</p>
