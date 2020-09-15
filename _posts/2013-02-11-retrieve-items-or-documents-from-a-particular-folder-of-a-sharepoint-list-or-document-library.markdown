---
layout: post
title: "Retrieve Items or Documents from a particular folder of a Sharepoint List or Document Library"
date: 2013-02-11 11:29:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2010", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/retrieve-items-or-documents-from-a-particular-folder-of-a-sharepoint-list-or-document-library"]
  ---
<!-- more -->
<p>Lists and Libraries are the commonly used data storage components is a Sharepoint portal. With data growing huge in the lists, site administrators usually create folders to make the data organized. We use the '<em>ViewAttribute</em>' of the <em>SPQuery</em> object to fetch all the items from the folders and sub folders. Below is a sample code snippet</p>
<pre class="brush:csharp;auto-links:false;toolbar:false" contenteditable="false">using (SPWeb spWeb = SPContext.Current.Site.AllWebs["Reports"])
{
    SPList spList = spWeb.Lists.TryGetList("ManageReports");
    SPQuery spQuery = new SPQuery();
    //Setting the ViewAttribute to "Scope='Recursive'" 
    //fetches all items from the list,
    //ie., also the items within the folders
    spQuery.ViewAttributes = "Scope=\"Recursive\"";
    SPListItemCollection items = spList.GetItems(spQuery);
}</pre>
<p>If the '<em>ViewAttribute</em>' is not specified for the <em>SPQuery</em> object, then only those items which are in the root folder (ie., the root level of the list) are returned. So, we need to specify the scope in <em>ViewAttribute</em> property to get the items from all the folders (if any) of a list.</p>
<p><strong>GETTING ITEMS FROM A SPECIFIC FOLDER</strong><br /> In some scenarios we might need to fetch only the items within a sepcific folder. For this we can use the '<em>Folder</em>' property of <em>SPQuery</em> class. Prepare the <em>SPQuery</em> object with the required CAML query, set the <em>RowLimit</em> and other required settings. And then set the '<em>Folder</em>' property to a Folder object which represents the folder instance from where we need the items. Refer the below code snippet</p>
<pre class="brush:csharp;auto-links:false;toolbar:false" contenteditable="false">using (SPWeb spWeb = SPContext.Current.Site.AllWebs["Reports"])
{
    SPList spList = spWeb.Lists.TryGetList("ManageReports");
    SPQuery spQuery = new SPQuery();
                
    //Getting the folder object from the list 
    SPFolder folder = spList.RootFolder.SubFolders["FolderName"];
    //Set the Folder property
    spQuery.Folder = folder;

    SPListItemCollection items = spList.GetItems(spQuery);
    //Now the items collection will have the items only from the given Folder
}</pre>
<p>&nbsp;</p>
<p>&nbsp;</p>
