---
layout: post
title: "Resolving the issues while configuring a Stand-Alone installation"
date: 2012-10-01 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2013"]
permalink: ["/post/stand-alone-installation-failed-configuration-database-sharepoint-2013"]
  ---
<!-- more -->
<p style="margin-bottom: 5px;">After a long wait, I got a chance to install SharePoint 2013 Preview in one of the development environments. Whenever I install any Microsoft Product, I eagerly wait for the Configuration Failed/Installation Failed screen. As soon I started running the per-requisite installer for SharePoint 2013, things were getting installed without any issues. I was little surprised and a bit worried thinking the installation will complete without any issues. But Microsoft never disappoints me. Below are the issues I faced while doing a Stand-Alone installation of SharePoint 2013 preview.</p>
<h2 style="margin-top: 5px;">Error 1: Failed to connect to the configuration database</h2>
<p><em>"Failed to refresh all running servers in the cluster. You may need to restart the cluster for these changes to take effect."</em> This error might come during the step 2 of the configuration wizard. Run the configuration wizard from command prompt with the following parameters</p>
<p class="ac"><em>psconfigui.exe -cmd Configdb create SkipRegisterAsDistributedCacheHost</em></p>
<ul class="spd-ul">
<li>Open command prompt</li>
<li>Change the directory to 'C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\BIN'</li>
<li>And run the command as shown above</li>
</ul>
<h3>What will this command actually do?</h3>
<p>After reading many SharePoint 2013 forums, I found that this issue comes while configuring the 'Distributed Cache Service', which is new in SharePoint 2013. While we run the configuration wizard, at step 2, it creates the configuration database and tries to configure the 'Distributed Cache Service' and this was failing in a Stand-Alone installation. The command which was used above will create the configuration database and continues the registration process by skipping the Distributed Cache Service configuration.</p>
<p>So, by following this workaround, we can complete the installation successfully, but the Cache service is not enabled. This workaround will limit the functionality and prevent the full evaluation of all the features. This issue doesn't come while doing a complete installation. Microsoft is yet to update the documentation about this issue for Stand-Alone installation (Reference: <a href="http://social.technet.microsoft.com/Forums/en-US/sharepointitpropreview/thread/57dfc126-c730-4c9b-bb70-5e4a2acdddd5" target="_blank">SharePoint forum</a>)</p>
<h2>Error 2: Failed to create sample data</h2>
<p>This error comes at the 8th step in the configuration wizard. If you check the additional information, it says &ndash; "The Server Service is not started", which is self-explanatory. Open the Services console and start the 'Server' service and run the configuration wizard.</p>
