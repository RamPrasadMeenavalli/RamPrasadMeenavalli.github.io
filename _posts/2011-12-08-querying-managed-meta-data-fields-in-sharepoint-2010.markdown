---
layout: post
title: "Querying Managed Meta Data fields in SharePoint 2010"
date: 2011-12-08 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Online", "SharePoint Server", "SharePoint 2010", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: /post/querying-managed-meta-data-fields-in-sharepoint-2010
---
<!-- more -->
<p>I have seen many developers using the &lt;contains&gt; or &lt;eq&gt; element of CAML to query items based on a Managed Meta Data column. Using these elements of CAML for querying, returns irrelevant items in the result.</p>
<p>Consider we have a pages library with 'Enterprise Keywords' column (OOB Site Column) in the library. Lets assume we have three pages with the Enterprise Keywords as shown in the screen below.</p>
<p><img src="/assets/images/query_mms.png" alt="" /></p>
<p>Now if we need to fetch all the items from this library, having '<em>hare</em>' or '<em>hares</em>' in their 'Enterprise Keywords' column, we would end up writing a CAML query as shown below</p>
<pre class="brush:xml;auto-links:false;toolbar:false" contenteditable="false">&lt;Query&gt;
    &lt;Where&gt;
        &lt;Or&gt;
            &lt;Contains&gt;
                &lt;FieldRef Name="TaxKeyword" /&gt;
                &lt;Value Type="Text"&gt;hare&lt;/Value&gt;
            &lt;/Contains&gt;
            &lt;Contains&gt;
                &lt;FieldRef Name="TaxKeyword" /&gt;
                &lt;Value Type="Text"&gt;hares&lt;/Value&gt;
            &lt;/Contains&gt;
        &lt;/Or&gt;
    &lt;/Where&gt;
&lt;/Query&gt;</pre>
<p>But this query will return all the three items from the library. Ideally we should get only one item, ie., default.aspx, as only this item is having the keyword as hare. But as we are using &lt;contains&gt; in the caml query, the words '<em>harebell</em>' and '<em>sharepoint</em>' contains the word '<em>hare</em>', so these 2 items are also returned. </p>
<p>So, to get only the relevant items from the library, we should not use the &lt;contains&gt; element of CAML for querying Managed Meta Data columns. We have to use the <strong><em>WssIds</em></strong> of the enterprise keywords intead of the actual word. Also there is a new CAML element introduced in SharePoint 2010 - '<em><strong>&lt;In&gt;</strong></em>'. This works similar to the IN clause in a SQL query. So for the above scenario the CAML query should be as shown below</p>
<pre class="brush:xml;auto-links:false;toolbar:false" contenteditable="false">&lt;Query&gt;
    &lt;Where&gt;
        &lt;In&gt;
            &lt;FieldRef LookupId="True" Name="TaxKeyword" /&gt;
            &lt;Values&gt;
                &lt;Value Type="Integer"&gt;WSS ID OF KEYWORD&lt;/Value&gt;
                &lt;Value Type="Integer"&gt;WSS ID OF KEYWORD&lt;/Value&gt;
            &lt;/Values&gt;
        &lt;/In&gt;
    &lt;/Where&gt;
&lt;/Query&gt;</pre>
<p>How to get the WSS ID of the keyword?<br /> Get all the keywords required which are of type TaxonomyFieldValue. This class has a property called WssId (TaxonomyFieldValue.WssId) which gives the WssId of the keyword. Using this property get the WssId of each keyword and use this Id as values inside IN element of CAML. </p>
<p>Use the below code to prepare the CAML query for querying the Managed Meta Data field</p>
<pre class="brush:xml;auto-links:false;toolbar:false" contenteditable="false">String CAML_QUERY = @"&lt;Where&gt;&lt;In&gt;&lt;FieldRef LookupId=""True"" Name=""TaxKeyword"" /&gt;&lt;Values&gt;{0}&lt;/Values&gt;&lt;/In&gt;&lt;/Where&gt;";
String VALUES = @"&lt;Value Type=""Integer""&gt;{0}&lt;/Value&gt;";

StringBuilder strValues = new StringBuilder();
Literal litDisplay = new Literal();

SPListItem item = SPContext.Current.ListItem;
TaxonomyFieldValueCollection keywords = (TaxonomyFieldValueCollection)item["TaxKeyword"];

foreach (TaxonomyFieldValue keyword in keywords)
{
    strValues.AppendLine(String.Format(VALUES, keyword.WssId));
}

String strQuery = String.Format(CAML_QUERY, strValues.ToString());

SPQuery sPQuery = new SPQuery();
sPQuery.Query = strQuery;

SPListItemCollection items = item.ParentList.GetItems(sPQuery);</pre>
<p>With the above implementation we will get only default.aspx in the result for the example scenario considered in this article (ie. default.aspx). The above code is for a webpart which is used in a page, to display all the articles having the same keywords as that of the current page. Modify the code as required based on the area you use it.</p>
