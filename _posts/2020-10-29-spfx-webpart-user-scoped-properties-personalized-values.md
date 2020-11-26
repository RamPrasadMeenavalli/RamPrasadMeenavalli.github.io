---
layout: post
title: "Creating a user scoped property for SPFx webparts"
date: 2020-10-29 00:00:00 +0530
comments: true
published: true
categories: ["spfx"]
tags: ["SharePoint", "SPFx webpart", "SPFx"]
permalink: /post/spfx-webpart-user-scoped-properties-personalized-values
---

Custom webpart properties provide a great way to support configurable values for SPFx webparts. The properties which we configure using the default property pane of an SPFx webpart can be changed when one edits the page and these property values are then for all users.

The traditional webparts which were used in the classic pages, supported User Scoped or Shared custom properties. The custom properties in SPFx webparts are always scoped to be shared, and they do not support User scoped properties. User scoped properties can be really useful for some use cases where we want to allow the end users to change the configurations or properties.

For example, a world time webpart can allow each user to choose their own choice of a time zone and shows a clock with that time zone.

![](/assets/images/spfx-webpart-personalization.png)

> Watch this video for detailed steps on how we can extend the world time webpart to support personalization.

{% include embed/youtube.html id='0UvweCXRFgQ' %}

#### How to support User Scoped properties in SPFx webparts

As SPFx webparts doesn't support user scoped properties by default, the same can be handled using custom code. Here is a high level overview of what we need

1. A unique id to identify the webpart instance
2. The logged in user's name
3. A custom Panel to allow the user to change the values
4. Storing the user selected value/s for the properties.

Below are the steps on how to extend the worldTime webpart from PnP SP Starter Kit to allow end users to choose a time zone of their own.

#### A unique id to identify the webpart instance
We have to save the user selected property values for a specific webpart instance. This should be unique so that we know what user selected value should be used later for a specific webpart.

The WebPartContext class provides a property called instanceId which we can use to uniqyely identify an instance of a webpart added to a page. This value can be passed to the component which needs the user selected value. 


#### The logged in user's name
We would need the logged in user's name to save the user selected values for the webpart. Use the PageContext object to get the current user's login name

In the World Time webpart sample make the following changes to get the webpart unique Id and logged in user's name

```typescript
export interface IWorldTimeProps {
  description: string;
  timeZoneOffset: number;
  errorHandler: (errorMessage: string) => void;
  webpartId: string;
  loginName: string;
}
```

Pass this value to the React component
```typescript
    const element: React.ReactElement<IWorldTimeProps> = React.createElement(
      WorldTime,
      {
        description: (timeZones.TimeZones.getTimeZone(this.properties.timeZoneOffset)).displayName,
        timeZoneOffset: this.properties.timeZoneOffset,
        errorHandler: (errorMessage: string) => {
          this.context.statusRenderer.renderError(this.domElement, errorMessage);
        },
        webpartId: this.context.instanceId,
        loginName: this.context.pageContext.user.loginName
      }
    );
```

#### A custom Panel to allow the user to change the values
A custom panel has to be shown which allows the end users to change the values. In world clock example, lets assume we want the end users to select a time zone of their choice. For this we have to show a dropdown with the list of time zones using a Fluent UI Panel.

Here is a snippet which is added to the WorldTime component in our example

```typescript
<Panel isOpen={this.state.showPanel}>
    Change the timezone
    <Dropdown options={this.getTimeZones()} selectedKey={timeZoneOffset} onChange={this._onPropertyChanged}></Dropdown>
</Panel>
```
#### Storing the user selected value/s for the properties.
Once the end users change the selected value, this have to stored and used for future page loads. This value should be saved for the current user and for the specific webpart instance id. There could be multiple options which to save this data.
- Browser Storage : This is easy to implement to save the data on browsers local storage. However this will not work when the user opens the page from a different browser or device.
- Persisted storage: Like a SharePoint list, User's onedrive or any other storage system. This will be more robust as the value is persisted and will work when opened from any browser and device.

A sample code on how to use the PnP Client Storage to save these values locally.

```typescript
private _storage:PnPClientStorage;
...
this._storage = new PnPClientStorage();
...
// To get the value stored in local storage
this._storage.local.get(`${this.props.webpartId}-${this.props.loginName}`) as number;

//To save the value in local storage
private _onPropertyChanged = (event, option, index) => {
this.setState({
    timeZoneOffset:option.key,
    showPanel: false
}, () => {
    this._storage.local.put(`${this.props.webpartId}-${this.props.loginName}`, option.key);
});
}
```
The complete code sample can be found in the [Github Repo][1]

[1]:https://github.com/RamPrasadMeenavalli/spfx-webparts-concepts/tree/main/src/webparts/clock