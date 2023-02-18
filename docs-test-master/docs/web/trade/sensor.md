---
id: ws-http-sdk-sensor
title: Sensor
---

## overview

The purpose of this sensor is to monitor the behavior of the sdk. There are 3 kind sensor track.

1. status

   ```tsx
   interface IStatus{
      uuid: string
      isHttp: string // true | false
      clientType: 'web' // 'web' | 'hybrid' | 'electron' | 'ios' | 'android'
      status: 'ws_start'|
     'ws_connected' |
     'ws_closed' |
     'ws_error' |
     'publicRequest' |
     'get_token' | 
     'get_token_value_invalid' |
     'get_token_error' |
     'deadEnd' |
     'privateRequest' |
     'friendlyRequest' |
     'list_channels' |
     'subscribe_channels' |
     'unsubscribe_channels' |
     'stream'
     duration: number // ms 
     statuses: string // combine status and duration eg: ws_close <-100- ws_connected
     ws_token: string
     ws_sourceUrl: string
   }
   ```

2. request error

   ```tsx
   type IPrivateResponseError = IStatus & ({
         type: 'INVALID_TOKEN' | 'INVALID_REQUEST' | 'TIMEOUT' | 'WS_CONNECTION_TIMEOUT' | 'WS_ABNORMAL_CLOSE'
       }
     | {
         type: 'SERVICE_ERROR'
         errorCode: number
         errorMsg: string
       }
     | {
         type: 'VERIFY'
         errorMsg: string
       }
     | {
         type: 'ERROR'
         error: any
       })
   
   type IPublicResponseError = IStatus & ({
         type: 'INVALID_REQUEST' | 'TIMEOUT' | 'WS_CONNECTION_TIMEOUT' | 'WS_ABNORMAL_CLOSE'
       }
     | {
         type: 'VERIFY'
         errorMsg: string
       }
     | {
         type: 'ERROR'
         error: any
       })
   
   type IFriendlyResponseError = IStatus & ({
         type: 'INVALID_TOKEN' | 'INVALID_REQUEST' | 'TIMEOUT' | 'WS_CONNECTION_TIMEOUT' | 'WS_ABNORMAL_CLOSE'
       }
     | {
         type: 'SERVICE_ERROR'
         errorCode: number
         errorMsg: string
       }
     | {
         type: 'VERIFY'
         errorMsg: string
       }
     | {
         type: 'ERROR'
         error: any
       })
   ```

3. ws connection type
  
  ```tsx
    type IWsStatus = IStatus && ({
      ws_connectId: string
      ws_type: 'ws_openDelay' | 'ws_open'
      duration: number
    } | {
      ws_connectId: string
      ws_type: 'ws_close'
      duration: number
      error: string
      code: number
    })
  
  ```

### success rate

![image-20210106143407784](https://static.devfdg.net/static/mono-static/docs-ui/img/ws-http-sdk/sensor/successRate/http.png)

![image-20210106143231859](https://static.devfdg.net/static/mono-static/docs-ui/img/ws-http-sdk/sensor/successRate/websocket.png)

### duration

### connection times
