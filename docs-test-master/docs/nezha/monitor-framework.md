---
id: monitor-framework
title: Framework Monitor
sidebar_label: Framework Monitor
---

framework monitor is used to do event reporting, performance monitoring and error monitoring at the framework level , used to implement the monitoring system at the framework level.

## Overall process

![monitor events](https://static.devfdg.net/image/julia/docs/mini-program/monitor-events.png)

## Upload data structure

```javascript
{
    /* Device Part */
    "os": string, // system version
    "brand": string, // device brand

    /* network part */
    "operator": "中国电信", // network name

    /* app part */
    "session": string, // session id of binance app
    "buid": string, // device id    

    /* package part */
    "root": string, // jailbreaked or rooted
    "client": string,  // client-type
    "version": string, // versionCode
    
    /* other part */
    "sVersion": string // fixed, the version of the schema
	  "browser": string // empty
	  "platform": string // empty

    /* Mini Program Part */
    "data": [{
        "t": string // monitor type
        "ty"?: string // optional, monitor sub type
        "v": string // version of mini program app
        "rv": string // revision
        "id": string // appId
        "d": number // timestamp, event reporting time
        "cv": string // common js version / sdk version

        "p"?: string // optional, page name
        "pid"?: string // optional, process id, including：page_id, component_id, renderer_id
        "sc"?: string // scene value
        "ses"?: string // mp session Id , it starts from the cold start, ends when close the mp
        "st"?: number // optional, start time of a procedure
        "et"?: number // optional, end time of a procedure
        "dt"?: number // optional, the duration of a procedure
        "rc"?: number // optional, result code SUCCESS(0) | CANCEL(2) | ERROR(1)
        "e"?: string // optional, error info
        "extra": {
            "cb" ?: boolean // cold boost
            "uc" ?: boolean // use cache
            "nu" ?: boolean // need update
            "url" ?: string // request url 
            "req" ?: string // request head
            "network-type" ?: string // the network type 
        }
    }]
}
```

Every event upload to the backend will have such structure. Event have a wrapping part, which will be the same info to describe the environment of the current runtime, such as the device info (`bnc-uuid`) of the network status (`network`, `network-type`).

And each event will include many data structure, each one is an event data report by the mini program framework. These data are collected on the native side, within 1 minute, of the events reported by the framework, and they are packaged and uploaded.

The data will be uploaded directly to the Big Data Backend. Some of the data will be filled by the backend, such as the `request_ip` and `region`

## Basic Data

The basic data is the common part of the event structure, which reported by the mini program framework. Event event should first fillin the field to the data as following:

```js
{
    "t": string // monitor type
    "v": string // version of mini program app
    "rv": string // revision
    "id": string // appId
    "d": number // timestamp, event reporting time
    "cv": string // common js version / sdk version
}
```

And for the events that report the errors, should also fillin the field `e` :

```js
{
  "e": string // error info
}
```

## Events Rule

**>>Event Title:** *NEZHA[_ERR]_[EVENT_NAME]*

EVENT_NAME try to use `noun` to represent the event, or `verb+object` to express the event stage

For those with multiple events, we split them into multiple phases and report them separately, and splice them by the backend statistics.

For example, for downloading multiple resources, we split the event into two events `FETCH_DETAIL`, `DOWNLOAD_BUNDLE`.

If the events cross multiple sides, it is required to put the name of the platform. Such as page rendering process, `CREATE_RENDERER`, `INJECT_RENDERER_JS`, `INIT_RENDERER_JS`, `RENDER_PAGE_JS`.

If an event has multiple steps, we use pid to concatenate them ( process id )
(in the specific process can be different multiple steps, such as `renderer_id` for rendering page, `component_id` for rendering native component)

**>>Error definition:** Any event with druation should have a corresponding error with the name *NEZHA_ERR_[EVENT_NAME]*

All errors will have parameter `e` of type string, which records the error details. It should be noted that the scope of the collectable must not have privacy issues.

## Events

**Note:** Each event will include the common part of the basic data.

### Resource Download Time

fetch the datail api

| key   | type   | description                                       |
|-------|--------|---------------------------------------------------|
| t     | string | NEZHA_FETCH_DETAIL                                |
| tp    | string | INITIAL, UPDATE, PREFETCH                         |
| st    | number | start time                                        |
| et    | number | end time                                          |
| dt    | number | duration                                          |
| pid   | string | used for the PREFETCH procedure                   |
| extra | object | {url: string, nu: boolean, network-type: string } |
| rc    | number | result code : SUCCESS(0), CANCEL(2), ERROR(1)     |

download the bundle file

| key   | type   | description                                   |
|-------|--------|-----------------------------------------------|
| t     | string | NEZHA_DOWNLOAD_BUNDLE                         |
| st    | number | start time                                    |
| et    | number | end time                                      |
| dt    | number | duration                                      |
| pid   | string | used for the PREFETCH procedure               |
| extra | object | {url: string, network-type: string }          |
| rc    | number | result code : SUCCESS(0), CANCEL(2), ERROR(1) |

fetch the datail api failed

| key   | type   | description            |
|-------|--------|------------------------|
| t     | string | NEZHA_ERR_FETCH_DETAIL |
| e     | string | error info             |
| extra | object | {url: string }         |

download the bundle file failed

| key   | type   | description               |
|-------|--------|---------------------------|
| t     | string | NEZHA_ERR_DOWNLOAD_BUNDLE |
| e     | string | error info                |
| extra | object | {url: string }            |

### Page View (PV)

The amount of creating the renderer.

| key | type   | description |
|-----|--------|-------------|
| t   | string | NEZHA_PV    |
| p   | string | page name   |

### Page Render Time

Page rendering process, from native starting the `route` process, to renderer finishing rendering.

start creating the renderer ( native )

| key | type   | description                                   |
|-----|--------|-----------------------------------------------|
| t   | string | NEZHA_CREATE_RENDERER                         |
| st  | number | start time                                    |
| et  | number | end time                                      |
| dt  | number | duration                                      |
| p   | string | page name                                     |
| pid | string | used for the PREFETCH procedure               |
| rc  | number | result code : SUCCESS(0), CANCEL(2), ERROR(1) |

start injecting the renderer ( native )

| key | type   | description                                   |
|-----|--------|-----------------------------------------------|
| t   | string | NEZHA_INJECT_RENDERER_JS                      |
| st  | number | start time                                    |
| et  | number | end time                                      |
| dt  | number | duration                                      |
| p   | string | page name                                     |
| pid | string | used for the PREFETCH procedure               |
| rc  | number | result code : SUCCESS(0), CANCEL(2), ERROR(1) |

end of init renderer js ( js )

| key | type   | description                     |
|-----|--------|---------------------------------|
| t   | string | NEZHA_INIT_RENDERER_JS_END      |
| pid | string | used for the PREFETCH procedure |

execute the renderer js ( js )

| key | type   | description                                   |
|-----|--------|-----------------------------------------------|
| t   | string | NEZHA_RENDER_PAGE_JS                          |
| st  | number | start time                                    |
| et  | number | end time                                      |
| dt  | number | duration                                      |
| p   | string | page name                                     |
| pid | string | used for the PREFETCH procedure               |
| rc  | number | result code : SUCCESS(0), CANCEL(2), ERROR(1) |

error when creating the renderer

| key   | type   | description               |
|-------|--------|---------------------------|
| t     | string | NEZHA_ERR_CREATE_RENDERER |
| e     | string | error info                |

error when injecting the renderer js 

| key | type   | description                  |
|-----|--------|------------------------------|
| t   | string | NEZHA_ERR_INJECT_RENDERER_JS |
| e   | string | error info                   |

## Page Show Time

The event to report the time from the onShow to onHide of the page. Report by native.

| key | type   | description     |
|-----|--------|-----------------|
| t   | string | NEZHA_PAGE_SHOW |
| st  | number | start time      |
| et  | number | end time        |
| dt  | number | duration        |
| p   | string | page name       |
| pid | string | renderer_id     |

### App Launch Time

The events from the native creating the worker, to the app been killed.

decompress the zip file, or load the cached resources from the local.

| key   | type   | description                                   |
|-------|--------|-----------------------------------------------|
| t     | string | NEZHA_INIT_RESOURCES                          |
| st    | number | start time                                    |
| et    | number | end time                                      |
| dt    | number | duration                                      |
| p     | string | index page name                               |
| extra | object | uc: boolean // use cache                      |
| rc    | number | result code : SUCCESS(0), CANCEL(2), ERROR(1) |

when the worker is created

| key   | type   | description                                   |
|-------|--------|-----------------------------------------------|
| t     | string | NEZHA_CREATE_WORKER_END                       |
| p     | string | index page name                               |

native starts to inject worker js

| key | type   | description                                   |
|-----|--------|-----------------------------------------------|
| t   | string | NEZHA_INJECT_WORKER_JS                        |
| st  | number | start time                                    |
| et  | number | end time                                      |
| dt  | number | duration                                      |
| p   | string | index page name                               |
| rc  | number | result code : SUCCESS(0), CANCEL(2), ERROR(1) |

js side finished to init the worker js (js)

| key | type   | description              |
|-----|--------|--------------------------|
| t   | string | NEZHA_INIT_WORKER_JS_END |

### App Show Time

The event to report the time from the onShow to onHide of the app. Report by native.

| key   | type   | description              |
|-------|--------|--------------------------|
| t     | string | NEZHA_PAGE_SHOW          |
| st    | number | start time               |
| et    | number | end time                 |
| dt    | number | duration                 |
| p     | string | index page name          |
| extra | object | uc: boolean // use cache |

### App Killed

The event to report the app has been killed by the system. Report by native.

| key | type   | description         |
|-----|--------|---------------------|
| t   | string | NEZHA_APP_TERMINATE |
| p   | string | the last page path  |

### JS Runtime Error Event

The runtime error of the js side, report by the native.

| key | type   | description                          |
|-----|--------|--------------------------------------|
| t   | string | NEZHA_ERR_JS_RUNTIME                 |
| ty  | string | `FRAMEWORK_ERROR`, `USER_CODE_ERROR` |
| p   | string | current page path                    |
| e   | string | error info                           |

### Call Pugin Function

The event that calling an action of plugin

| key | type   | description                                                                            |
|-----|--------|----------------------------------------------------------------------------------------|
| t   | string | NEZHA_CALL_PLUGIN                                                                      |
| ty  | string | action name                                                                            |
| st  | number | start time                                                                             |
| et  | number | end time                                                                               |
| dt  | number | duration                                                                               |
| p   | string | index page name                                                                        |
| pid | string | caller Id. if is render, set to rendererId. If is worker, set to fixed string `worker` |


If there's any error when calling the plugin

| key | type   | description                                                                            |
|-----|--------|----------------------------------------------------------------------------------------|
| t   | string | NEZHA_ERR_CALL_PLUGIN                                                                  |
| ty  | string | action name                                                                            |
| pid | string | caller Id. if is render, set to rendererId. If is worker, set to fixed string `worker` |
| e   | string | error info                                                                             |

### Native Component Error

The error from the native component. Report by the native.

| key | type   | description                |
|-----|--------|----------------------------|
| t   | string | NEZHA_ERR_NATIVE_COMPONENT |
| ty  | string | component type             |
| p   | string | current page path          |
| e   | string | error info                 |


## Data Backend

The reporting side of the framework monitor is the big data backend, and the reporting address is: 
https://trace.binance.gg/000001/msmxmr

The url of Data Backend is configured on the Apollo, with `nezhaConfig.binance-trace.domain`. If the domain is empty, the native just doesn't upload the monitor data.
