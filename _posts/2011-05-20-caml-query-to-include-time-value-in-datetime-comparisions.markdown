---
layout: post
title: "CAML Query to Include Time Value in DateTime comparisions"
date: 2011-05-20 10:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Online", "SharePoint Server", "SharePoint 2007", "SharePoint 2010", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: /post/caml-query-to-include-time-value-in-datetime-comparisions
---
<!-- more -->
<p>The <em>Collaborative Application Markup Language (CAML)</em> query is an XML based language used for filtering and/or sorting data from a list within a SharePoint portal . This is often used in custom webparts and controls while retrieving data.</p>
<p>Let's say, we need to read all the items from a list between some specified dates. We end up preparing a CAML query which looks like</p>
<pre class="brush:xml;auto-links:false;toolbar:false" contenteditable="false">&lt;Query&gt;
   &lt;Where&gt;
      &lt;And&gt;
         &lt;Gt&gt;
            &lt;FieldRef Name='Modified' /&gt;
            &lt;Value Type='DateTime'&gt;2011-02-09T12:00:00Z&lt;/Value&gt;
         &lt;/Gt&gt;
         &lt;Lt&gt;
            &lt;FieldRef Name='Modified' /&gt;
            &lt;Value Type='DateTime'&gt;2011-02-10T10:00:00Z&lt;/Value&gt;
         &lt;/Lt&gt;
      &lt;/And&gt;
   &lt;/Where&gt;
&lt;/Query&gt;</pre>
<p>On seeing this query, we understand that, it will fetch all the items from the list which are modified between Feb-09<sup>th</sup> 12:00 and Feb-10<sup>th</sup> 10:00. But when we exceute this query, no results are returned (even though items are modified between the specified dates). This happens because, by default the timestamp is not considered while comparing the dates. So, in this case both the comparision operators (&lt;Gt&gt; &amp; &lt;Lt&gt;) will always fail, and hence no results are returned for this query.</p>
<p>In such scenarios, we need to use the <em><strong>IncludeTimeValue</strong></em> attribute for the <em>Value</em> element. Add this attribute and assign it true, as shown in the below XML</p>
<pre class="brush:xml;auto-links:false;toolbar:false" contenteditable="false">&lt;Query&gt;
   &lt;Where&gt;
      &lt;And&gt;
         &lt;Gt&gt;
            &lt;FieldRef Name='Modified' /&gt;
            &lt;Value IncludeTimeValue='TRUE' Type='DateTime'&gt;2011-02-09T12:00:00Z&lt;/Value&gt;
         &lt;/Gt&gt;
         &lt;Lt&gt;
            &lt;FieldRef Name='Modified' /&gt;
            &lt;Value IncludeTimeValue='TRUE' Type='DateTime'&gt;2011-02-10T10:00:00Z&lt;/Value&gt;
         &lt;/Lt&gt;
      &lt;/And&gt;
   &lt;/Where&gt;
&lt;/Query&gt;</pre>
<p>Now by adding the <em><strong>IncludeTimeValue</strong></em> attribute, the timestamp is also considered while doing the comparisions. With this query all the list items modified between Feb-09<sup>th</sup> 12:00 and Feb-10<sup>th</sup> 10:00 are returned in the result set.</p>
