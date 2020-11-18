---
layout: post
title: Creating a modern Document Set using PnPjs from an SPFx webpart
comments: true
published: true
categories: ["SPFx", "PnPjs"]
tags: ["Document Set", "SharePoint", "Modern Document Set", "SPFx webpart"]
---

Document Sets are useful for grouping relevant documents together and maintain a shared metadata for all the documents. Document Sets are supported in the modern sites and it is available when we activate the site collection feature called **'Document Sets'** (Site Settings -> Site Collection Administration -> Site collection features)

![](/assets/images/document-set-feature.png)

Here are the steps to be done for using a Document Set
1. Activate the 'Document Sets' feature
2. From the List Settings page of the library, Advanced Settings, select 'Yes' for Allow management of content types and save the settings.
3. In the list settings page of the library, under Content Types section, select 'Add from existing site content types' and select the Document Set content type.
4. Now you will be able to create a Document Set from the Modern UI of the document library.

### How to create a document set using PnPjs

While this is no direct API to handle document sets, we can create a new document set by performing some of the above steps using PnPjs (2.0.11). And then create a normal folder and change the content type id of the new folder to the id of the document set content type.

```typescript
import { sp } from '@pnp/sp';
import '@pnp/sp/sites';
import '@pnp/sp/features';
import '@pnp/sp/webs';
import '@pnp/sp/lists';
import '@pnp/sp/content-types';
import '@pnp/sp/folders';
import '@pnp/sp/items';

// Activate the Document Set feature using the feature ID
// This will be same for all tenants, as this a default/OOTB feature
const res = await sp.site.features.add("3bae86a2-776d-499d-9db8-fa4cdc7884f8", true);

// Add the content type to the document library
const lib = sp.web.lists.getByTitle("Documents");
// ID for the document set content type. This will be same across all the tenants.
const documentSetContentTypeId = "0x0120D520";
await lib.contentTypes.addAvailableContentType(documentSetContentTypeId);

// Create a normal folder within an existing folder
const folderName = "Manuals";
const newFolder = await lib.rootFolder.folders.getByName('Client Documents').addSubFolderUsingPath(folderName)
// Update the content type Id
const fldObj = await lib.rootFolder.folders.getByName('Client Documents').folders.getByName(folderName).listItemAllFields.get();
await lib.items.getById(fldObj.Id).update({
    ContentTypeId: documentSetContentTypeId
});
```
The above snippet creates a new Document Set called 'Manuals' within the folder 'Client Documents', inside the Shared Documents library.
