---
id: rpc-action-download-file
title: Download File
sidebar_label: Download File
---

## Overview

Download File

## Actions

RPC actions and events that are related to download task.

### Action: `download-start`

Download file resources to the local. The client directly initiates an HTTPS GET request and returns the local temporary path (local path) of the file. 

`payload`

| Property |   Type   | Default Value | Required |                               Description                                |
| -------- | -------- | ------------- | -------- | ------------------------------------------------------------------------ |
| url      | `string` |               | ✓        | The HTTPS API URL of developer server                                    |
| header   | `Object` |               |          | Header of HTTP request, Referer cannot be set in Header                  |
| timeout  | `number` | `10000`       |          | timeout of request, unit is **millisecond**                              |
| filePath | `string` |               |          | Specify the path where the file is stored after downloading (local path) |


`response`

| Property |   Type   |                Description                |
| -------- | -------- | ----------------------------------------- |
| id       | `string` | The id of created Download Task instance. |

### Action: `download-abort`

Abort download task

`payload`

| Property |   Type   | Default Value | Required |                Description                |
| -------- | -------- | ------------- | -------- | ----------------------------------------- |
| id       | `string` | -             | ✓        | The id of created Download Task instance. |


## Events

### Event: `download-event`

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

Fired when download progress updated.

```json
{
  "id": "string",
  "name": "progress",
  "payload": {
    "progress": 50,
    "totalBytesWritten": 5000,
    "totalBytesExpectedToWrite": 10000
  }
}
```

`payload`

|         Property          |  Type  |                         Description                          |
| ------------------------- | ------ | ------------------------------------------------------------ |
| progress                  | number | The percentage of download progress                          |
| totalBytesWritten         | number | The length of the downloaded data, in Bytes                  |
| totalBytesExpectedToWrite | number | The total length of data expected to be downloaded, in Bytes |

Event: `success`

Fired when download finished.

```json
{
  "id": "string",
  "name": "success",
  "payload": {
    "statusCode": 200,
    "tempFilePath": "string",
    "profile":{}
  }
}
```

`payload`

|   Property   |  Type  |                      Description                      |
| ------------ | ------ | ----------------------------------------------------- |
| tempFilePath | string | Temporary file path (local path).                     |
| filePath     | string | User file path (local path).                          |
| statusCode   | number | HTTP status code                                      |
| profile      | Object | Some debugging information during the network request |


`profile`

All Property in `profile` are optional, and only need to be put into the `profile` when the network library supports to provide relevant information. 

|             Property             |  Type   |                                                    Description                                                     |
| -------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------ |
| redirectStart                    | number  | 第一个 HTTP 重定向发生时的时间。有跳转且是同域名内的重定向才算，否则值为 0                                         |
| redirectEnd                      | number  | 最后一个 HTTP 重定向完成时的时间。有跳转且是同域名内部的重定向才算，否则值为 0                                     |
| fetchStart                       | number  | 组件准备好使用 HTTP 请求抓取资源的时间，这发生在检查本地缓存之前                                                   |
| domainLookupStart                | number  | DNS 域名查询开始的时间，如果使用了本地缓存（即无 DNS 查询）或持久连接，则与 fetchStart 值相等                      |
| domainLookupEnd                  | number  | DNS 域名查询完成的时间，如果使用了本地缓存（即无 DNS 查询）或持久连接，则与 fetchStart 值相等                      |
| connectStart                     | number  | HTTP（TCP） 开始建立连接的时间，如果是持久连接，则与 fetchStart 值相等。注意如果在传输层发生了错误且重新建立连接，则这里显示的是新建立的连接开始的时间                                                                     |
| connectEnd                       | number  | HTTP（TCP） 完成建立连接的时间（完成握手），如果是持久连接，则与 fetchStart 值相等。注意如果在传输层发生了错误且重新建立连接，则这里显示的是新建立的连接完成的时间。注意这里握手结束，包括安全连接建立完成、SOCKS 授权通过 |
| SSLconnectionStart               | number  | SSL建立连接的时间,如果不是安全连接,则值为 0                                                                        |
| SSLconnectionEnd                 | number  | SSL建立完成的时间,如果不是安全连接,则值为 0                                                                        |
| requestStart                     | number  | HTTP请求读取真实文档开始的时间（完成建立连接），包括从本地读取缓存。连接错误重连时，这里显示的也是新建立连接的时间 |
| requestEnd                       | number  | HTTP请求读取真实文档结束的时间                                                                                     |
| responseStart                    | number  | HTTP 开始接收响应的时间（获取到第一个字节），包括从本地读取缓存                                                    |
| responseEnd                      | number  | HTTP 响应全部接收完成的时间（获取到最后一个字节），包括从本地读取缓存                                              |
| rtt                              | number  | 当次请求连接过程中实时 rtt                                                                                         |
| estimate_nettype                 | string  | 评估的网络状态 slow 2g/2g/3g/4g                                                                                    |
| httpRttEstimate                  | number  | 协议层根据多个请求评估当前网络的 rtt（仅供参考）                                                                   |
| transportRttEstimate             | number  | 传输层根据多个请求评估的当前网络的 rtt（仅供参考）                                                                 |
| downstreamThroughputKbpsEstimate | number  | 评估当前网络下载的kbps                                                                                             |
| throughputKbps                   | number  | 当前网络的实际下载kbps                                                                                             |
| peerIP                           | string  | 当前请求的IP                                                                                                       |
| port                             | number  | 当前请求的端口                                                                                                     |
| socketReused                     | boolean | 是否复用连接                                                                                                       |
| sendBytesCount                   | number  | 发送的字节数                                                                                                       |
| receivedBytesCount               | number  | 收到字节数                                                                                                         |
| protocol                         | string  | 使用协议类型，有效值：http1.1, h2, quic, unknown                                                                   |

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
