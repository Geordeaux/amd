---
id: arch-background-fetch
title: Background Fetch
sidebar_label: Background Fetch
---

import Mermaid from '@theme/Mermaid'

There's two types of Background fetch: **Pre Fetch** and **Periodic Fetch**

## Pre Fetch

Pre-fetch can pull business data from third-party servers through the MP backend in advance when the app is cold-start, rendering the page faster when the code package is loaded, reducing user waiting time and thus improving the opening speed of the app.

Sequence:

<Mermaid chart={`
sequenceDiagram
    Native->>MP App: Start app
    Native->>Developer's Server: reqeust data with token
    Developer's Server->>Native: pre fetch data
    MP App->>Native: getBackgroundFetchData() 
    Native->>MP App: callback with pre fetch data
`}></Mermaid>

Work flow: 

<Mermaid chart={`
graph TD
    A[Start] --> B(Start MP App)
    A --> C(get prefetch data from developer's server)
    C --> D[Save pre fetch data in the memory]
    B --> E[Call getBackgroundFetchData]
    E --> F(wait until prefetch data available)
    F --> G(return prefetch data via callback)
`}></Mermaid>

1. Backend will pass api address in the details api, return to native
Native will start the process of downloading and open mp
2. In the meanwhile, If there’s service token in the cache, native will request for data from the api address with token. Native will save the data in the cache.
3. After the mp started, mp will call function getBackgroundData, native will return the data from step 3 (if any) to the mp.

## Periodic Fetch

Periodic updates can pull data from the server in advance even when the user does not open the mp app, rendering the page faster when the user opens the mp app, reducing user waiting time and enhancing usability in weak network conditions.

<Mermaid chart={`
graph TD
    A[iterate all the mp in the local] --> B{last open in 7 days?}
    B -->|Yes|C{ Has backgroundFetchToken?}
    C -->|YES|D(Request data from developer's server)
    D --> E(Save to storage)
    F(Start MP App) --> G[Call getBackgroundFetchData]
    G --> H(wait until periodic fetch data available)
    H --> P(return periodic fetch data via callback)
`}></Mermaid>

- It will request data in the background every 12 hours ( only for the mp open in last 7 days).
- All the mp apps will be checked at a fixed time every day one by one. If Binance app doesn't work at that time, the process will start next time user open the Binance app, and reset the timer.

## Related actions ( function )

* bn.setBackgroundFetchToken(option) 
```ts
interface option { 
  token: string
  success: (res: string)=>void
  failed: func
  complete: func 
} 
```

* bn.onBackgroundFetchData(callback) 
```ts
type callback = ({
  fetchType: 'pre' | 'periodic', 
  fetchedData: string, 
  timeStamp: number
  }) => void
```

* bn.getBackgroundFetchToken(option) 
```ts
interface option { 
  success: ( token ) => void
  failed: func
  complete: func 
}
```

* bn.getBackgroundFetchData(option)
```ts
interface option {
  fetchType: 'pre' | 'periodic'
  success: (res: string) => void
  failed: func
  complete: func 
}
```

Function getBackgroundFetchData will return all the arguments sent by the native and also a field fetchedData for the data fetch from remote. 
It use a parameter fetchType to get two types of data:  pre and periodic

## Related Data in the response of api

**GET** /bapi/mp/v1/app/{appId}/details 

Response:
```
{
  ...
  developmentSettings: { 
    preFetchUrl: string
    backgroundFetchUrl: string
  }
}
```

## How to Request Remote 

### Periodic Fetch 
Native will make an HTTP GET request to the configured data download address every 12 hours (based on the last successful update) under certain network conditions, which contains the following query parameters.

| Arguments | Type   | Description                                                                         |
|-----------|--------|-------------------------------------------------------------------------------------|
| appid     | string | appId of mp                                                                         |
| token     | string | Token set by setBackgroundToken()                                                   |
| timestamp | number | The time client requested                                                           |
| lang      | string | The language current app used                                                       |
| version   | string | The version of mp app                                                               |
| cdnUrl    | string | Only used in the template, for the app with permission: USE_BINANCE_DYNAMIC_DOMAINS |

### Pre-Fetch
When the user opens the mp, the mp server will make an HTTP GET request to the developer server (the data download address configured above)

| Arguments | Type   | Description                                                                                          |
|-----------|--------|------------------------------------------------------------------------------------------------------|
| appid     | string | appId of mp                                                                                          |
| token     | string | Not required.                                                                                        |
| code      | string | Not required, if there’s no token, generated by natvie to help developer identify user, (oauth code) |
| timestamp | number | Current time                                                                                         |
| path      | string | Not required, path when opening the mp, from the deeplink                                            |
| query     | string | Not required, query when opening the mp, from the deeplink                                           |
| lang      | string | The language current app used                                                                        |
| version   | string | The version of mp app                                                                                |
| cdnUrl    | string | Only used in the template, for the app with permission: USE_BINANCE_DYNAMIC_DOMAINS                  |

### The rules of *-fetch-url

1. The domain should be in the service domain list, so it always have certificates data
2. Urls can use template for variables: 
    - https://example.com/{lang}.json
    - If one argument key matched in the template, it will not passed by get arguments anymore


## Related Documents

Wechat document: 
- https://developers.weixin.qq.com/miniprogram/dev/framework/ability/pre-fetch.html
- https://developers.weixin.qq.com/miniprogram/dev/framework/ability/background-fetch.html 



