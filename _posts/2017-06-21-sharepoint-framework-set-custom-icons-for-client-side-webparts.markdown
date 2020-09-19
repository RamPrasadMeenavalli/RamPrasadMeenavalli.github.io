---
layout: post
title: "SharePoint Framework: Set Custom Icons for Client Side Webparts"
date: 2017-06-21 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "SharePoint Server", "SharePoint 2016", "SharePoint 2019", "SPFx"]
permalink: /post/sharepoint-framework-set-custom-icons-for-client-side-webparts
---
<!-- more -->
<p>Every Client Side Webpart within a SharePoint Framework App package can have an icon which is shown in the toolbox when we add a webpart to the modern page.</p>
<p>Following properties can be used to set a custom icon</p>
<ul class="spd-ul">
<li><strong>iconImageUrl</strong>: Using a custom image</li>
<li><strong>officeFabricIconFontName</strong>: Using a font icon</li>
</ul>
<h2>Client-Side Web Part icon using an image</h2>
<p>Open the <strong>'[your webpart name].manifest.json'</strong> file from <strong>src/webparts/[your webpart name]</strong>. And replace the <strong>officeFabricIconFontName</strong> with <strong>iconImageUrl</strong>. This expects a path to an image of 38X38 dimension. At this time of writing this article only absolute URLs are accepted for this property. When its not possible to specify an absolute URL, we can encode an image into base 64 format and provide the encoded string to this property.</p>
<p><img src="/assets/images/SPFX_Set_Icon_0.png" alt="" /><img src="/assets/images/SPFX_Set_Icon_1.png" alt="" /></p>
<h2>Client-Side Web Part icon using a font icon</h2>
<p>This is used by default when you create a SharePoint Framework webpart solution. This property is set to "Page" and the page icon appears by default to all the custom webparts</p>
<p><img src="/assets/images/SPFX_Set_Icon_2.png" alt="" /></p>
<p>Following are the possible values for this property</p>
<ul class="spd-ul floatedLis" style="display: inline-block;">
<li>Add</li>
<li>AddGroup</li>
<li>AlignCenter</li>
<li>AlignLeft</li>
<li>AlignRight</li>
<li>Attach</li>
<li>Back</li>
<li>BackToWindow</li>
<li>BlowingSnow</li>
<li>Bold</li>
<li>BulletedList</li>
<li>Calendar</li>
<li>Camera</li>
<li>Cancel</li>
<li>Chart</li>
<li>CheckMark</li>
<li>ChevronLeft</li>
<li>ChevronRight</li>
<li>CirclePlus</li>
<li>Clear</li>
<li>ClearFormatting</li>
<li>ClearNight</li>
<li>CloudWeather</li>
<li>Cloudy</li>
<li>Completed</li>
<li>CompletedSolid</li>
<li>Delete</li>
<li>DocLibrary</li>
<li>Duststorm</li>
<li>Edit</li>
<li>EditMirrored</li>
<li>Embed</li>
<li>Emoji2</li>
<li>ExcelLogo</li>
<li>FacebookLogo</li>
<li>FavoriteStar</li>
<li>FavoriteStarFill</li>
<li>Filter</li>
<li>Financial</li>
<li>Fog</li>
<li>Folder</li>
<li>FolderOpen</li>
<li>Font</li>
<li>FontStyleSerif</li>
<li>Forward</li>
<li>Freezing</li>
<li>Frigid</li>
<li>FullScreen</li>
<li>Globe</li>
<li>Group</li>
<li>HailDay</li>
<li>HailNight</li>
<li>Header</li>
<li>Italic</li>
<li>Link</li>
<li>Message</li>
<li>MobileSelected</li>
<li>More</li>
<li>MultiSelect</li>
<li>Nav2DMapView</li>
<li>News</li>
<li>NumberedList</li>
<li>OfficeVideoLogo</li>
<li>OneNoteLogo</li>
<li>OpenFile</li>
<li>OpenWith</li>
<li>Org</li>
<li>Page</li>
<li>PageAdd</li>
<li>PartlyCloudyDay</li>
<li>PartlyCloudyNight</li>
<li>Photo2</li>
<li>Photo2Add</li>
<li>Photo2Remove</li>
<li>PhotoCollection</li>
<li>Picture</li>
<li>Play</li>
<li>PowerApps</li>
<li>PowerBILogo</li>
<li>PowerPointLogo</li>
<li>Precipitation</li>
<li>Preview</li>
<li>Rain</li>
<li>RainShowersDay</li>
<li>RainShowersNight</li>
<li>RainSnow</li>
<li>Recent</li>
<li>Refresh</li>
<li>Remove</li>
<li>RemoveLink</li>
<li>Reshare</li>
<li>Ribbon</li>
<li>RightDoubleQuote</li>
<li>Save</li>
<li>Search</li>
<li>Settings</li>
<li>Share</li>
<li>SharepointLogo</li>
<li>SIPMove</li>
<li>Snow</li>
<li>SnowShowerDay</li>
<li>SnowShowerNight</li>
<li>Squalls</li>
<li>StackIndicator</li>
<li>Sunny</li>
<li>SwayLogo</li>
<li>Sync</li>
<li>System</li>
<li>Tablet</li>
<li>TabletSelected</li>
<li>Teamwork</li>
<li>Thunderstorms</li>
<li>Tiles</li>
<li>TVMonitorSelected</li>
<li>TwitterLogo</li>
<li>Underline</li>
<li>Unfavorite</li>
<li>Video</li>
<li>View</li>
<li>VisioLogo</li>
<li>Webcam</li>
<li>WordLogo</li>
<li>WorldClock</li>
<li>YammerLogo</li>
<li>Zoom</li>
<li>ZoomIn</li>
<li>ZoomOut</li>
</ul>
<p>When both the properties are specified, the property <strong>officeFabricIconFontName</strong> takes the precedence and iconImageUrl is ignored.</p>
<p>We can also specify an App Icon for a SharePoint Framework App Package which is shown in the Classic view of site contents page. Check the steps on my other post - <a title="SharePoint Framework: Set Custom App Icon" href="http://spdeveloper.co.in/sharepoint2016/sharepoint-framework-custom-app-icon.aspx">SharePoint Framework: Set Custom App Icon</a></p>
