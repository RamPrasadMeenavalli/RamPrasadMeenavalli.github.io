---
layout: post
title: "SharePoint JSOM code to Hide/Remove Ribbon tabs in SharePoint List Forms or Pages"
date: 2016-06-20 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "Classic Sites", "SharePoint Server", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/sharepoint-jsom-code-to-hide-remove-ribbon-tabs-in-sharepoint-list-forms-or-pages"]
  ---
<!-- more -->
<p>The Ribbon shows certain tabs like 'Edit', 'View', 'Items', 'List' etc., when we are on a list form page or when a List is added to any SharePoint page. In some cases we do not want to show these tabs to the users, which can be hidden by adding JSOM code to remove the unwanted tabs. Below is the code to get an instance of the Ribbon and remove the Edit tab from the Ribbon. This JavaScript can be added to a certain list form page through a Script Editor WebPart or can be included in a custom list definition.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false"> &lt;script type="text/javascript"&gt;
        function hideEditRibbon() {
            try {
                var ribbon = SP.Ribbon.PageManager.get_instance().get_ribbon();
                // Set the tab to the "Browse" tab
                SelectRibbonTab("Ribbon.Read", true);
                // Remove the "Edit" tab from a list from from the ribbon.
                ribbon.removeChild('Ribbon.ListForm.Edit');
            } catch (ex) {

            }
        }
      SP.SOD.executeOrDelayUntilScriptLoaded(function () {
          var pm = SP.Ribbon.PageManager.get_instance();
          pm.add_ribbonInited(function () {
              hideEditRibbon();
          });
          var ribbon = null;
          try {
              ribbon = pm.get_ribbon();
          }
          catch (e) { }

          if (!ribbon) {
              if (typeof (_ribbonStartInit) == "function")
                  _ribbonStartInit(_ribbon.initialTabId, false, null);
          }
          else {
              hideEditRibbon();
          }
      },
   "sp.ribbon.js");
    &lt;/script&gt;</pre>
<p>Below is the list of valid Ribbon tabs which can be hidden/removed by passing this value to the <em><strong>ribbon.removeChild</strong></em> method in the above snippet.</p>
<table style="width: 90%;">
<tbody>
<tr>
<td>
<ul class="spd-ul">
<li>Ribbon.BDCAdmin</li>
<li>Ribbon.DocLibListForm.Edit</li>
<li>Ribbon.ListForm.Display</li>
<li>Ribbon.ListForm.Edit</li>
<li>Ribbon.PostListForm.Edit</li>
<li>Ribbon.SvcApp</li>
<li>Ribbon.Solution</li>
<li>Ribbon.UsageReport</li>
<li>Ribbon.WikiPageTab</li>
<li>Ribbon.PublishTab</li>
<li>Ribbon.WebPartPage</li>
<li>Ribbon.WebApp</li>
<li>Ribbon.SiteCollections</li>
<li>Ribbon.ManageTrust</li>
<li>Ribbon.EditingTools.CPEditTab</li>
</ul>
</td>
<td>
<ul class="spd-ul">
<li>Ribbon.EditingTools.CPInsert</li>
<li>Ribbon.Image.Image</li>
<li>Ribbon.Document</li>
<li>Ribbon.Library</li>
<li>Ribbon.ListItem</li>
<li>Ribbon.List</li>
<li>Ribbon.Link.Link</li>
<li>Ribbon.Table.Layout</li>
<li>Ribbon.Table.Design</li>
<li>Ribbon.WebPartInsert.Tab</li>
<li>Ribbon.WebPartOption</li>
<li>Ribbon.Calendar.Events</li>
<li>Ribbon.Calendar.Calendar</li>
<li>Ribbon.Permission</li>
</ul>
</td>
</tr>
</tbody>
</table>
