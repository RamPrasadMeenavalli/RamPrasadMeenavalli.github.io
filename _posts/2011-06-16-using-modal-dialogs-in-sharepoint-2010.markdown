---
layout: post
title: "Using Modal Dialogs in SharePoint 2010"
date: 2011-06-16 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2010", "SharePoint 2013"]
permalink: ["/post/using-modal-dialogs-in-sharepoint-2010"]
  ---
<!-- more -->
<h2>Overview</h2>
<p>Modal Dialogs are one of the new features provided by SharePoint 2010. It's the dialog framework provided by the JavaScript client object model. Modal Dialogs can fetch data from anywhere and display it over the page. One good feature about the Modal Dialogs is, there won't be any navigation to another page, making the user to stay in the context of the current page. The number of post backs will also be reduced by using modal dialogs. These dialogs are used in a large scale in many of the OOB operations, like creating a page, viewing/editing item properties etc., These dialogs are JavaScript pop up dialogs with an iFrame in which the data is displayed.</p>
<h2>Using the Client Object Model</h2>
<p>'SP.UI' namespace from the client object model provides a static class 'SP.UI.ModalDialog' with many methods which are used for creating dialogs and controlling their behavior. Basically, for creating a custom modal dialog, we need to call a JavaScript method. This JavaScript call can be added wherever required, may be in a web part canvas, in content editor web part etc.,</p>
<h2>Code Snippet</h2>
<p>The method 'showModalDialog()' of the 'SP.UI.ModalDialog' class creates the dialog. It requires one parameter of an object called options. This parameter is set with the properties and settings needed for the dialog, like the URL to be opened by the dialog, its title, height &amp; width etc.,</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">var options ={
url: '/_layouts/spd/welcome.aspx',
title:'Welcome to SharePoint Developers Hub',
width:100,
height:150,
dialogReturnValueCallback: ShowStatus
};</pre>
<p>The options object should be prepared with the required attributes for the dialog as shown above. Now this object should be passed to the function 'SP.UI.ModalDialog.showModalDialog()', then a modal dialog is created. Below is the complete code snippet for a demo modal dialog.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">&lt;script type="text/javascript"&gt;
function ShowWelcomeDialog()
{
var htmlP = document.createElement('p');
var htmlMsg = document.createTextNode('For articles on SharePoint 2010, visit SharePoint Developers Hub - http://www.spdeveloper.co.in');
htmlP.appendChild(htmlMsg);
var options ={
html: htmlP,
title:'Welcome to SharePoint Developers Hub',
width:400,
height:75,
dialogReturnValueCallback: ShowStatus
};
SP.UI.ModalDialog.showModalDialog(options);
}

function ShowStatus(dialogResult, retValue)
{
SP.UI.Notify.addNotification("The dialog is closed");
}
&lt;/script&gt;
&lt;a title="Open Dialog" href="javascript:ShowWelcomeDialog();"&gt;Open Dialog&lt;/a&gt;</pre>
<p>In the above snippet, I created an object for options with some properties for the dialog box. I have assigned a call back function to the dialog through 'dialogReturnValueCallback' property. This function gets called when the dialog is closed. After the &lt;script&gt; is closed, I created an anchor tag setting its href attribute to the JavaScript function 'ShowWelcomeDialog()'.</p>
<p>In the callback function for the dialog I have created a simple Notification to be displayed on the page. For getting it into action, paste the above code in a Content Editor Web Part (in HTML View) in a SharePoint page, and on clicking the link a pop up should be opened. This JavaScript function can be called from anywhere (from a webpart, a button on ribbon menu etc., ) based on the requirement.</p>
<h2>Different Properties</h2>
<p>There are various properties the showModalDialog() method accepts through the options object. Below is the list of these properties</p>
<table>
<thead>
<tr>
<th>Property Name</th>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>allowMaximize</td>
<td>boolean (true/false)</td>
<td>Determines the visibility of the maximize button (at top right corner) for the modal dialog</td>
</tr>
<tr>
<td>args</td>
<td>object</td>
<td>The args property allows us to pass arbitrary properties into our dialog.</td>
</tr>
<tr>
<td>autoSize</td>
<td>boolean (true/false)</td>
</tr>
<tr>
<td>dialogReturnValueCallback</td>
<td>function</td>
<td>This property accepts a callback function which gets executed when the dialog is closed.</td>
</tr>
<tr>
<td>height</td>
<td>numeric</td>
<td>The height of the dialog</td>
</tr>
<tr>
<td>html</td>
<td>HTML Element</td>
<td>The HTML to be rendered in the window (when the URL property is not specified)</td>
</tr>
<tr>
<td>showClose</td>
<td>boolean (true/false)</td>
<td>Determines the visibility of the close button (at top right corner) for the modal dialog</td>
</tr>
<tr>
<td>showMaximized</td>
<td>boolean (true/false)</td>
<td>If set to true, the dialog will render maximized, ie., it fills the available screen space.</td>
</tr>
<tr>
<td>title</td>
<td>string</td>
<td>Title of the modal dialog. When no title is specified, the title of the document referred to by the Url property is used instead.</td>
</tr>
<tr>
<td>url</td>
<td>string</td>
<td>The URL of the page to be shown in the dialog.</td>
</tr>
<tr>
<td>width</td>
<td>numeric</td>
<td>The width of the dialog to be displayed</td>
</tr>
<tr>
<td>x</td>
<td>numeric</td>
<td>Specifies the starting position from the left, where the dialog is to be rendered</td>
</tr>
<tr>
<td>y</td>
<td>numeric</td>
<td>Specifies the starting position from the bottom, where the dialog is to be rendered</td>
</tr>
</tbody>
</table>
<p>The URL for the modal dialog can be anything, a publishing page or application page etc., We can close the dialog using the function 'commonModalDialogClose()' method of the SP.UI.ModalDialog class. There might be scenarios where we need to close the dialog through code behind (like in the click event of a Button), then pass the following JavaScript through the response object to close the dialog.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">this.Page.Response.Clear(); 
this.Page.Response.Write("&lt;script type=\"text/javascript\"&gt;window.frameElement. 
                    commonModalDialogClose(1, 'Success');&lt;/script&gt;"); 
this.Page.Response.End();</pre>
<p>So, for creating a modal dialog we need to call a JavaScript function (it's mostly specified in the anchor tag's href attribute). By following this approach, we are creating modal dialogs which lack accessibility. What if the end user disables JavaScript? This anchor tag will not open the dialog. I have written an <a title="Creating Accessible Modal Dialogs" href="http://spdeveloper.co.in/articles/pages/creating-accessible-modal-dialogs.aspx">article</a> on creating accessible modal dialogs, which you can go through once you are comfortable with creating basic dialogs.</p>
