---
layout: post
title: "Get all Site Users - SharePoint Server 2013 REST end point through Angular JS : Get the response in JSON format."
date: 2015-04-22 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "Classic Sites", "SharePoint Server", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/sharepoint-all-users-rest-api-end-point-angular-js-json-response"]
  ---
<!-- more -->
<h2>Intro</h2>
<p>SharePoint Server 2013 provides many REST endpoints which can be used to consume data and perform most of the CRUD operations on a Site Collections. Consuming the REST end points through various JavaScript frameworks like Angular JS, Knockout JS etc., is the most widely used approach these days. These frameworks are built on the fundamentals of JSON and it's easy to integrate and develop scripts if we have our data in JSON format. But, the SharePoint Server 2013 REST APIs/end points return the response in ATOM or XML by default.</p>
<h3>How to receive a JSON response from REST APIs?</h3>
<p>We can instruct the server to give the response in a JSON format instead of ATOM or XML feed. When the REST end points are called, the Request Header is checked for the following property/attribute</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">Accept: application/json;odata=verbose</pre>
<p>Once this property is found in the request header, the response is returned as JSON. So, this property should be added to the Request Header before the REST call is made. For Angular JS, refer the following script on how to add the header information. This script is a simple scenario which shows the code snippets to get the list of users in a site collection, by consuming the REST end points using Angular JS.</p>
<h3>HTML Template</h3>
<pre class="brush:html;auto-links:false;toolbar:false" contenteditable="false">&lt;html ng-app="AllSiteUsers"&gt;&lt;head&gt;
&lt;title&gt;All Site Users&lt;/title&gt;
&lt;script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.0.8/angular.min.js"&gt;&lt;/script&gt;
&lt;script src="script.js"&gt;&lt;/script&gt;
&lt;/head&gt;

&lt;body ng-controller="GetAllUsers"&gt;

&lt;table&gt;
&lt;thead&gt;&lt;th&gt;Login Name&lt;/th&gt;&lt;th&gt;Display Name&lt;/th&gt;&lt;/thead&gt;
&lt;tbody&gt;
&lt;tr ng-repeat="user in users"&gt;
&lt;td&gt;{{user.LoginName}}&lt;/td&gt;
&lt;td&gt;{{user.Title}}&lt;/td&gt;
&lt;/tr&gt;
&lt;/tbody&gt;
&lt;/table&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>
<h3>script.js</h3>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">var mod = angular.module('AllSiteUsers', []);

mod.controller('GetAllUsers', ['$scope', '$http', function ($scope, $http) {

	$http.defaults.headers.common['Accept'] = 'application/json;odata=verbose';
	var url='http://rams-sharepoint/sites/ram/_api/web/siteusers';
	$http.get(url).
        success(function(data) {
            $scope.users = data.d.results;
        });
}]);</pre>
<p>&nbsp;</p>
