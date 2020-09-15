---
layout: post
title: "Optimizing Server Configuration for Development Farm"
date: 2012-10-09 15:38:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2013"]
permalink: ["/post/optimizing-server-configuration-for-development-farm"]
  ---
<!-- more -->
<p>Setting up development environments for SharePoint Server has always been very difficult and costly because of the hardware requirements specifications needed. Development environments mostly use a single server with built-in database. The hardware requirements for SharePoint Server 2013 Preview as suggested by Microsoft (Source: <a title="Technet" href="http://technet.microsoft.com/en-us/library/cc262485%28v=office.15%29.aspx#hwforwebserver">Technet</a>) <br /> Processor - 64 bit, 4 cores and RAM - 24GB</p>
<p>Getting development machines with 24 GB memory may not possible in most of the organizations. For a development environment, we might not need all the functionalities and features of SharePoint to work. Two features/services in SharePoint 2013 which consume lot of memory are 'Search Service' and 'Distributed Cache Service'. These services might not be needed in most of the development scenarios. Below are the details and uses about these services. Once we understand what these services do and are used for, we can either configure them to use less memory or stop them.</p>
<h2>Search Service</h2>
<p>The architecture of search has undergone lot of changes in SharePoint 2013. Many of the core components are replaced by the FAST Search components. Below are a few new components which are added in the new architecture</p>
<ul class="spd-ul">
<li>Crawl Component</li>
<li>Content Processing Component</li>
<li>Query Processing Component</li>
<li>Index Component</li>
<li>Analytic Processing Component</li>
</ul>
<p>All these components run as a process called 'noderunner.exe'. On a default single server installation of SharePoint there will five instances of noderunner.exe (one for each of the component listed above). There is another process called 'Host Controller', which monitors the noderunner processes. If any of the noderunner.exe fails, the host runner will restart that process.</p>
<p>REDUCE THE PERFORMANCE LEVEL</p>
<p>Search has become the most integral part in SharePoint 2013 now. It&rsquo;s tied up with many other components. For example, the web analytics service is not a new Service Application anymore, it's integrated with search and all the reports are generated using the Search Services. So, it's not a good idea to stop this service. But we can limit the memory usage or reduce the performance level for Search Service.</p>
<p>By default the performance level for a Search Service is set to Maximum. Using the below powershell cmdlet we can reduce the performance level to Reduced</p>
<pre class="brush:ps;auto-links:false;toolbar:false" contenteditable="false">Set-SPEnterpriseSearchService -PerformanceLevel Reduced</pre>
<p>LIMIT THE MEMORY USAGE FOR NODERUNNER.EXE</p>
<p>These processes consume lot of memory on the server. We can configure the maximum amount of memory this process can use. Edit the noderunner.exe's config file which is available at this location &ndash; 'C:\Program Files\Microsoft Office Servers\15.0\Search\Runtime\1.0\noderunner.exe.config'. Search for the 'nodeRunnerSettings' element and assign the maximum amount of memory (in MB) that can be used by the process. By default this is 0, which means unlimited.</p>
<p><em> &lt;nodeRunnerSettings memoryLimitMegabytes="50" /&gt;</em></p>
<p>Save the config file and restart all the noderunner.exe processes. </p>
<h2>Distributed Cache Service</h2>
<p>A new caching service is added in SharePoint 2013 called 'Distributed Cache Service' which is built based on Windows Server AppFabric Distributed Cache. Many features rely on this service to store data for fast retrieval when needed. This is used by services/features like Authentication Token Cache, Micro Blogging features, My Site Social Feeds etc.,</p>
<p>HOW TO STOP THIS SERVICE</p>
<p>This service can be managed from the 'Services on Server' page in the central admin. It can be started/stopped from here.</p>
<p>ALLOCATE LESS MEMORY</p>
<p>By default when SharePoint 2013 preview is installed, Distributed Cache Service's memory allocation is set to 10 percent of the total physical memory allocation. Using the below powershell cmdlets we can change the memory allocation for this service.</p>
<pre class="brush:ps;auto-links:false;toolbar:false" contenteditable="false">$instanceName ="SPDistributedCacheService Name=AppFabricCachingService"
$serviceInstance = Get-SPServiceInstance | ? {($_.service.tostring()) -eq $instanceName -and ($_.server.name) -eq $env:computername}
$serviceInstance.Unprovision()
Set-CacheHostConfig -Hostname &lt;HostName&gt; -cacheport 22233 -cachesize &lt;Size in MB&gt;
$serviceInstance.Provision()</pre>
<p><em><br /></em>The above cmdlets stops the Caching Service, and sets the memory allocation to the specified number of megabytes (MB), and then starts the Caching service. </p>
<h2>Do not create new Web Applications</h2>
<p>By default, there will be one web application created on 80 port of the server. A site collection will also be created, but we have to choose the template for the site collection to get provisioned. Never create multiple web applications, as they might create multiple Application Pools which will consume memory. It&rsquo;s good to use a single web application or use the same application pool as far as possible. This keeps only one w3wp.exe running and uses less memory when compared to two worker processes.</p>
<p>In my development environment, I have set the memory limit for Distributed Cache Service to 300 MB, changed the search performance level to Reduced, and set the memory limit for noderunner.exe to 50MB. With these changes I am able to work on my server with 6GB memory. Most of the functionalities are working fine.</p>
<p><em>Note: All the configurations and practices which I mentioned above are for farms which will be used by developers. These should NOT be used for Integration/Testing/Staging/Production environments.</em></p>
