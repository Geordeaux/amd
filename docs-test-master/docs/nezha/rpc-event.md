---
id: rpc-event
title: Event
sidebar_label: Event
---

import Mermaid from '@theme/Mermaid'

## Overview

An `Event` is only allowed to send from `Native` to `Worker`. `Worker` should **never** reply an `Event`.

<Mermaid chart={`graph LR Native-->|Event|Worker`}></Mermaid>

`payload` is always **required**. if it's not been defined, it should be an empty object.

## App lifecycle events

### `app-launch`

Triggered once globally when Mini Program initialization is completed.

`payload`

| Property     | Type     | Description                                                                                                                                                    |
| ------------ | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| path         | `string` | current route path                                                                                                                                             |
| scene        | `number` | The scene value for switching the Mini Program to foreground                                                                                                   |
| query        | `string` | parsed query object in json string                                                                                                                             |
| rendererId   | `number` | the id of the renderer                                                                                                                                         |
| startTime    | `number` | time when user open the mini app for performance reports.                                                                                                      |
| referrerInfo | `Object` | The source information. This is returned when a user enters the Mini Program from another Mini Program, Official Account, or app. Otherwise, `{}` is returned. |

### `app-enter-foreground`

Triggered when the Mini Program is switched from the background to the foreground.

`payload`

| Property     | Type     | Description                                                                                                                                                    |
| ------------ | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| path         | `string` | current route path                                                                                                                                             |
| scene        | `number` | The scene value for switching the Mini Program to foreground                                                                                                   |
| query        | `string` | parsed query object in json string                                                                                                                             |
| rendererId   | `number` | the id of the renderer                                                                                                                                         |
| startTime    | `number` | time when user open the mini app for performance reports.                                                                                                      |
| referrerInfo | `Object` | The source information. This is returned when a user enters the Mini Program from another Mini Program, Official Account, or app. Otherwise, `{}` is returned. |

#### referrerInfo is composed as follows

| Property  | Type     | Description                                                                          |
| --------- | -------- | ------------------------------------------------------------------------------------ |
| appId     | `string` | The appId of the source Mini Program, Official Account, or app.                      |
| extraData | `string` | The [encoded Base64URL string](https://base64.guru/standards/base64url/encode) data. |

#### Scenes that Return Valid referrerInfo

| Scene Value | Scene                                    | Meaning of appId     |
| ----------- | ---------------------------------------- | -------------------- |
| 1037        | Mini Program opened from a Mini Program. | Source Mini Program. |
| 1038        | Returned from another Mini Program.      | Source Mini Program. |

### `app-enter-background`

Triggered when the Mini Program switches from the foreground to the background.

### `app-error`

Fired whenever an error occurs

`payload`

| Property | Type     | Description |
| -------- | -------- | ----------- |
| error    | `string` | err message |

## Navigation events

### `route-change`

Fired when the page stacks change

`payload`

| Property   | Type     | Description                                                                             |
| ---------- | -------- | --------------------------------------------------------------------------------------- |
| type       | `string` | available types: “appLaunch”, "navigateBack" , "navigateTo", "switchTab" , “redirectTo” |
| path       | `string` | current route path                                                                      |
| query      | `string` | parsed query object in json string                                                      |
| payload    | `Object` | if route-change triggered by switch-tab, the `tabItem info` will be passed as payload   |
| rendererId | `number` | the id of the renderer                                                                  |
| startTime  | `number` | time when `route-start` for performance reports.                                        |

`tabItem info`

| Property | Type   | Description                               |
| -------- | ------ | ----------------------------------------- |
| index    | number | the index of the tab item starting from 0 |
| pagePath | String | the path of the tab item                  |
| text     | String | the text value of the tab item            |

## Keyboard

### `keyboard-height-change`

Fired when keyboard height changes

`payload`

| Property | Type     | Description     |
| -------- | -------- | --------------- |
| height   | `number` | keyboard height |

## UI events

### `pull-down-refresh-started`

Fired when user pulls down and release pull-down-to-refresh control. Only sent if `enablePullDownRefresh` is enabled (see [window configuration](./config-global#window)).

`payload`

| Property   | Type     | Description            |
| ---------- | -------- | ---------------------- |
| rendererId | `number` | the id of the renderer |

### `theme-change`

Fired whenever system theme changes

`payload`

| Property | Type              | Description   |
| -------- | ----------------- | ------------- |
| theme    | `light` or `dark` | current theme |

## UpdateManager

### `app-check-for-update`

Fired when a request for checking for updates is received from the Native backend. Native automatically checks for updates when the Mini program cold starts. The developer does not need to trigger this method.

`payload`

| Property  | Type      | Description                                  |
| --------- | --------- | -------------------------------------------- |
| hasUpdate | `boolean` | Indicates whether a new version is available |

### `app-update-failed`

Fired when Mini Program update failed. The app, instead of the developer, triggers the download when a newer version is available. A event is sent when the download fails (probably due to network problems).

### `app-update-ready`

Fired when a newer Mini Program version is available. The app, instead of the developer, triggers the download. A event is performed after successful download.

## Location

### `location-change`

monitor real-time geographic location change events

`payload`

| Property           | Type     | Description          |
| ------------------ | -------- | -------------------- |
| latitude           | `number` | range -90 ~ 90       |
| latitude           | `number` | range -180 ~ 180     |
| speed              | `number` | unit m/s             |
| accuracy           | `number` | accuracy of location |
| altitude           | `number` | unit m               |
| verticalAccuracy   | `number` | unit m               |
| horizontalAccuracy | `number` | unit m               |

## Network

### `request-event`

HTTP request events, check [Request events](./rpc-action-request#action-request-event) for more information.

### `download-event`

Download task events, check [Download events](./rpc-action-download-file#action-download-event) for more information.

### `upload-event`

Upload task events, check [Upload events](./rpc-action-upload-file#action-upload-event) for more information.

### `network-status-change`

Fired when network status is changed.

`payload`

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

## Web Socket

### `websocket-event`

Web socket events, check [WebSocket events](./websocket#event-websocket-event) for more information.

## Performance

### `memory-warning`

Listens on the insufficient memory alarm event.

This event is triggered when iOS/Android sends an insufficient memory alarm to the Mini Program process. Triggering this event does not mean that the Mini Program will be terminated. In most cases, it is only an alarm. Developers can reclaim some unnecessary resources after receiving the alarm to free up memory space.

`payload`

| Property | Type     | Description                                                                                   |
| -------- | -------- | --------------------------------------------------------------------------------------------- |
| level    | `number` | Memory alarm level, only available in Android. It corresponds to the system macro definition. |

`Valid values of level`

| Value | Description                  |
| ----- | ---------------------------- |
| 5     | TRIM_MEMORY_RUNNING_MODERATE |
| 10    | TRIM_MEMORY_RUNNING_LOW      |
| 15    | TRIM_MEMORY_RUNNING_CRITICAL |

## Media

### `audio-interruption-begin`

Listen audio is interrupted by system occupation. Start event. The following scenarios trigger this event: alarm clock, phone, FaceTime Call, WeChat voice chat, WeChat video chat. When this event is triggered, all audio in the Mini Program will pause.

`payload`

| Property | Type | Description |
| -------- | ---- | ----------- |

### `audio-interruption-end`

Listen for audio interrupt end event. Received. onAudioInterruptionBegin After the event, all audio in the Mini Program will be suspended and can be played again after receiving this event

`payload`

| Property | Type | Description |
| -------- | ---- | ----------- |

## Page Lifecycle

### Push

1. hide - current renderer
2. show - new renderer

### Pop

1. unload - current renderer
2. show - previous renderer

### Redirect

1. unload - current renderer
2. show - new renderer
