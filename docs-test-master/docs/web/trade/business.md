---
id: ws-http-sdk-business
title: business
---

## overview

share the same websocket connect with all the http request and websocket stream.

**for http request usage**:
Same usage as before, but in case of security, you need to sync login status with server.

**for websocket stream**:
Init the stream first, then you can subscribe to the stream and unsubscribe the stream.

## integrate

### init

   > We need get env constant from env MARKET_STREAM_HOST, ACCOUNTS_HOST_PUBLIC, WS_HTTP_SDK_RESTFUL_RATE
   > This SDK is very new, so we need to add more track as possible

  a. web

  ```tsx
    import { wsHttpSdkInit } from '@binance/ws-http-sdk';
    import { useLoginStatus } from '@binance/hooks'
    import { MARKET_STREAM_HOST, WS_HTTP_SDK_RESTFUL_RATE, ACCOUNTS_HOST_PUBLIC } from '@/constant'
    import { track } from './track';

    const isHttp = Math.random() < Number(WS_HTTP_SDK_RESTFUL_RATE)
    //....
    wsHttpSdkInit({
      isHttp,
      track,
      sourceUrl: `${MARKET_STREAM_HOST}/market`,
      tokenUrlPrefix: __DEV__ ? '' : ACCOUNTS_HOST_PUBLIC,
    })
    //....
  ```

  b. electron
   electron need one more params: getSessionInfo, provided in electron_desktop >= v1.9.2

   add some code in `utils` file

   ```tsx
   import ElectronDesktop from 'electron-desktop'
    // eslint-disable-next-line import/no-mutable-exports
    let service = {}

    if (process.electron && window.__RUN_BY_ELECTRON__) {
      service = ElectronDesktop
    } else {
      service = {
        //...
        async getSessionInfo() {
          return {}
        },
        // ...
      }
    }
  ```

  ```tsx
  import { wsHttpSdkInit } from '@binance/ws-http-sdk';
  import { useLoginStatus } from '@binance/hooks'
  import { MARKET_STREAM_HOST, WS_HTTP_SDK_RESTFUL_RATE, ACCOUNTS_HOST_PUBLIC } from '@/constant'
  import electron from '@/utils/electron';
  import { track } from './track';
  
  const isHttp = Math.random() < Number(WS_HTTP_SDK_RESTFUL_RATE)
  //....
  wsHttpSdkInit({
    isHttp,
    track,
    getSessionInfo: electron.getSessionInfo,
    sourceUrl: `${MARKET_STREAM_HOST}/market`,
    tokenUrlPrefix: __DEV__ ? '' : ACCOUNTS_HOST_PUBLIC,
  })
  //....
```

### upgrade csp

  1. `MARKET_STREAM_HOST`
  2. `ACCOUNTS_HOST_PUBLIC`

  > should combine with domain name eg. `www.binance.cc` should have a csp of `wss://nbstream.binance.cc`

### upgrade the corresponding `@binance/api`
  
  比如说更新的api是`accountUserConfigGet`, 需要pr验证一下， 通过后升一下`@binance/api`的版本

### change api  => @binance/api

  `accountUserConfigGet` 是基于 `/gateway-api/v1/private/account/user-config/get`接口的封装， 所以需要将直接使用`/gateway-api/v1/private/account/user-config/get` 接口改成使用`@binance/api` 中的API
  