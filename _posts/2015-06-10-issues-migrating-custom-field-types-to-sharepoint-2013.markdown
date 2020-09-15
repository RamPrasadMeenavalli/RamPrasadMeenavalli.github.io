---
layout: post
title: "Issues with migrating custom field types to SharePoint Server 2013"
date: 2015-06-10 12:00:00 +0530
comments: true
published: true
categories: ["blog", "archives"]
tags: ["SharePoint Server", "SharePoint 2013", "SharePoint 2016"]
permalink: ["/post/issues-migrating-custom-field-types-to-sharePoint-2013", "/post/issues-migrating-custom-field-types-to-sharepoint-2013"]
  ---
<!-- more -->
<p style="text-align: justify;">Custom field types are one of those components which cause issues when migrated to SharePoint Server 2013 from its earlier versions. Following are the basic troubleshooting steps to be validated</p>
<ul class="spd-ul" style="text-align: justify;">
<li>Use the 'Import SharePoint 2010' template available in Visual Studio 2012 to upgrade the custom field types solution.</li>
<li>Make sure the SharePointProductVersion is set to 15.0 instead of 14.0 in the manifest file for the Solution element</li>
<li>The .net Framework target should be 4.5 for the TargetOfficeVersion of the project files.</li>
<li>Make sure the references to user controls or files are updated to refer the 15 hive (eg., /_layouts/15/controltemplates/...)</li>
</ul>
<p style="text-align: justify;">Even after verifying these, the custom field types were not working in our environment. The field was present in the lists and the migrated data was visible in the list views. Also, the field control was working in 2010 site/s (which are migrated to SharePoint 2013). But, after the visual upgrade the field control was not rendering in the new and edit forms. After some research I found that it's because of the change in the control rendering mechanism in SharePoint 2013.</p>
<p style="text-align: justify;">The rendering design of custom field types has a significant change in SharePoint Server 2013, compared to earlier versions of SharePoint. For versions prior to SharePoint 2013 we had to create a custom field control (a user control) and this is bound to the field type using its <em><strong>FieldRenderingControl</strong></em> property. Usually this property is overridden and the custom user control is instantiated and returned to the pipeline. Sample snippet below</p>
<pre class="brush:csharp;auto-links:false;toolbar:false" contenteditable="false">public override BaseFieldControl FieldRenderingControl
{
     [SharePointPermission(SecurityAction.LinkDemand, ObjectModel = true)]
    get
    {
        BaseFieldControl fieldControl = new CustomFieldControl();
        fieldControl.FieldName = this.InternalName;

        return fieldControl;
    }
}</pre>
<p style="text-align: justify;">SharePoint 2013 has changed the display behavior of custom field types with Client Side Rendering mechanism. The fields now have a property called <strong><em>JSLink</em></strong>, using which we can refer a custom js template to define how the control/s render for custom field types. After some research I found that whenever SharePoint finds a JSLink property, it ignores the <em><strong>FieldRenderingControl</strong></em> property and as a result our custom field control is never rendered. To force the rendering of the custom field control, we have to override the JSLink property and return an empty string for this property. The final code snippet should like below</p>
<pre class="brush:csharp;auto-links:false;toolbar:false" contenteditable="false">public override string JSLink
        {
	get
            {
                try
                {
                    var mode = FieldRenderingControl.ControlMode;
                    return string.Empty;
                }
                catch (Exception)
                {
                    // Reference to custom JS file if 
                    //user control is not found
                    return "customtemplate.js";
                }
            }
            set
            {
                base.JSLink = value;
            }
}</pre>
<p>With this, the custom field control should be loaded and field type should work on the visually upgraded SharePoint site.</p>
