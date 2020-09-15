---
layout: post
title: "Creating Accessible Modal Dialogs"
date: 2011-07-05 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2010", "SharePoint 2013"]
permalink: ["/post/creating-accessible-modal-dialogs"]
  ---
<!-- more -->
<p>In the previous <a title="Modal Dialogs in SharePoint 2010" href="http://spdeveloper.co.in/articles/pages/using-sharepoint-2010-modal-dialogs.aspx">article</a> we have seen what 'Modal Dialogs' are, and how can we create simple dialogs. Basically we create a modal dialog by calling a JavaScript function in the href attribute of an anchor tag.</p>
<p>&lt;a href="javascript:OpenPopUpPage();"&gt;Open Dialog&lt;/a&gt;</p>
<p>Modal Dialogs created using this approach are not accessible. What if the end users have JavaScript disabled? The anchor tag (which is supposed to open the modal dialog) will not function as expected.</p>
<p>To solve this, we can make the anchor tag as a simple link to the URL (which we want to be opened in the modal dialog). And using jQuery we should manage the behavior of this anchor tag. If JavaScript is disabled, it acts as a simple link, and when JavaScript is enabled it opens the page in a modal dialog.<br />Please visit the <a title="jQuery" href="http://www.jquery.com">jQuery</a> home page to know about its usage.</p>
<p>To bring this to action follow the below steps.</p>
<ul class="spd-ul">
<li>Get the <a title="jQuery" href="http://www.jquery.com">jQuery</a> script library and attach it to the master page or page layout where you want the accessible modal dialog to be created.</li>
<li>Create the anchor tag to the URL (which is to be opened in the modal dialog) with some unique CSS class.<br />&lt;a href="http://www.spdeveloper.co.in" class="sp2010-dialog"&gt;Show Dialog&lt;/a&gt;</li>
<li>Create a JavaScript file with the below code</li>
</ul>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">(function($){
  $.fn.sharePop = function(){
    if(typeof OpenPopUpPage == 'function'){
      return this.each(function(i){
        if($(this).attr('href') != null){
          $(this).click(function(e){
            e.preventDefault();
            OpenPopUpPage($(this).attr('href'));
          });
        }
      });
    }
    else{
      return false;
    }
  };
})(jQuery);

	$(document).ready(function(){
	$('.sp2010-dialog').sharePop();
	});</pre>
<ul class="spd-ul">
<li>Attach this JavaScript file to the page where we want the modal dialog (the page in which the anchor tag for modal dialog is to be shown).</li>
</ul>
<p>So, when a user opens the page, the link renders as a simple navigation link. And if JavaScript is enabled, then the jQuery function (shown above) will execute. This method will override the default behavior of all the anchor tags having class name as 'sp2010-dailog', and opens the link in a modal dialog. So, this becomes an accessible modal dialog link, as users and search engines which do not use JavaScript can still view the content.</p>
