---
layout: post
title: How to create a new Microsoft Teams Team using PnPjs within your SPFx components
comments: true
published: true
categories: ["SPFx", "PnPjs"]
tags: ["Teams", "SharePoint","SPFx", "SPFx webpart", "PnPjs"]
---

PnPjs offers great flexibility in quickly connecting to various services in your Office 365 suite. I was working on an SPFx webpart which manages Teams provisioning for an organization. The webpart accepts requests from users and when an admin approves the request it provisions a new MS Team with some custom settings and apps.

With PnPjs (used 2.0.11 when writing), creating a new team is a 2 step process. First we have to create a new group, or use an existing group, and then add/create a new Team to the Office 365 group.

Below is the sample code on how to do this.

```typescript
import { graph } from '@pnp/graph';
import '@pnp/graph/groups';
import { GroupType } from '@pnp/graph/groups';
import "@pnp/graph/teams"

const groupName = "Research and Development";
const groupNickName = "RnD";
const group = await graph.groups.add(groupName, groupNickName, GroupType.Office365);

const team = await graph.groups.getById(group.data.id).createTeam({
            funSettings:{
                allowGiphy: true,
                allowCustomMemes: true,
                allowStickersAndMemes: false,
                giphyContentRating: "Strict"
            },
            guestSettings:{
                allowDeleteChannels: false,
                allowCreateUpdateChannels: true,
            }
        });
```

When executing these 2 steps in sequence, we might often face a 404 error for the teams creation call. This happens because the graph API is not able to find the newly created group. Even though the graph.groups.add() is succesfully executed, the group is still being provisioned and our call to create the team will fail with a 404.

So, before trying to create a team, check if the group is ready and delay the process untill the group is ready. 
Here is how we can do it.

```typescript
const group = await graph.groups.add(groupName, groupNickName, GroupType.Office365);

// Wait until the new group is ready
let newGroup = null;
let retry:boolean = true;
while (retry){
    try {
        newGroup = await graph.groups.getById(projectId).get();
        retry = false;
    } catch (ex) {
        // Wait for 10 seconds before checking again
        await this._delay(10000);
        retry = true;
    }
}

const team = await graph.groups.getById(projectId).createTeam({
    funSettings:{
        allowGiphy: true,
    }
});

// A helper function to delay for the given milliseconds of time.
private _delay = (ms: number) => {
    return new Promise( resolve => setTimeout(resolve, ms) );
}
```