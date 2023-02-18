---
id: websocket
title: WebSocket
sidebar_label: WebSocket
---

## Overview

WebSocket communication.

## Actions

RPC actions and events that are related to WebSocket.

### Action: `websocket-create`

Creates a WebSocket connection.

`payload`

| Property  | Type       | Default Value | Required | Description                                                                                    |
| --------- | ---------- | ------------- | -------- | ---------------------------------------------------------------------------------------------- |
| url       | `string`   | -             | ✓        | The wss API URL of developer server                                                            |
| headers   | `object`   | -             |          | set request header, can't set `Referer`. default value of `content-type` is `application/json` |
| protocols | `string[]` | -             |          | Sub-protocol array.                                                                            |

`response`

| Property | Type     | Description                           |
| -------- | -------- | ------------------------------------- |
| id       | `string` | The id of created WebSocket instance. |

#### Notes:

- Up to 5 WebSocket connections can exist at the same time. If new connection exceed the limit, \
then the oldest connection will be disconnected.

- All existing web socket connections will be disconnected, when application enters background.

### Action: `websocket-send`

Sends data over a WebSocket connection.

`payload`

| Property | Type                      | Default Value | Required | Description                                      |
| -------- | ------------------------- | ------------- | -------- | ------------------------------------------------ |
| id       | `string`                  | -             | ✓        | The id of created WebSocket instance.            |
| type     | `'text'`, `'arraybuffer'` | -             | ✓        | The type of data.                                |
| data     | `string/number[]`         | -             | ✓        | The data to be sent. `number[]` is an Uint8Array |

### Action: `websocket-close`

Disables the WebSocket connection.

`payload`

| Property | Type     | Default Value                                                | Required | Description                                                                                                                                                |
| -------- | -------- | ------------------------------------------------------------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id       | `string` | -                                                            | ✓        | The id of created WebSocket instance.                                                                                                                      |
| code     | `number` | `1000` (indicating that the connection is disabled normally) |          | A numeric value indicates the status code explaining why the connection has been disabled.                                                                 |
| reason   | `string` | -                                                            |          | A readable string explaining why the connection has been disabled. This string must be a UTF-8-encoded text (not characters) with not more than 123 bytes. |

## Events

### Event: `websocket-event`

| Property | Type                        | Default Value | Required | Description                           |
| -------- | --------------------------- | ------------- | -------- | ------------------------------------- |
| id       | `string`                    |               | ✓        | The id of created WebSocket instance. |
| name     | open, close, error, message |               | ✓        | Name of the event.                    |
| payload  | `object`                    |               | ✓        | Payload of the event.                 |

Event: `open`

Fired when enabling the WebSocket connection.

```json
{
  "id": "string",
  "name": "open",
  "payload": {
    "headers": {} // Connected HTTP response header
  }
}
```

Event: `close`

Fired when disabling the WebSocket connection.

```json
{
  "id": "string",
  "name": "close",
  "payload": {
    "code": 1006,
    "reason": "string"
  }
}
```

Event: `error`

Fired when errors.

```json
{
  "id": "string",
  "name": "error",
  "payload": {
    "errMsg": "string" // Error message
  }
}
```

Event: `message`

Fired when receiving string data from server messages.

```json
{
  "id": "string",
  "name": "message",
  "payload": {
    "type": "text",
    "data": "string" // Messages returned by the server
  }
}
```

Fired when receiving ArrayBuffer data from server messages.

```json
{
  "id": "string",
  "name": "message",
  "payload": {
    "type": "arraybuffer",
    "data": [10, 0, 0, 0, 0, 0, 0, 0] // Uint8Array
  }
}
```
