---
id: authorization
title: Authorization
sidebar_label: Authorization
---

## Authorization

Some of the APIs need users’ authorization before they can be called. We have divided these APIs into multiple scope according to the scope of usage. The users can select scope to authorize. After a scope is authorized, all of its APIs can be used directly.

When such an API is called:

- If the user has not accepted or rejected this authorization, a pop-up window will appear to ask the user if he/she wants to accept. The API can be called only after the user clicks to accept;
- If the user has accepted authorization, the API can be called directly;
- If the user has rejected authorization, no pop-up appears. Instead, API fail callback will be accessed directly. **Developers should make the scenario compatible where the user has rejected to authorization.**

## Obtain User Authorization Settings

The developer can use mp.getSetting(todo) to get the user's current authorization state.

## Go to Settings

The user can control the authorization state of the Mini Program in Mini Program Settings by clicking About in the upper right and then Settings in the upper right.

The developer can call mp.openSetting(todo) to open Settings, and guide the user to enable authorization.

## Initiate the Authorization Request in Advance

The developer can use wx.authorize to initiate the authorization request to the user in advance before calling the API to authorize

## Scope List

| Property          | Corresponding APIs                                                 | Description         |
| ----------------- | ---------------------------------------------------- | ------------------- |
| scope.userLocation    | bn.getLocation, bn.chooseLocation |boolean                                    | Geographic location |
| scope.writePhotosAlbum | bn.saveImageToPhotosAlbum, bn.saveVideoToPhotosAlbum | Saves to album      |
| scope.camera      | camera component                                     | Camera              |
