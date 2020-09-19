---
layout: post
title: "Upgrade-SPSolution | Installs the newly added features when a solution(WSP) is updated"
date: 2013-03-23 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2010", "SharePoint 2013"]
permalink: /post/install-features-after-update-spsolution
---
<!-- more -->
<p><em>WSP</em> is the most common and reliable way any developer uses to deploy SharePoint components between different environments. As suggested by Microsoft in this article <a title="Upgrading a Farm Solution in SharePoint 2010" href="http://msdn.microsoft.com/en-us/library/aa543659.aspx">Upgrading a Farm Solution in SharePoint 2010</a>, there are two ways of upgrading a farm solution.</p>
<ul class="spd-ul">
<li>Remove existing solution, and redeploy using the new package</li>
<li>Use update commands (in powershell or using stsadm) to upgrade the solution</li>
</ul>
<p>The first approach suggested will remove all the existing components when the farm solution is uninstalled and removed. Then after adding and installing the new solution, all the components and files in this package are deployed to the farm.<br /> But the second approach, in which we use the update commands, the components are actually replaced with the updated versions from the package. Its suggested that the number of files and features should be same on the package when we are upgrading it.</p>
<p>So, when we follow the second approach, any new feature/s added in the package will not be installed in the farm. They need to be installed manually. But many administrators wanted to update the solution, and also the new features to be installed in the farm. But '<em>Update-SPSolution</em>' doesn't install newly added features. Here is a solution...</p>
<p>I created a custom cmdlet which gets added in the SharePoint 2010 Management shell - <a title="Upgrade-SPSolution" href="http://upgradespsolution.codeplex.com/">Upgrade-SPSolution</a>. This cmdlet updates the solution by replacing the components from the new package to the farm, and then creates a one time timer job, which installs all the newly added features in the farm. Download the <a title="Upgrade-SPSolution" href="http://upgradespsolution.codeplex.com/">Upgrade-SPSolution</a> from codeplex and install it in your farm. You will have a new cmdlet 'Upgrade-SPSolution'.</p>
