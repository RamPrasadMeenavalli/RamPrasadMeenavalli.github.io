---
layout: post
title: Adding OOTB/Default modern SPFx webparts to classic pages
comments: true
published: true
categories: ["SPFx"]
tags: ["SharePoint", "SharePoint Classic","SPFx", "SPFx webpart"]
hideAds: true
---

![](/assets/images/adding-spfx-webparts-to-classic-pages.png)

The SharePoint Modern Sites provides many modern webparts which are very easy to use and appealing. When we develope any custom SPFx webpart, it can be added to any classic page, if we install the SPFx app to the classic site. But the OOTB/default SPFx webparts are not available.

Follow these steps to enable adding OOTB/Default SPFx webparts to classic pages

> For people like me who are lazy readers, check out the [video](https://www.youtube.com/watch?v=tA3SXVTQjXw) at the end of this post

1. Open the sharepoint workbench from any site (https://site.sharepoint.com/sites/team**/_layouts/15/workbench.aspx**)
2. Add a modern webpart, which we want to add on classic pages, to the workbench. For example, add the Weather webpart.
3. After the webpart is added, click on **Web part data** on the command bar, and click on **Classic Pages**
    ![](/assets/images/adding-spfx-webparts-to-classic-pages-1.png)
4. Copy the **xml** for the Classic page and save it as a file with .webpart extension. Example: SPFx-Weather.webpart
5. Now open the Webpart Gallery of the classic site (https://site.sharepoint.com/sites/classic/**_catalogs/wp**)
6. Click on **Files** from the library ribbon and click on **Upload Document**
    ![](/assets/images/adding-spfx-webparts-to-classic-pages-3.png)
7. Browse and upload the SPFx-Weather.webpart file and save it.
8. Now, edit any classic page and try to add a webpart. We should see the modern SPFx Weather webpart under the 'Miscellaneous' (or a custom group which was given in previous step) category.
![](/assets/images/adding-spfx-webparts-to-classic-pages-2.png)

These steps can be repeated for all the OOTB/Default modern SPFx webparts and editors can then add these webparts to classic pages using the Webpart Adder controls.

{% include embed/youtube.html id='tA3SXVTQjXw' %}