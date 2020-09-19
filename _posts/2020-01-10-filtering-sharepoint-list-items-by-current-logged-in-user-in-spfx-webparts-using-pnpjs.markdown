---
layout: post
title: "Filtering SharePoint list items by current logged-in user in SPFx webparts"
date: 2020-01-10 11:29:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "PnPjs", "SPFx"]
permalink: /post/filtering-sharepoint-list-items-by-current-logged-in-user-in-spfx-webparts-using-pnpjs
---
<!-- more -->
<h2>How to filter list items based on a Person type field?</h2>
<p>There could be scenarios where we have to filter or query for SharePoint list items&nbsp;based on a Person field. For example, we might want to show the list items created by the current logged in user. This&nbsp;can be done using the below REST call.</p>
<pre class="brush:js;collapse:true;gutter:false;html-script:true" contenteditable="false">https://&lt;site-url&gt;/_api/web/lists/getbytitle('Personal Links')/items?$filter=Author eq 6</pre>
<p>In this REST call, the current user's&nbsp;ID is used to filter the items. This ID is specific to a site collection, based on the User Information List of that particular site.</p>
<h2>Within an SPFx webpart</h2>
<p>In the classic pages&nbsp;one can get the current user's id using the <em><strong>_spPageContextInfo.userId</strong>. </em>This property is also exposed in the SPFx webpart through <em><strong>this.context.pageContext.legacyPageContext.userId</strong></em>.</p>
<p>But, it is <strong>not recommended to use the legacyPageContext object</strong>, as it could change in the future releases. And it is provided only to migrate the old code and webparts.</p>
<p>The next approach would be, to make a rest call to find the current user's ID. Something like below</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">    sp.web.currentUser.get().then(user =&gt; {
      // Query for the list items using user.Id
      // sp.web.lists.......
    });</pre>
<p>The only drawback with this approach is, we are making an additional REST call just to find the current user's id. So, we make two REST calls to query for list items&nbsp;filtered for the current user.&nbsp;Can we avoid the REST call to get the user id? Here are few approaches</p>
<h2>Other approaches</h2>
<p>The REST API also supports filtering for person type fields using EMail and User Token. Here are REST calls to filter for items created by the current user</p>
<pre class="brush:js;auto-links:false;toolbar:false;highlight:ram" contenteditable="false">https://&lt;site-url&gt;/_api/web/lists/getbytitle('Personal Links')/items?
$filter=Author/EMail eq 'r@tenant-name.onmicrosoft.com'

https://&lt;site-url&gt;/_api/web/lists/getbytitle('Personal Links')/items?
$filter=Author/Name eq 'i:0#.f|membership|r@tenant-name.onmicrosoft.com'</pre>
<p>To use these filters within an SPFx webpart, we can use the <strong>pageContext&nbsp;</strong>property of the <strong>WebpartContext</strong> object, which has the user's email and loginName populated. If the current user's email or user token are used to filter the items, the additional REST call to find the user's ID can be avoided.</p>
<p>Below is the method which shows how we can use these variables and PnPjs to query for list items created by current user.</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">  private _getPersonalLinks = (filterByEmail:boolean):Promise&lt;Array&lt;any&gt;&gt; =&gt; {

    // This legacyPageContext is not recommended to be used.
    console.log(this.context.pageContext.legacyPageContext.userId);

    if(filterByEmail)
    {
      // Filter by EMail
      return sp.web.lists.getByTitle('Personal Links').items
      .filter(`Author/EMail eq '${encodeURIComponent(this.context.pageContext.user.email)}'`)
      .select('Title')
      .get();
    }
    else
    {
      //Filter by LoginName (i:0#.f|membership|r@tenant-name.onmicrosoft.com)
      let userToken = `i:0#.f|membership|${this.context.pageContext.user.loginName}`;
      return sp.web.lists.getByTitle('Personal Links').items
      .filter(`Author/Name eq '${encodeURIComponent(userToken)}'`)
      .select('Title')
      .get();
    }
  }

  private _links:Array&lt;any&gt;;

  protected async onInit(): Promise&lt;void&gt; {
    sp.setup({spfxContext:this.context});
    this._links = await this._getPersonalLinks(false);
    return super.onInit();
  }</pre>
<p>I would recommend using the loginName instead of the email for filtering, because there could be some users in an Office 365 tenant without an email address.</p>
<p><strong>For external users,</strong> this approach will not work, because the user token has a different format (like&nbsp;i:0#.f|membership|ramprasad_em_demo.com#ext#@example.onmicrosoft.com). So, in such cases use the <strong>context.pageContext.user.isAnonymousGuestUser</strong> and <strong>context.pageContext.user.isExternalGuestUser </strong>properties to find if the user is an external user and query based on the user's ID.</p>
<p>You can find the full sample on Github - <a href="https://github.com/RamPrasadMeenavalli/myPersonalLinks" target="_blank">https://github.com/RamPrasadMeenavalli/myPersonalLinks</a></p>
