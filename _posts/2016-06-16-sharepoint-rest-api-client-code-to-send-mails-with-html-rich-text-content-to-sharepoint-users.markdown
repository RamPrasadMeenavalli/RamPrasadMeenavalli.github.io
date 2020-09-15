---
layout: post
title: "SharePoint REST API/Client Code to send mails with HTML/Rich Text Content to SharePoint Users"
date: 2016-06-16 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Office 365", "SharePoint Online", "SharePoint Server", "SharePoint 2013", "SharePoint 2016", "SharePoint 2019"]
permalink: ["/post/sharepoint-rest-api-client-code-to-send-mails-with-html-rich-text-content-to-sharepoint-users"]
  ---
<!-- more -->
<p>Following is the code snippet to send emails to local SharePoint users with HTML Content in the email body.</p>
<p><strong> The recipients should be valid SharePoint Users. Mails cannot be sent to non-SharePoint users and external users. </strong></p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">function sendMail(toList, subject, mailContent) {
    var restUrl = _spPageContextInfo.webAbsoluteUrl + "/_api/SP.Utilities.Utility.SendEmail",
    restHeaders = {
        "Accept": "application/json;odata=verbose",
        "X-RequestDigest": $("#__REQUESTDIGEST").val(),
        "Content-Type": "application/json;odata=verbose"
    },
    mailObject = {
        'properties': {
            '__metadata': {
                'type': 'SP.Utilities.EmailProperties'
            },
            'To': {
                'results': toList
            },
            'Subject': subject,
            'Body': mailContent,
            "AdditionalHeaders":
                {
                    "__metadata":
                       { "type": "Collection(SP.KeyValue)" },
                    "results":
                    [
                        {
                            "__metadata": {
                                "type": 'SP.KeyValue'
                            },
                            "Key": "content-type",
                            "Value": 'text/html',
                            "ValueType": "Edm.String"
                        }
                    ]
                }
            
        }
    };
    return $.ajax({
        contentType: "application/json",
        url: restUrl,
        type: "POST",
        data: JSON.stringify(mailObject),
        headers: restHeaders
    });
} </pre>
<p>The highlighted headers in the mailObject above are required to specify the content type of the email as HTML. Without this header, the mail will be sent as a plain text and the HTML formatting is ignored.</p>
<h3>Using this helper method</h3>
<p>The above method can be used as a helper method to send mails from your client code or SharePoint Apps or Add-Ins. Make sure to include the required jQuery references</p>
<pre class="brush:js;auto-links:false;toolbar:false" contenteditable="false">//SUBJECT OF THE EMAIL
var subject = "SUBJECT OF THE MAIL";
//CONTENT OF THE MAIL - CAN BE HTML OR PLAIN TEXT. 
var mailContent = "&lt;h3&gt;Some Heading for the mail&lt;/h3&gt;&lt;p&gt;Content&lt;/p&gt;&lt;div&gt;Content&lt;/div&gt;";
//AN ARRAY HAVING THE RECIPIENT DETAILS.
//THE USERNAME/EMAIL-ID SHOULD BE OF A VALID SHAREPOINT USER. 
//EXTERNAL USERS OR EXTERNAL EMAIL IDS ARE NOT SUPPORTED
var toList = ["DOMAIN\\USERNAME", "USER1-EMAIL-ID@DOMAIN.COM"]

sendMail(toList, subject, mailContent).done(function (response) {
    console.log("Mail Sent.");
}).fail(function () {
    console.error("Error while sending mail.");
});</pre>
<p>&nbsp;</p>
<p><strong>&nbsp;</strong></p>
