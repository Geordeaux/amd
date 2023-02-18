---
id: rpc-action-upload-file
title: Upload File
sidebar_label: Upload File
---


## Overview

Download File

## Actions

RPC actions and events that are related to upload task.

### Action: `upload-start`

Download file resources to the local. The client directly initiates an HTTPS GET request and returns the local temporary path (local path) of the file. 

`payload`

| Property |   Type   | Default Value | Required |                                       Description                                       |
| -------- | -------- | ------------- | -------- | --------------------------------------------------------------------------------------- |
| url      | `string` |               | ✓        | The HTTPS API URL of developer server                                                   |
| filePath | `string` |               | ✓        | The path of the file resource to be uploaded (local path)                               |
| name     | `string` |               | ✓        | The key corresponding to the file, the server can get the file content through this key |
| header   | `Object` |               |          | Header of HTTP request, Referer cannot be set in Header                                 |
| formData | `Object` |               |          | Other additional form data in the HTTP request                                          |
| timeout  | `number` | `10000`       |          | timeout of request, unit is **millisecond**                                             |


`response`

| Property |   Type   |                Description                |
| -------- | -------- | ----------------------------------------- |
| id       | `string` | The id of created Download Task instance. |

### Action: `upload-abort`

Abort upload task

`payload`

| Property |   Type   | Default Value | Required |                Description                |
| -------- | -------- | ------------- | -------- | ----------------------------------------- |
| id       | `string` | -             | ✓        | The id of created Download Task instance. |


## Events

### Event: `upload-event`

| Property |                  Type                  | Default Value | Required |                Description                |
| -------- | -------------------------------------- | ------------- | -------- | ----------------------------------------- |
| id       | `string`                               |               | ✓        | The id of created HTTPS Request instance. |
| name     | headersReceived,progress,success,error |               | ✓        | Name of the event.                        |
| payload  | `object`                               |               | ✓        | Payload of the event.                     |

Event: `headersReceived`

Fired when server responds headers.

```json
{
  "id": "string",
  "name": "headersReceived",
  "payload": {
    "statusCode": 200,
    "header": {
      "Content-Type": "application/json",
      "Set-Cookie": "key1=value1; path=/; httponly, key2=value2; path=/; httponly",
    } // Connected HTTP response header
  }
}
```

Event: `progress`

Fired when upload progress updated.

```json
{
  "id": "string",
  "name": "progress",
  "payload": {
    "progress": 50,
    "totalBytesSent": 5000,
    "totalBytesExpectedToSend": 10000
  }
}
```

`payload`

|         Property         |  Type  |                        Description                         |
| ------------------------ | ------ | ---------------------------------------------------------- |
| progress                 | number | The percentage of upload progress                          |
| totalBytesSent           | number | The length of the uploaded data, in Bytes                  |
| totalBytesExpectedToSend | number | The total length of data expected to be uploaded, in Bytes |

Event: `success`

Fired when upload finished.

```json
{
  "id": "string",
  "name": "success",
  "payload": {
    "statusCode": 200,
    "data": "string",
    "profile":{}
  }
}
```

`payload`

|  Property  |   Type   |    Description     |
| ---------- | -------- | ------------------ |
| statusCode | `number` | HTTP status code   |
| data       | `string` | HTTP Response Body |

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
