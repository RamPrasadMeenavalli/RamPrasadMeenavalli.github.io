---
layout: post
title: "SharePoint JSOM: How to create a list instance using a custom list definition?"
date: 2016-06-15 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "SharePoint Server", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: /post/sharepoint-JSOM-create-list-custom-list-definition
---
<!-- more -->
<h3 id="default">Using Default/OOB List Definitions</h3>
<p>Below is the code to create a list instance on a SharePoint site using one of the existing (OOB) list definitions.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">var clientContext = SP.ClientContext.get_current();
    var oWebsite = clientContext.get_web();
    
    var listCreationInfo = new SP.ListCreationInformation();
    listCreationInfo.set_title('My Announcements List');
    listCreationInfo.set_templateType(SP.ListTemplateType.announcements);

    this.oList = oWebsite.get_lists().add(listCreationInfo);

    clientContext.load(oList);
    clientContext.executeQueryAsync(
        Function.createDelegate(this, this.onQuerySucceeded), 
        Function.createDelegate(this, this.onQueryFailed)
    );</pre>
<p>The SP.ListTemplateType enumeration has the following List Definitions. Once can use the required template to create a list instance.</p>
<ul class="spd-ul">
<li>SP.ListTemplateType.GenericList</li>
<li>SP.ListTemplateType.DocumentLibrary</li>
<li>SP.ListTemplateType.Survey</li>
<li>SP.ListTemplateType.Announcements</li>
<li>SP.ListTemplateType.Contacts</li>
<li>SP.ListTemplateType.Events</li>
<li>SP.ListTemplateType.Tasks</li>
<li>SP.ListTemplateType.DiscussionBoard</li>
<li>SP.ListTemplateType.PictureLibrary</li>
<li>SP.ListTemplateType.DataSources</li>
<li>SP.ListTemplateType.XmlForm</li>
<li>SP.ListTemplateType.NoCodeWorkflows</li>
<li>SP.ListTemplateType.WorkflowProcess</li>
<li>SP.ListTemplateType.WebPageLibrary</li>
<li>SP.ListTemplateType.CustomGrid</li>
<li>SP.ListTemplateType.WorkflowHistory</li>
<li>SP.ListTemplateType.GanttTasks</li>
<li>SP.ListTemplateType.IssuesTracking</li>
</ul>
<h3 id="custom">How to create a List Instance using Custom List Definitions</h3>
<p>As the SP.ListTemplateType enumeration doesn't include the custom list definitions, we cannot use it when we want to create a list using custom list definitions. Instead, we have to fetch the custom list definitions details from the SharePoint Web and use that information for creating the list instance. Below is the code.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">//Title of the custom list definition/template
var LIST_TEMPLATE_TITLE = "MY_PROJECTS";  

var ctx = SP.ClientContext.get_current();
//Get the details of the custom list definition.
var projectsTmpl = ctx.get_web().get_listTemplates().getByName(LIST_TEMPLATE_TITLE);
ctx.load(projectsTmpl);

ctx.executeQueryAsync(function () {

	var projectslistCI, projectsList;
        projectslistCI = new SP.ListCreationInformation();
        projectslistCI.set_title("My Upcoming Projects");
        projectslistCI.set_templateFeatureId(projectsTmpl.get_featureId());
        projectslistCI.set_templateType(projectsTmpl.get_listTemplateTypeKind())
        projectsList = ctx.get_web().get_lists().add(projectslistCI);
        ctx.load(projectsList);

	//List created using custom list definition/tempalte.

	ctx.executeQueryAsync(function (sender, args) {
	},function(sender, args){
		//ERROR WHILE CREATING LISTS
		//LOG ERROR
	});

}, function (sender, args) {
	//ERROR WHILE FETCHING TEMPLATE DETAILS
	//LOG ERROR
});</pre>
<p>&nbsp;</p>
