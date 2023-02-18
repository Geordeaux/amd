---
id: rpc-action
title: Action
sidebar_label: Action
---

import Mermaid from '@theme/Mermaid'

## Overview

An `Action` is only allowed to s end from `Worker` to `Native` and `Native` should **always reply the action** with a `Response`. if `Response` has not been defined, it should be an empty object.

<Mermaid chart={`sequenceDiagram Worker->>Native: Action Native->>Worker: Response`}></Mermaid>

## Actions

`callbackId` is required if `response` is specified in an `Action`.

`payload` is always required. if it's not been defined, it should be an empty object.

## Init workflow

### `$ready`

Notify `Native` that `Worker` is ready to receive messages.

## Basics

### `can-use`

Determines whether the Actions of the Mini Program are available in the current version.

`payload`

| Property | Type     | Default Value | Required | Description |
| -------- | -------- | ------------- | :------: | ----------- |
| action   | `string` |               |    ✓     | Action name |

`response`

| Property | Type   | Default Value | Required | Description                                                    |
| -------- | ------ | ------------- | :------: | -------------------------------------------------------------- |
| result   | `bool` |               |    ✓     | Indicates whether the API is supported in the current version. |

## Navigation

### `navigation-push`

Adds a new renderer to the top of the navigation history stack and loads a specific page.

`payload`

| Property | Type     | Default Value | Required | Description     |
| -------- | -------- | ------------- | :------: | --------------- |
| url      | `string` |               |    ✓     | url to navigate |

`response`

| Property | Type     | Default Value | Required | Description            |
| -------- | -------- | ------------- | :------: | ---------------------- |
| id       | `number` |               |    ✓     | rendererId of new page |

### `navigation-pop`

Removes `delta` existing renderers from the the navigation history stack.

`payload`

| Property | Type     | Default Value | Required | Description                                                                                                                                                                           |
| -------- | -------- | ------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| delta    | `number` | `1`           |          | The position in the history to which you want to move, relative to the current page. For example, `pop(2)` moves back two pages. If no value is passed then the default delta is `1`. |
| animated | `bool`   | true          |          | enable / disable animation                                                                                                                                                            |

### `navigation-redirect`

Destory the current renderer and create a new one with specified `url`.

`payload`

| Property | Type     | Default Value | Required | Description                |
| -------- | -------- | ------------- | -------- | -------------------------- |
| url      | `string` |               | ✓        | url to navigate            |
| animated | `bool`   | true          |          | enable / disable animation |

### `navigation-to-deeplink`

Opens deeplink defined in native.

`payload`

| Property | Type     | Default Value | Required | Description          |
| -------- | -------- | ------------- | :------: | -------------------- |
| url      | `string` |               |    ✓     | deeplink to navigate |

> APP deeplink definition: <https://confluence.toolsfdg.net/pages/viewpage.action?pageId=33099017>

### `navigation-back-mp`

Returns to the previous Mini Program. It can be called only when the current Mini Program is opened from another Mini Program.

`payload`

| Property  | Type     | Default Value | Required | Description                                                                                                                                                                                              |
| --------- | -------- | ------------- | :------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| extraData | `string` |               |          | The [encoded Base64URL string](https://base64.guru/standards/base64url/encode) data that needs to be returned to the previous Mini Program. The previous Mini Program can get this data from App.onShow. |

### `exit-mini-program`

Exit the miniprogram. There must be a click behavior to call successfully.

## Keyboard

### `keyboard-hide`

Hide keyboard

`payload`

| Property | Type | Default Value | Required | Description |
| -------- | ---- | ------------- | :------: | ----------- |

`response`

| Property | Type | Default Value | Required | Description |
| -------- | ---- | ------------- | :------: | ----------- |

### `keyboard-get-selected-text-range`

Obtains the cursor position of the input box after the focus of input, textarea, etc. Note: This API works only when it is called during the focus process.

`payload`

| Property | Type | Default Value | Required | Description |
| -------- | ---- | ------------- | :------: | ----------- |

`response`

| Property | Type     | Default Value | Required | Description                       |
| -------- | -------- | ------------- | :------: | --------------------------------- |
| start    | `number` |               |    ✓     | The start position of the cursor. |
| end      | `number` |               |    ✓     | The end position of the cursor.   |

## Network

### `request` _deprecated_

> This rpc action has been deprecated, please do not use it.

Send a http request.

`payload`

| Property | Type                      | Default Value | Required | Description                                                                                    |
| -------- | ------------------------- | ------------- | -------- | ---------------------------------------------------------------------------------------------- |
| url      | `string`                  | -             | ✓        | request url                                                                                    |
| data     | `string`                  | -             |          | request data                                                                                   |
| headers  | `object`                  | -             |          | set request header, can't set `Referer`. default value of `content-type` is `application/json` |
| timeout  | `number`                  | `10000`       |          | timeout of request, unit is **millisecond**                                                    |
| method   | [method](#request-method) | `'GET'`       |          | request method                                                                                 |

`response`

A [Response](#request-response) object.

### `download-file` _deprecated_

Downloads local resources to the local device. The client initiates an HTTPS GET request. The local temporary path to the file is returned. The maximum file size for a single download is 50 MB.

Note: Specify a reasonable Content-Type field in the server response header to ensure that the client handles the file type properly.

`payload`

| Property | Type     | Default Value | Required | Description                                                                                    |
| -------- | -------- | ------------- | -------- | ---------------------------------------------------------------------------------------------- |
| url      | `string` | -             | ✓        | URL to download resources                                                                      |
| headers  | `object` | -             |          | Set request header, can't set `Referer`. default value of `content-type` is `application/json` |

`response`

| Property     | Type     | Description                                                                                                                                         |
| ------------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| tempFilePath | `string` | Temporary file path. It is returned when the filePath to save files is not specified. The downloaded files will be stored in a temporary file path. |
| statusCode   | `number` | HTTP status code returned by the developer server                                                                                                   |

### `request-start`

Start an HTTPS network request, for more info check [Request start action](./rpc-action-request#action-request-start).

### `request-abort`

Abort request task, for more info check [Request abort action](./rpc-action-request#action-request-abort).

### `download-start`

Start an download task, for more info check [Downlaod start action](./rpc-action-download-file#action-download-start).

### `download-abort`

Abort download task, for more info check [Downlaod abort action](./rpc-action-download-file#action-download-abort).

### `upload-start`

Start an upload task, for more info check [Upload start action](./rpc-action-upload-file#action-upload-start).

### `upload-abort`

Abort upload task, for more info check [Upload abort action](./rpc-action-upload-file#action-upload-abort).

### `create-buffer-url`

create a unique url in memory

> It is a sync action.

`payload`

| Property | Type       | Default Value | Required | Description                                     |
| -------- | ---------- | ------------- | :------: | ----------------------------------------------- |
| data     | `number[]` |               |    ✓     | The data to be sent. number[] is an Uint8Array. |

`response`

| Property | Type     | Default Value | Required | Description          |
| -------- | -------- | ------------- | :------: | -------------------- |
| url      | `string` |               |    ✓     | unique url on native |

### `revoke-buffer-url`

revoke a unique url in memory

> It is a sync action.

`payload`

| Property | Type     | Default Value | Required | Description          |
| -------- | -------- | ------------- | :------: | -------------------- |
| url      | `string` |               |    ✓     | unique url on native |

`response`

| Property | Type | Default Value | Required | Description |
| -------- | ---- | ------------- | :------: | ----------- |

## Web Socket

### `websocket-create`

Create web socket instance, for more info check [WebSocket create action](./websocket#action-websocket-create).

### `websocket-send`

Send data/text to web, for more info check [WebSocket send action](./websocket#action-websocket-send).

### `websocket-close`

Close web socket, previously created by `websocket-create`, for more info check [WebSocket close action](./websocket#action-websocket-close).

## Network status

### `get-network-type`

Returns current network status (type).

`payload`

- empty

`response`

| Property       | Type     | Description                                           |
| -------------- | -------- | ----------------------------------------------------- |
| isConnected    | `bool`   | Current network status                                |
| networkType    | `string` | Current network type                                  |
| signalStrength | `number` | Signal strength, dbm (optional, not supported in iOS) |

`networkType` values

| networkType | Description                        |
| ----------- | ---------------------------------- |
| `wifi`      | Wi-Fi network                      |
| `2g`        | 2G network                         |
| `3g`        | 3G network                         |
| `4g`        | 4G network                         |
| `5g`        | 5G network                         |
| `unknown`   | Uncommon network types for Android |
| `none`      | No network                         |

## Permissions authorization

### `authorize`

Request user authorization / permission for sensitive data or device capabilities, for example - location, camera, user data. Granted permission is associated with specific mini program, user decision is saved until mini program uninstallation.

`payload`

| Property | Type   | Default Value | Required | Description                                                                            |
| -------- | ------ | ------------- | -------- | -------------------------------------------------------------------------------------- |
| scope    | string | -             | true     | The scope to be authorized. For details, see scope [list](./authorization#scope-list). |

`Error`

- permission denied

## Settings

### `get-setting`

Obtains the user's current settings. **Only the permissions that have been requested by the Mini Program from the user are returned.**

`response`

[`Settings`](#settings) object.

### `open-setting`

Opens the Mini Program settings interface and returns setting results.

`response`

[`Settings`](#settings) object.

## UI

### `show-toast`

Display message prompt box

`payload`

| Property | Type     | Default Value | Required | Description                                                  |
| -------- | -------- | ------------- | -------- | ------------------------------------------------------------ |
| title    | `string` | -             | ✓        | Prompt content                                               |
| icon     | `string` | `success`     |          | Icon. Avaliable values: `success`,`loading`, `none`, `error` |
| duration | `number` | `1500`        |          | The delay time for a prompt                                  |

### `hide-toast`

Hide message prompt box

### `show-loading`

Display loading prompt box

### `hide-loading`

Hide loading prompt box

### `show-modal`

Display the modal dialog box

`payload`

| Property    | Type      | Default Value | Required | Description                                                 |
| ----------- | --------- | ------------- | -------- | ----------------------------------------------------------- |
| title       | `string`  | -             | ✓        | Prompt title                                                |
| content     | `string`  | -             | ✓        | Prompt content                                              |
| confirmText | `string`  | -             |          | The text of the "OK "button, not more than 4 characters     |
| showCancel  | `boolean` | `success`     |          | Indicates whether to display the "Cancel" button            |
| cancelText  | `string`  | -             |          | The text of the "Cancel" button, not more than 4 characters |

`response`

```ts
{
  // return true when user clicked the confirm button, otherwise return false.
  status: true | false
}
```

### `show-action-sheet`

Display the action sheet

`payload`

| Property  | Type             | Default Value | Required | Description                                              |
| --------- | ---------------- | ------------- | -------- | -------------------------------------------------------- |
| alertText | `string`         | -             |          | Alert text                                               |
| itemList  | `Array.<string>` | -             | ✓        | The text array of the button, with a length limited to 6 |
| itemColor | `string`         | `#000000`     |          | The text color of the button                             |

`response`

```ts
{
  // The sequence number of the button tapped by the user, from top to bottom and starting from 0
  tapIndex: number
}
```

### `set-navigation-bar-title`

Sets the navigation bar titles of a specific renderer.

`payload`

| Property   | Type     | Default Value | Required | Description                 |
| ---------- | -------- | ------------- | :------: | --------------------------- |
| title      | `string` |               |    ✓     | title of the navigation bar |
| rendererId | `number` |               |    ✓     | the id of the renderer      |

### `set-navigation-bar-color`

Sets the color of the navigation bar in the page.

`payload`

| Property        | Type     | Default Value | Required | Description                                                                                                         |
| --------------- | -------- | ------------- | :------: | ------------------------------------------------------------------------------------------------------------------- |
| frontColor      | `string` |               |    ✓     | Foreground color values, including colors of button, title, and status bar; only #ffffff and #000000 are supported. |
| backgroundColor | `string` |               |    ✓     | Background color value, whose valid value is hexadecimal color                                                      |
| animation       | `object` |               |    ✓     | Animation effects                                                                                                   |
| rendererId      | `number` |               |    ✓     | the id of the renderer                                                                                              |

`object.animation` is composed as follows

| Property   | Type     | Default Value | Required | Description                   |
| ---------- | -------- | ------------- | :------: | ----------------------------- |
| duration   | `number` | `0`           |          | Animation change time (in ms) |
| timingFunc | `string` | `'linear'`    |          | Animation change mode         |

Valid values of object.animation.timingFunc

| Value     | Description                                          |
| --------- | ---------------------------------------------------- |
| linear    | The animation keeps the same speed from start to end |
| easeIn    | The animation starts at low speed                    |
| easeOut   | The animation ends at low speed                      |
| easeInOut | The animation starts and ends at low speed           |

### `start-pull-down-refresh`

Shows pull-down-to-refresh control and start refreshing animation. Only available if `enablePullDownRefresh` is enabled (see [window configuration](./config-global#window)).

`payload`

| Property   | Type     | Description            |
| ---------- | -------- | ---------------------- |
| rendererId | `number` | the id of the renderer |

### `stop-pull-down-refresh`

Stops pull-down-to-refresh control from refreshing and hides it. Only available if `enablePullDownRefresh` is enabled (see [window configuration](./config-global#window)).

`payload`

| Property   | Type     | Description            |
| ---------- | -------- | ---------------------- |
| rendererId | `number` | the id of the renderer |

### `set-tab-bar-item`

Dynamically sets the content of a tabBarn item.

`payload`

| Property         | Type     | Default Value | Required | Description                                                                                                                                                     |
| ---------------- | -------- | ------------- | :------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| index            | `number` |               |    ✓     | Specifies which item of tabBar, starting from the left.                                                                                                         |
| text             | `string` |               |          | The text of a button on tab.                                                                                                                                    |
| iconPath         | `string` |               |          | The path to the icon. The icon size is limited to 40 KB. Recommended size is 81 px \* 81 px, This parameter does not take effect when position is top.          |
| selectedIconPath | `string` |               |          | The path to the selected icon. The icon size is limited to 40 KB. Recommended size is 81 px \* 81 px, This parameter does not take effect when position is top. |

### `show-tab-bar`

Display tabBar.

`payload`

| Property  | Type      | Default Value | Required | Description           |
| --------- | --------- | ------------- | :------: | --------------------- |
| animation | `boolean` | `false`       |          | Show animation effect |

### `hide-tab-bar`

Hide tabBar.

`payload`

| Property  | Type      | Default Value | Required | Description           |
| --------- | --------- | ------------- | :------: | --------------------- |
| animation | `boolean` | `false`       |          | Show animation effect |

### `hide-home-button`

Hide Back HomeButton.When the bottom page of the mini program opened by the user is not the homepage, the "Back to Homepage" button is displayed by default. Developers can use `hideHomeButton` in the onShow/componentDidShow page to hide HomeButton.

`payload`

### `scroll-view-update`

Update context of ScrollView component.

`payload`

| Property     | Type     | Default Value | Required | Description                                         |
| ------------ | -------- | ------------- | :------: | --------------------------------------------------- |
| rendererId   | `number` |               |    ✓     | the id of the renderer                              |
| scrollViewId | `string` |               |    ✓     | the id of the scroll dom node                       |
| props        | `object` |               |    ✓     | Changed context properties compared to the previous |

**props:**

| Property             | Type      | Default Value | Required | Description                                                                                 |
| -------------------- | --------- | ------------- | -------- | ------------------------------------------------------------------------------------------- |
| scrollEnabled        | `boolean` |               |          | enable scroll                                                                               |
| bounces              | `boolean` |               |          | whether the scroll view bounces past the edge of content and back again. (Only work on iOS) |
| showScrollbar        | `boolean` |               |          | whether to show scrollbar.                                                                  |
| pagingEnabled        | `boolean` |               |          | whether paging is enabled for the scroll view                                               |
| fastDeceleration     | `boolean` |               |          | use fast rate of deceleration after the user lifts their finger.                            |
| decelerationDisabled | `boolean` |               |          | disable deceleration (Only work on iOS)                                                     |

## Storage

### `get-storage-info`

Asynchronously acquire the relevant information of the current storage

`response`

```ts
{
  // All keys in the current storage
  keys: string[];

  // Current space occupied (in KB)
  currentSize: number;

  // Space size limit (in KB)
  limitSize: number;
}
```

### `set-storage`

Storing data in the specified key in the local cache will override the original contents corresponding to the key.

Size of single object that is written to the storage is also limited to **1MB**. If worker calls `set-storage` with object which size is bigger than limit, native sends a callback with error, for example `error: "object is exceeding object size limit - object size: 2000000, limit: 1048576"`

The total data stored for each user per MP is limited to **10MB**. If worker calls `set-storage` when this limit is reached, native sends a callback with error, for example `error: "storage is out of size limit - current size: 10485000, limit: 10485760"`.

After receiving error, it's up to mp developer to either do nothing, or perform `remove-storage`/`clear-storage` to free up storage space.

`payload`

| Property | Type     | Default Value | Required | Description                                                                                                   |
| -------- | -------- | ------------- | :------: | ------------------------------------------------------------------------------------------------------------- |
| key      | `string` |               |    ✓     | The specified key in the local cache                                                                          |
| data     | `string` |               |    ✓     | Contents to be stored can only be native types, dates, and objects that can be serialized via JSON.stringify. |

### `get-storage`

Asynchronously acquire the contents of a specified key from the local cache

`payload`

| Property | Type     | Default Value | Required | Description                          |
| -------- | -------- | ------------- | :------: | ------------------------------------ |
| key      | `string` |               |    ✓     | The specified key in the local cache |

`response`

```ts
{
  data: string
}
```

### `remove-storage`

Remove the specified key from the local cache

`payload`

| Property | Type     | Default Value | Required | Description                          |
| -------- | -------- | ------------- | :------: | ------------------------------------ |
| key      | `string` |               |    ✓     | The specified key in the local cache |

### `clear-storage`

Clear local data cache

## File System Manager

### `read-file`

`payload`

| Property | Type     | Default Value | Required | Description                                                                                                                                                                                                       |
| -------- | -------- | ------------- | :------: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filePath | `string` |               |    ✓     | The file path to read.                                                                                                                                                                                            |
| encoding | `string` |               |          | File's character encoding. if not provided, read it in format of ArrayBuffer.                                                                                                                                     |
| position | `number` |               |          | Read file starting from specific position.If not provided, start from file head.The reading range should be left-closed and right-open, [position, position+length). Valid range: [0, fileLength - 1].Unit: byte. |
| length   | `number` |               |          | Specify the length of file. If not provided, read until the end of file.Valid range: [1, fileLength].Unit: byte.                                                                                                  |

Valid value of encoding: ascii, base64, binary, hex, ucs2(read in little endian), ucs-2(read in little endian)，utf16le(read in little endian),utf-16le(read in little endian),utf-8, utf8, latin1.

`response`

| Property | Type                   | Default Value | Required | Description                                                        |
| -------- | ---------------------- | ------------- | :------: | ------------------------------------------------------------------ |
| data     | `string` or `number[]` |               |    ✓     | File content in string or buffer format.number[] is an Uint8Array. |

```ts
{
  data: string | number[] // number[] is an Uint8Array.
}
```

## Framework Storage

Internal storage plugin.

### type

Native will set the clear up strategy of storage based on the `type`.

```js
FrameworkStorageType = 'rendering-cache'
```

### `framework-set-storage`

`payload`

| Property | Type                   | Default Value | Required | Description                                                                                                   |
| -------- | ---------------------- | ------------- | :------: | ------------------------------------------------------------------------------------------------------------- |
| key      | `string`               |               |    ✓     | The specified key in the cache.                                                                               |
| type     | `FrameworkStorageType` |               |    ✓     | The specified type of storage.                                                                                |
| data     | `string`               |               |    ✓     | Contents to be stored can only be native types, dates, and objects that can be serialized via JSON.stringify. |

### `framework-get-storage`

Asynchronously acquire the contents of a specified key and type from the local cache

`payload`

| Property | Type                   | Default Value | Required | Description                     |
| -------- | ---------------------- | ------------- | :------: | ------------------------------- |
| key      | `string`               |               |    ✓     | The specified key in the cache. |
| type     | `FrameworkStorageType` |               |    ✓     | The specified type of storage.  |

`response`

| Property | Type     | Default Value | Required | Description                                                                                                   |
| -------- | -------- | ------------- | :------: | ------------------------------------------------------------------------------------------------------------- |
| data     | `string` |               |    ✓     | Contents to be stored can only be native types, dates, and objects that can be serialized via JSON.stringify. |

```ts
{
  data: string
}
```

### `framework-remove-storage`

Remove the specified key and type from the local cache

`payload`

| Property | Type                   | Default Value | Required | Description                     |
| -------- | ---------------------- | ------------- | :------: | ------------------------------- |
| key      | `string`               |               |    ✓     | The specified key in the cache. |
| type     | `FrameworkStorageType` |               |    ✓     | The specified type of storage.  |

### `framework-clear-storage`

Clear the specified type from the local cache

| Property | Type                   | Default Value | Required | Description                             |
| -------- | ---------------------- | ------------- | :------: | --------------------------------------- |
| type     | `FrameworkStorageType` |               |          | To clear all type without `type` value. |

## SystemInfo

### `get-system-info`

Gets system information.

`response`

| Property        | Type     | Example          | Description                                                               |
| --------------- | -------- | ---------------- | ------------------------------------------------------------------------- |
| brand           | `string` | "Apple" "Xiaomi" | Device brand                                                              |
| model           | `string` |                  | Device model                                                              |
| pixelRatio      | `number` |                  | Device's pixel ratio                                                      |
| screenWidth     | `number` |                  | Screen width in px                                                        |
| screenHeight    | `number` |                  | Screen height in px                                                       |
| windowWidth     | `number` |                  | Available window width in px                                              |
| windowHeight    | `number` |                  | Available window height in px                                             |
| statusBarHeight | `number` |                  | Status bar height in px                                                   |
| language        | `string` |                  | Language set in App                                                       |
| version         | `string` |                  | App version                                                               |
| SDKVersion      | `string` |                  | Base library version for the App version                                  |
| system          | `string` |                  | Operating system and version                                              |
| platform        | `string` | "ios" "android"  | Client platform                                                           |
| safeArea        | `Object` |                  | Safe area when the screen is in vertical orientation platform             |
| theme           | `string` |                  | Current system theme, `light` or `dark` if `"darkmode":true` is provided. |
| host            | `Object` |                  | Host information.                                                         |

`response.safeArea`

| Property | Type     | Description                                                  |
| -------- | -------- | ------------------------------------------------------------ |
| left     | `number` | The x-coordinate of the top-left corner of the safe area     |
| right    | `number` | The x-coordinate of the bottom-right corner of the safe area |
| top      | `number` | The y-coordinate of the top-left corner of the safe area     |
| bottom   | `number` | The y-coordinate of the bottom-right corner of the safe area |
| width    | `number` | Safe area width in logical pixels                            |
| height   | `number` | Safe area height in logical pixels                           |

`response.host`

| Property | Type     | Description            |
| -------- | -------- | ---------------------- |
| appId    | `string` | App id of mini program |

## UpdateManager

### `app-apply-update`

Forces a Mini Program to restart and update to the latest version.

## Location

### `start-location-update`

Starts monitoring real-time position changes in the foreground

### `stop-location-update`

Stops monitoring real-time location changes

### `open-location`

Shows given location using the built-in map.

`payload`

| Property  | Type     | Default Value | Required | Description                                                                                                                                                                             |
| --------- | -------- | ------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| latitude  | `number` | -             | ✓        | Latitude. The value ranges from -90 to +90, and the negative number means south latitude. The GCJ-02 coordinate system of the State Bureau of Surveying and Mapping is used. title      |
| longitude | `number` | -             | ✓        | Longitude. The value ranges from -180 to +180, and the negative number means west longitude. The GCJ-02 coordinate system of the State Bureau of Surveying and Mapping is used. content |
| scale     | `number` | `18`          |          | Scale, ranging from 5 to 18                                                                                                                                                             |
| name      | `string` |               |          | Location name                                                                                                                                                                           |
| address   | `string` |               |          | Detailed address                                                                                                                                                                        |

### `get-location`

Gets current geographic location and speed.

`payload`

| Property | Type     | Default Value | Required | Description                                                                                                                                                |
| -------- | -------- | ------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type     | `string` | `wgs84`       |          | GPS coordinates are returned for WGS84, and coordinates used for bn.openLocation are returned for GCJ-02.                                                  |
| altitude | `string` | `false`       |          | Altitude information is returned if true is passed. The API will take a longer time to respond since a higher accuracy is required to obtain the altitude. |

`response`

| Property           | Type     | Description                                                                                  |
| ------------------ | -------- | -------------------------------------------------------------------------------------------- |
| latitude           | `number` | Latitude. The value ranges from -90 to +90, and the negative number means south latitude.    |
| longitude          | `number` | Longitude. The value ranges from -180 to +180, and the negative number means west longitude. |
| speed              | `number` | speed(unit: m/s)                                                                             |
| accuracy           | `number` |
| altitude           | `number` | altitude(unit: m)                                                                            |
| verticalAccuracy   | `number` | Vertical accuracy(unit: m)                                                                   |
| horizontalAccuracy | `number` | Horizontal accuracy(unit: m)                                                                 |

### `choose-location`

Opens the map to allow user to select a location.

`payload`

| Property  | Type     | Default Value | Required | Description              |
| --------- | -------- | ------------- | -------- | ------------------------ |
| latitude  | `number` | -             |          | Latitude of the target.  |
| longitude | `number` | -             |          | Longitude of the target. |

`response`

| Property  | Type     | Description                                                                                                                                                                                                          |
| --------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name      | `string` | Location name brand                                                                                                                                                                                                  |
| address   | `string` | Detailed address platform                                                                                                                                                                                            |
| latitude  | `string` | Latitude, which is expressed by a floating point number ranging from -90 to +90, and the negative number means south latitude. The GCJ-02 coordinate system of the State Bureau of Surveying and Mapping is used.    |
| longitude | `string` | Longitude, which is expressed by a floating point number ranging from -180 to +180, and the negative number means west longitude. The GCJ-02 coordinate system of the State Bureau of Surveying and Mapping is used. |

## UserInfo

### `get-userInfo`

> [2021-10-29 updated] Android return a fake nickname which is not defined on Binance system. iOS isn't implement it. Mark this action as removed. The api can not be used.

Gets user information

`payload`

| Property        | Type      | Default Value | Required | Description                                                                                                                                                                                                                                                                                                                                                                            |
| --------------- | --------- | ------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| withCredentials | `boolean` |               |          | Indicates whether to include login status information. When withCredentials is true, bn.login must be called previously and the login status must be effective. In this case, sensitive information such as encryptedData and iv is returned. When withCredentials is false, the login status is not required, and sensitive information such as encryptedData and iv is not returned. |
| lang            | `string`  | `en`          |          | The language of the displayed user information                                                                                                                                                                                                                                                                                                                                         |

`response`

| Property | Type     | Description                                                                |
| -------- | -------- | -------------------------------------------------------------------------- |
| userInfo | `Object` | User information object, excluding openid and other sensitive information. |

`response.userInfo`

| Property | Type     | Description       |
| -------- | -------- | ----------------- |
| nickname | `string` | The user's alias. |

### `login`

Login with Binance Account.

`payload`

| Property | Type     | Default Value | Required | Description                                                                          |
| -------- | -------- | ------------- | -------- | ------------------------------------------------------------------------------------ |
| state    | `string` | -             |          | The CSRF token to protect against CSRF (cross-site request forgery) attacks.         |
| scope    | `string` | -             | ✓        | List of scopes enum your application requests access to, with comma (`,`) separated. |

`response`

| Property | Type     | Description                                                   |
| -------- | -------- | ------------------------------------------------------------- |
| code     | `string` | The authorization code. For example it is `'0'` on demo page. |
| state    | `string` | `state` is the same as the one in payload.                    |

## Payment

### `request-payment`

Pay with Binance Payment.

`payload`

| Property | Type     | Default Value | Required | Description      |
| -------- | -------- | ------------- | -------- | ---------------- |
| prepayId | `string` | -             | ✓        | Prepaid order ID |

`response`

| Property | Type     | Description                             |
| -------- | -------- | --------------------------------------- |
| status   | `string` | `0` : pay success `others` : pay failed |

## Monitor

### `monitor-upload`

Reports custom analysis data to `es` for internal monitoring.

`payload`

| Property | Type     | Default Value | Required | Description |
| -------- | -------- | ------------- | -------- | ----------- |
| data     | `object` | -             | -        | event data  |

> data interface: <https://docs.fe.devfdg.net/docs/nezha/web-monitor>

### `report-monitor`

Reports monitoring results of custom service data.

`payload`

| Property | Type     | Default Value | Required | Description                                                                                                                       |
| -------- | -------- | ------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------- |
| name     | `string` | -             | ✓        | The monitoring ID, which is obtained after a data indicator is created on the Mini Program admin console.                         |
| value    | `number` | -             | ✓        | Reported value. After values are processed, the sum of reported values per minute is displayed on the Mini Program admin console. |

```json
{
  "name": "1",
  "value": 1
}
```

### `report-event`

Reports monitoring event.

`payload`

| Property | Type     | Default Value | Required | Description                    |
| -------- | -------- | ------------- | -------- | ------------------------------ |
| eventId  | `string` | -             | ✓        | Event name in english.         |
| data     | `string` | -             | ✓        | Reported data in json string . |

```json
{
  "eventId": "eventId",
  "data": "{}"
}
```

### `report-analytics`

Reports custom analysis data. Before using this API, you need to create an event in **Custom Analysis** on the Mini Program admin console, and configure the event name and fields.

`payload`

| Property  | Type     | Default Value | Required | Description                                       |
| --------- | -------- | ------------- | -------- | ------------------------------------------------- |
| eventName | `string` | -             | ✓        | The event name.                                   |
| data      | `object` | -             | ✓        | The reported custom analysis data in json string. |

```json
{
  "eventName": "purchase",
  "data": "{\"price\":120,\"color\":\"red\"}"
}
```

### `report-performance`

Reports performance data. Before using this API, you need to setup on the Mini Program admin console.

`payload`

| Property   | Type              | Default Value | Required | Description        |
| ---------- | ----------------- | ------------- | -------- | ------------------ |
| id         | `number`          | -             | ✓        | id of entry.       |
| value      | `number`          | -             | ✓        | The reported data. |
| dimensions | `string`, `array` | -             |          | Custom dimensions. |

```json
{
  "id": 1101,
  "value": 680,
  "dimensions": "custom"
}
```

## Background fetch

### `get-pre-fetch-data`

Gets the data previously fetched in background during the app launch.

`response`

| Property  | Type     | Default Value | Required | Description                                |
| --------- | -------- | ------------- | -------- | ------------------------------------------ |
| data      | `string` | -             | ✓        | fetched data                               |
| timestamp | `number` | -             | ✓        | timestamp of the fetch                     |
| token     | `string` | -             | ✓        | token that was set on the moment of fetch  |
| lang      | `string` | -             | ✓        | language on the moment of fetch            |
| version   | `string` | -             | ✓        | version of mini app on the moment of fetch |
| query     | `string` | -             | -        | deeplink query                             |
| path      | `string` | -             | -        | deeplink path                              |

### `get-periodic-fetch-data`

Gets the data previously fetched in background using periodic background fetch.

`response`

| Property  | Type     | Default Value | Required | Description                                |
| --------- | -------- | ------------- | -------- | ------------------------------------------ |
| data      | `string` | -             | ✓        | fetched data                               |
| timestamp | `number` | -             | ✓        | timestamp of the fetch                     |
| token     | `string` | -             | ✓        | token that was set on the moment of fetch  |
| lang      | `string` | -             | ✓        | language on the moment of fetch            |
| version   | `string` | -             | ✓        | version of mini app on the moment of fetch |

### `set-background-fetch-token`

Set custom token for identification purposes, which will be sent to both periodic and pre fetch url.

`payload`

| Property | Type     | Default Value | Required | Description  |
| -------- | -------- | ------------- | -------- | ------------ |
| token    | `string` | -             | ✓        | custom token |

## Interface

### Request Method

```js
type Method = 'OPTIONS' | 'HEAD' | 'GET' | 'POST' | 'PUT' | 'DELETE'
```

### Request Response

```js
interface Response {
  headers: object // http response header
  status: number // The status code of the response. (This will be 200 for a success).
  statusText : string // The status message corresponding to the status code. (e.g., OK for 200).
  url: string // the URL of the response.
  data: string // resource content
}
```

### Settings

```ts
interface Settings {
  authSetting: AuthSettings
}

interface AuthSettings {
  'scope.userLocation': bool
  'scope.writePhotosAlbum': bool
  'scope.camera': bool
  ...
}
```

## Clipboard

### `set-clipboard-data`

Set the contents of the system clipboard.

`payload`

| Property | Type     | Default Value | Required | Description       |
| -------- | -------- | ------------- | :------: | ----------------- |
| data     | `string` |               |    ✓     | Clipboard content |

### `get-clipboard-data`

Get the contents of the system clipboard.

`payload`

| Property | Type | Default Value | Required | Description |
| -------- | ---- | ------------- | :------: | ----------- |

`response`

```ts
{
  data: string
}
```

## ScanCode

### `scan-code`

Tun up the client scan code interface for scanning code.

`payload`

| Property | Type             | Default Value | Required | Description    |
| -------- | ---------------- | ------------- | :------: | -------------- |
| scanType | `Array.<string>` | ['qrCode']    |    -     | Scan code type |

`response`

| Property | Type     | Default Value | Required | Description                 |
| -------- | -------- | ------------- | :------: | --------------------------- |
| result   | `string` |               |    -     | the content of scanned code |
| scanType | `string` |               |    -     | the type of scanned code    |

## `__mp_private_api__`

### `private-report-event`

Reports monitoring event.

`payload`

| Property | Type         | Default Value | Required | Description                    |
| -------- | ------------ | ------------- | -------- | ------------------------------ |
| data     | `jsonString` | -             | ✓        | Reported data in json string . |

```json
{
  "data": "{\"event\":\"click\"}"
}
```

Reports monitoring .

### `private-kyc-show-dialog`

Show KYC dialog and start KYC process.

`payload`

| Property | Type     | Example                                         | Required | Description                    |
| -------- | -------- | ----------------------------------------------- | :------: | ------------------------------ |
| code     | `string` | "200003903" "200003904" "200003905" "200003906" |    ✓     | error code from server side    |
| message  | `string` |                                                 |    ✓     | error message from server side |

### `two-fa`

Get the scene code information.

`payload`

| Property | Type     | Default Value | Required | Description |
| -------- | -------- | ------------- | -------- | ----------- |
| scene    | `string` | -             | ✓        | scene code  |

```json
{ "scene": "mobile" }
```

`response`

| Property         | Type     | Default Value | Required | Description |
| ---------------- | -------- | ------------- | -------- | ----------- |
| emailVerifyCode  | `string` | -             | -        |             |
| googleVerifyCode | `string` | -             | -        |             |
| mobileVerifyCode | `string` | -             | -        |             |

### `private-get-user-profile`

Get user profile.

`response`

| Property | Type     | Default Value | Required | Description |
| -------- | -------- | ------------- | -------- | ----------- |
| uid      | `string` | -             | ✓        | user_id     |

### `private-get-app-settings`

Get app settings.

`response`

| Property        | Type     | Default Value | Required | Description      |
| --------------- | -------- | ------------- | -------- | ---------------- |
| currency        | `string` | -             | ✓        | currency         |
| paymentCurrency | `string` | -             | ✓        | payment currency |

## Framework

### `framework-load-script`

Load the script to worker (e.g: /path/to/page.worker.js)

`payload`

| Property | Type     | Default Value | Required | Description             |
| -------- | -------- | ------------- | :------: | ----------------------- |
| path     | `string` |               |    ✓     | absulute path of script |

## Media

### `choose-image`

Selects an image from the local album or takes a photo with the camera.

`payload`

| Property   | Type             | Default Value              | Required | Description                          |
| ---------- | ---------------- | -------------------------- | :------: | ------------------------------------ |
| count      | `number`         | 9                          |    no    | The maximum number of images allowed |
| sizeType   | `Array.<string>` | ['original', 'compressed'] |    no    | The size of the select image         |
| sourceType | `Array.<string>` | ['album', 'camera']        |    no    | The source of the image              |

#### Valid values of object.sizeType

| Value      | Description      |
| ---------- | ---------------- |
| original   | Original image   |
| compressed | Compressed image |

#### Valid values of object.sourceType

| Value  | Description                     |
| ------ | ------------------------------- |
| album  | Selects an image from the album |
| camera | Takes a photo with the camera   |

`response`

| Property      | Type             | Description                                      |
| ------------- | ---------------- | ------------------------------------------------ |
| tempFilePaths | `Array.<string>` | The list of local temporary file paths to images |
| tempFiles     | `Array.<Object>` | The local temporary file list for images         |

#### res.tempFiles is composed as follows

| Property | Type   | Description                                  |
| -------- | ------ | -------------------------------------------- |
| path     | string | The path to the local temporary file         |
| size     | number | The size of a local temporary file, in bytes |
