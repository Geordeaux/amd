---
id: web-view
title: WebView
sidebar_label: WebView
---

## Overview

Container that carries webpages. It automatically fills the entire Mini Program page.

### Protocols

RPC actions and events that are related to WebView.

#### Action: `show-webview`

`payload`

| Property   | Type     | Default Value | Required | Description             |
| ---------- | -------- | ------------- | -------- | ----------------------- |
| rendererId | `number` |               | ✓        | The id of renderer.     |
| src        | `string` |               | ✓        | The source of web view. |

`response`

| Property  | Type     | Example                      | Description                 |
| --------- | -------- | ---------------------------- | --------------------------- |
| webviewId | `number` |                              | Instance id of the web view |

#### Action: `update-webview`

| Property   | Type     | Default Value | Required | Description                      |
| ---------- | -------- | ------------- | -------- | -------------------------------- |
| rendererId | `number` |               | ✓        | The id of renderer.              |
| webviewId  | `number` |               | ✓        | Instance id of the web view.     |
| src        | `string` |               | ✓        | The source of web view.          |

#### Action: `hide-webview`

| Property   | Type     | Default Value | Required | Description                  |
| ---------- | -------- | ------------- | -------- | ---------------------------- |
| rendererId | `number` |               | ✓        | The id of renderer.          |
| webviewId  | `number` |               | ✓        | Instance id of the web view. |

#### Event: `event-webview`

| Property   | Type              | Default Value | Required | Description                  |
| ---------- | ----------------- | ------------- | -------- | ---------------------------- |
| rendererId | `number`          |               | ✓        | The id of renderer.          |
| webviewId  | `number`          |               | ✓        | Instance id of the web view. |
| name       | `load` \| `error` |               | ✓        | Name of the event.           |
| data       | `object`          |               | ✓        | Data of the event.           |

Event: `load`

Fired when the navigation is done.

```json
{
  "rendererId": 0,
  "webviewId": 0,
  "name": "load",
  "data": {
    "src": "https://www.binance.com/en"
  }
}
```

Event: `error`

Fired when the load failed or was cancelled.

```json
{
  "rendererId": 0,
  "webviewId": 0,
  "name": "error",
  "data": {
    "src": "https://www.binance.com/en",
    "error": "ERR_FILE_NOT_FOUND"
  }
}
```
