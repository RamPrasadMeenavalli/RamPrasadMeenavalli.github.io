---
layout: post
title: "SharePoint 2010 Notifications and Status"
date: 2011-02-12 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2010"]
permalink: ["/post/sharepoint-2010-notifications-and-status"]
  ---
<!-- more -->
<h2>Using the ECMAScript of SharePoint 2010</h2>
<p>SharePoint Server 2010 comes with a flashy UI. On the home page of a SharePoint Site, we see many components like the ribbon tool bar, Notifications, Status message, Quick Launch etc., As a developer we should be able to customize all these components. In this article lets explore how to create custom '<em>Notifications</em>' and '<em>Status</em>' messages using the ECMAScript of SharePoint 2010</p>
<p>What are these Notifications and Status messages?<br /> A <em>Notification</em> is a small text area which gets displayed at the top right corner of the window, while we perform some actions like saving a page, checking out a page etc., (this is like the notification which we see while sending a mail through gmail). This is a simple text area with yellow background and a dark yellow border as shown in the screen below. This is something new in SharePoint 2010 which is not available in MOSS 2007.</p>
<p><img src="/image.axd?picture=/notification.png" alt="" /></p>
<p>A <em>Status</em> message is also like a Notification, but it stretches to the width of page. This lies between the body and header of the page. To be more precise, it comes below the title bar (where title and breadcrumb are displayed), or below the ribbon toolbar (when it is opened). This can have different colored backgrounds, signifying the severity of the message. Shown below is a default Status message displayed when we check out a page for editing.</p>
<p><img src="/image.axd?picture=/statusarea.png" alt="" /></p>
<h2>Creating Custom Notification</h2>
<p>Using <strong><em>'SP.UI.Notify.addNotification()'</em></strong> method we can create custom notifications on a page.<br /><br />var value = SP.UI.Notify.addNotification(strHtml, bSticky);<br />strHTML - the HTML text that is to be displayed in the notification area.<br />bSticky - true/false. A boolean value which specifies whether the notification should be fixed or to auto closed.<br />value - The ID of the notification added to the page.<br /><br />By default if we add a Notification with <em>bSticky</em> value as false, it closes after 5 seconds. If we specify its value as true, then we have to explicitly close the notification by passing the ID (return value while creating the notification) to the <em><strong>SP.UI.Notify.removeNotification(nid) method</strong></em>. These two methods are part of the <em>SP.UI.Notify</em> class of the ECMAScript library.</p>
<h2>Creating Custom Status Messages</h2>
<p>The class <em>SP.UI.Status</em> provides seven methods for managing custom Status messages.</p>
<ul class="spd-ul">
<li><strong>SP.UI.Status.addStatus(strTitle, strHtml, atBegining);</strong></li>
<li><strong>SP.UI.Status.appendStatus(sid, strTitle, strHtml);</strong></li>
<li><strong>SP.UI.Status.removeAllStatus(hide);</strong></li>
<li><strong>SP.UI.Status.removeStatus(sid);</strong></li>
<li><strong>SP.UI.Status.setStatusPriColor(sid, strColor);</strong></li>
<li><strong>SP.UI.Status()</strong> - This is the constructor which creates a 'Status' object</li>
<li><strong>SP.UI.Status.updateStatus(sid, strHtml);</strong></li>
</ul>
<p>Using these methods we can create a Status message, update it, change the color and remove the status when needed. </p>
<h2>Code Snippet</h2>
<p>Now lets put this into action. Create a page and add a 'Content Editor' webpart to the page. Copy the below script and paste it in the content editor webpart. Make sure you go to the 'Edit HTML Source' view of the content editor webpart and add this script, if we add this in normal view, the scripts gets removed and will not work.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">&lt;script language="ecmascript" type="text/ecmascript"&gt;

var statusId = '';
var notifyId = '';

function AddNotification(strTitle, isSticky) {
    var txtnid = document.getElementById("txtNotifId");
    notifyId = SP.UI.Notify.addNotification(strTitle, isSticky);
    txtnid.value = notifyId;
}

function RemoveNotification(nid) {
    SP.UI.Notify.removeNotification(nid);
    notifyId  = '';
}</pre>
<p>In brief the above script has some buttons which are bound with some javascript functions through which we create and close the Notifications and Status messages.<br /><br /> <strong>POSSIBLE COLORS FOR STATUS MESSAGES</strong> <br /> The status messages displayed on the pages supports only four colors. Below are the four color and their priorities</p>
<table>
<tbody>
<tr>
<td style="width: 190px; background-color: #df5a5b;">&nbsp;</td>
<td>&nbsp;Very high priority</td>
</tr>
<tr>
<td style="width: 190px; background-color: #fdf289;">&nbsp;</td>
<td>&nbsp;Important</td>
</tr>
<tr>
<td style="width: 190px; background-color: #71b84f;">&nbsp;</td>
<td>&nbsp;Success</td>
</tr>
<tr>
<td style="width: 190px; background-color: #c9d7e6;">&nbsp;</td>
<td>&nbsp;Information</td>
</tr>
</tbody>
</table>
<p>So, only four colors are supported by SharePoint 2010 for status messages. However we can show the status messages with other colors by changing some CSS styles. Check this <a title="Article" href="http://spdeveloper.co.in/articles/pages/custom-status-messages-with-different-colors.aspx">article</a>, which gives a detailed explanation on doing this.<br /><br /> <strong>NOTE:</strong> In the above example we are creating a page using the OOB layout and master page. So, these scripts will work as expected. For the ECMAScript to work, we need to load the sp.js and sp.ui.js files. As we are using the default master page (which loads these JS files), our code snippet will work. If you are using a custom master page, make sure you load all the JS files needed.<br /> <strong>Note</strong>:SP.UI Namespace is defined in the following JS files</p>
<ul class="spd-ul">
<li>SP.Core.js</li>
<li>SP.js</li>
<li>SP.UI.Dialog.js</li>
</ul>
