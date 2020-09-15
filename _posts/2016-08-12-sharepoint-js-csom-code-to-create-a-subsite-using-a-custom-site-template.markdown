---
layout: post
title: "SharePoint JS CSOM code to create a subsite using a custom Site Template"
date: 2016-08-12 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "SharePoint Server", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/sharepoint-js-csom-code-to-create-a-subsite-using-a-custom-site-template"]
  ---
<!-- more -->
<p>Below is the code snippet to create a subsite in a SharePoint site collection using an existing custom site template.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">var webUrl = "newSubSite",
webTitle = "New Sub Site",
webDesc = "Description about the new web/site";

var siteTemplate;
var CONST_PROJECT_TEMPLATE = "Custom Project Template";

var ctx = SP.ClientContext.get_current();
var webTemplates = ctx.get_web().getAvailableWebTemplates(1033, false);
ctx.load(webTemplates);
ctx.executeQueryAsync(function () {

    //Get Custom Site Template Name
    var tmpls = webTemplates.getEnumerator();
    while (tmpls.moveNext()) {
        var tmpl = tmpls.get_current();
        if (tmpl.get_title() == CONST_PROJECT_TEMPLATE) {
            siteTemplate = tmpl.get_name();
        }
    }


    //Create SPWeb
    var webCreationInformation = new SP.WebCreationInformation();
    webCreationInformation.set_title(webTitle);
    webCreationInformation.set_description(webDesc);
    webCreationInformation.set_language(1033);
    webCreationInformation.set_url(webUrl);
    //Set false, if Unique Permissions are required
    webCreationInformation.set_useSamePermissionsAsParentSite(true); 
    webCreationInformation.set_webTemplate(siteTemplate);
    var newSiteObj = ctx.get_web().get_webs().add(webCreationInformation);
    ctx.load(newSiteObj, 'Title', 'Url');
    ctx.executeQueryAsync(function () {
        console.log("Site created. Title : " + newSiteObj.get_title() + ", Url : " + newSiteObj.get_url());
    }, function (sender, args) {
        console.error(args.get_message());
    });
}, function (sender, args) {
    console.error(args.get_message());
});</pre>
<p>&nbsp;</p>
