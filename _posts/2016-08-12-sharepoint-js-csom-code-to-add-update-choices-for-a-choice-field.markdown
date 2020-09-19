---
layout: post
title: "SharePoint JS CSOM Code to add/update Choices for a Choice Field"
date: 2016-08-12 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "SharePoint Server", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: /post/sharepoint-js-csom-code-to-add-update-choices-for-a-choice-field
---
<!-- more -->
<p>Following code snippet shows how to update the 'Status' column within a Tasks list and add new choice values to the field/column using JS CSOM. The Status column within a Tasks list is of type Choice Field.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">var CONST_TASKS_LIST_TITLE = "Tasks";
var CONST_TASKS_LIST_COLUMN = "Status";
var strStatusValues = "Deferred,Closed,Blocked,Postponed for next Release";
           
var ctx = SP.ClientContext.get_current();
var listDevTasks = ctx.get_web().get_lists().getByTitle(CONST_TASKS_LIST_TITLE);
var fldStatus = listDevTasks.get_fields().getByInternalNameOrTitle(CONST_TASKS_LIST_COLUMN)
var fldStatusChoice = ctx.castTo(fldStatus, SP.FieldChoice);
ctx.load(fldStatusChoice);
ctx.executeQueryAsync(function () {
    var newValues = strStatusValues.split(",");
    var currentChoices = fldStatusChoice.get_choices();
    for (var i = 0; i &lt; newValues.length; i++) {
        currentChoices.push(newValues[i]);
    }
    fldStatusChoice.set_choices(currentChoices);
    fldStatusChoice.updateAndPushChanges();
    ctx.executeQueryAsync(function () {
        console.log("Added new choice values to the column");
    }, function (sender, args) { deferred.reject(args.get_message()); });
}, function (sender, args) { deferred.reject(args.get_message()); });</pre>
<p>&nbsp;</p>
