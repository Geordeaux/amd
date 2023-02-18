---
id: api-native-list
title: Native API List
sidebar_label: API List
---

## Network

### mp.request

launch http network request

#### params

| property | type | defaultValue | required | note |
| :---: | :---: | :---: | :---: | :---: |
| url | string | - | true | request url |
| data | object | - | false | request data |
| header | object | - | false | set request header, can't set `Referer`. default value of `content-type` is `application/json` |
| timeout | number | 10000 | false | timeout of request, unit is **millisecond** |
| method | RequestMethod | GET | false | request method |

#### params.method

```ts
type RequestMethod = 'OPTIONS' | 'HEAD' | 'GET' | 'POST' | 'PUT' | 'DELETE'
```

#### response

```ts
interface Response {
  data: any // data from developer server
  statusCode: number // http status code
  header: object // http response header
}
```

#### NativeRPCMessage.payload

```ts
{
  url: string
  data?: object
  header?: object
  timeout?: number
  method?: RequestMethod
}
```

#### demo code

```ts
mp.request({
  url: 'https://xxx.com/yyy',
  data: {
    x: '',
    y: ''
  },
  header: {
    'content-type': 'application/json',
  },
  success (res) {
    console.log(res.data.data)
    console.log(res.data.headers)
  }
})
```

## UI

### mp.showToast

Displays the message prompt box

### mp.hideToast

Hides the message prompt box.

### mp.showModal

Displays the modal dialog box.

### mp.showLoading

Displays the loading prompt box. hideLoading must be called to close the prompt box.

### mp.hideLoading

Hides the loading prompt box

## Storage

### mp.getStorageInfo

Asynchronously acquire the relevant information of the current storage

### mp.setStorage

Storing data in the specified key in the local cache will override the original contents corresponding to the key.

### mp.getStorage

Asynchronously acquire the contents of a specified key from the local cache

### mp.removeStorage

Remove the specified key from the local cache

### mp.clearStorage

Clear local data cache

## Log

### mp.log

## SystemInfo

### mp.getSystemInfo

Gets system information.

## Clipboard

### mp.copy

### mp.paste

## UpdateManager

### mp.applyUpdate

Forces a Mini Program to restart and update to the latest version. This API is called after the new Mini Program version is downloaded (i.e., when the onUpdateReady callback is received).

### mp.onCheckForUpdate

Listens on the event that a request for checking for updates is sent to the Native backend. Native automatically checks for updates when the Mini program cold starts. The developer does not need to trigger this method.

### mp.onUpdateFailed

Listens on Mini Program update failure event. The app, instead of the developer, triggers the download when a newer version is available. A callback is performed when the download fails (probably due to network problems).

### mp.onUpdateReady

Listens on the event that a newer Mini Program version is available. The app, instead of the developer, triggers the download. A callback is performed after successful download.

## Location

### mp.startLocationUpdate

开启小程序进入前台时接收位置消息
start monitoring real-time position changes in the foreground

### mp.stopLocationUpdate

关闭监听实时位置变化，前后台都停止消息接收
stop monitoring real-time location changes

### mp.onLocationChange

监听实时地理位置变化事件
monitor real-time geographic location change events

### mp.offLocationChange

取消监听实时地理位置变化事件
cancel monitoring of real-time location change events

### mp.openLocation

Views location using the built-in map.

### mp.getLocation

Gets current geographic location and speed.

### mp.chooseLocation

Opens the map to select a location

## UserInfo

### mp.getUserInfo

Gets user information
