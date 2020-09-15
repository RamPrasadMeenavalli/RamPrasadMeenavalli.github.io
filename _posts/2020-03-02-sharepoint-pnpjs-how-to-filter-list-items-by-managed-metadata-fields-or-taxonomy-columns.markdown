---
layout: post
title: "SharePoint PnPjs : How to filter list items by Managed Metadata Fields or Taxonomy Columns"
date: 2020-03-02 13:56:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "PnPjs", "SPFx"]
permalink: ["/post/sharepoint-pnpjs-how-to-filter-list-items-by-managed-metadata-fields-or-taxonomy-columns"]
  ---
<!-- more -->
<p>SharePoint REST APIs does not allow to Managed Metadata / Taxonomy fields within the OData $filter parameter. So, PnPjs which uses REST APIs internally, cannot query on a&nbsp;Managed Metadata column using the .filter() method.</p>
<p>But we can query using CAML query and the <strong>.getItemsByCAMLQuery&nbsp;</strong>method. Below is a&nbsp;sample&nbsp;document library which is has some policy documents uploaded and there is Managed Metadata Field called '<strong>Deparments</strong>' which accepts multiple values.</p>
<p><img src="/image.axd?picture=/listStructure.PNG" alt="" /></p>
<p>Below&nbsp;is a sample PnPjs V2 snippet which finds the list of documents that are tagged with a specific Managed Metadata term.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">import { setup as pnpSetup } from "@pnp/common";
import { sp } from "@pnp/sp";
import "@pnp/sp/webs";
import "@pnp/sp/lists/web";
import "@pnp/sp/items/list";

...
  protected onInit(): Promise&lt;void&gt; {
    return super.onInit().then(_ =&gt; {
      pnpSetup({spfxContext: this.context}); });
  }
...

const queryView = `
      &lt;View&gt;
        &lt;Query&gt;
          &lt;Where&gt;&lt;Eq&gt;
            &lt;FieldRef Name="Department" /&gt;
            &lt;Value Type='TaxonomyFieldTypeMulti'&gt;HR&lt;/Value&gt;
          &lt;/Eq&gt;&lt;/Where&gt;
        &lt;/Query&gt;
      &lt;/View&gt;
    `;

    const docs = await sp.web.lists
    .getByTitle("Policies")
    .getItemsByCAMLQuery({ViewXml: queryView},"File");
    
    docs.map(doc =&gt; {
      const depts = doc.Department.map(dept =&gt; {return dept.Label});
      console.log(`Url : ${doc.File.ServerRelativeUrl} || Department : ${depts.join(",")}`);
    });</pre>
<p>This will filter for all the documents which are tagged with the Taxonomy term 'HR'. And here is the output showing the required documents that are filtered.</p>
<p><img src="/image.axd?picture=/taxonomyQueryOP.PNG" alt="" /></p>
<p>This can be used in scenarios where an SPFx webpart should show content based on the current user's properties. The above example can be extended to show policy documents for the current user's department. The user's department can be fetched from the profile and the value can be passed to the CAML Query.</p>
