---
id: End-to-end
title: End-to-end
hide_title: true
---

# End-to-end

End-to-end currently includes the following two parts of data collection:：

- HTTP track(t=API）
- WS track（t=WSS）

## 1.Log reporting API

POST requst

dev：`https://www.devfdg.net/000000/isamfy`

qa: `https://www.qa1fdg.net/000000/isamfy`

Prod

path： `bapi/bigdata/v1/public/monitor/trace/{event_id}/{event_token}`

host：Use the configuration API `traceLogDomain` field, get the first one by default, if the configuration is not obtained, use `log.bntrace.com` by default.

Apollo configuration interface: `/bapi/composite/v2/public/common/config/app/dynamic-config`

```json
{
    "code":"000000",
    "data":{
        "resTimeout":"15",
        "reqTimeout":"15",
        "traceLogDomain":[
            "log.bntrace.com"
        ],
        "subTimeout":"3"
    },
    "success":true
}
```

## 2.Common Field description

| Field    | Schema type | Summary               | Example                                                      | Orign              |
| -------- | ----------- | --------------------- | ------------------------------------------------------------ | ------------------ |
| client   | string      | client type           | ios, android, electron, web                                  | clienttype         |
| version  | string      | version code          | 2.33.0                                                       | versioncode / etag |
| buid     | string      | device id             | 24535964-D974-4CC3-A76E-969C1F02B88E                         | BNC-UUID           |
| browser  | string      | // group by space     | Chrome 89.0.4389.86                                          | browser            |
| platform | string      | mac / windows / linux | mac                                                          | platform           |
| os       | string      | system version        | 14.0.1                                                       | system-version     |
| brand    | string      | device brand          | iPhone Xs                                                    | brand              |
| session  | string      | app session id        | C1750826-D810-402D-8BFE-4B5C56E8815                          | session-id         |
| root     | boolean     | app jailbreaked       |                                                              | jailbreaked        |
| appId    | string      | app name              | com.czzhao.binance                                           | bundleId           |
| operator | string      | network operator      | 中国电信                                                     | operator           |
| data     | array       | Monitor data list     | [{  "t": "", // monitor type, required }]                    | data-list          |
| sVersion | string      | Schema version        | 1.0 1: MAJOR version, existing general field changes (e.g: naming adjustment, deletion), "1.0": 2.0 0: MINOR version, new general field, "1.0": 1.1 | /                  |

The above fields are common fields.



## 3.HTTP Monitoring

HTTP monitors all requests in Hook Binance, and the time is in milliseconds (ms).

1. HTTP monitoring report field list

   | Key  | Type      | Description                                                     |
   | ---- | --------- | -------------------------------------------------------- |
   | t    | String    | API                                            |
   | d    | TimeStamp | Starting time                                                 |
   | p    | String?    | page name(android)                                               |
   | u    | URL       | Request url                                        |
   | tid | String    | x-trace-id                          |
   | host | String    | url host                                                         |
   | s    | int       | HTTP Status                                              |
   | c    | int       |（error != nil ? -10000 : 200）                  |
   | reqs | long      | request package size                                         |
   | ress | long      | response package size                                         |
   | dns  | long      | dns time                                                  |
   | tls  | long      | tls time                                                  |
   | con  | long      | socket connection time|
   | ct   | long      | request total time                                             |
   | bc   | String?   | The message field in the response |
   | req  | long      | The time to send the request         |
   |l                   | long                          | latency: responseStart- requestEnd |
   | res  | long      | The time to accept data and read data     |
   | reqh  | long      | The time to send header data at the request(android)      |
   | reqb  | long      | The time to send body data at the request(android)     |
   | resh  | long      | The time to accept header data and read data(android)     |
   | resb  | long      | The time to accept body data and read data(android)     |
   | busc | String?   | The code field in the response   |
   | i | String?   | remoteAddress   |
   | h | String?   | networkProtocolName h2, http/1.1, spdy/3.1    |
   | network | String   | network type, WIFI/3G/4G |
   | tlsV | String   | tls version, TLS_1_2    |
   | cn | String?   | app channel binance or google (android)   |
   | pkg | String?   | app package name (android)   |
   
2. The complete data format reported to the server

   ```json
   {
       "data":[
           {
               "t":"API",
               "tlsV":"771",
               "ress":751,
               "host":"www.hanqiweb.co",
               "ct":5191,
               "h":"h2",
               "tid":"8B0ECC80-B397-4F76-AF3D-87F423BFC9B7",
               "reqs":1211,
               "s":200,
               "d":1628671218892,
               "res":9,
               "network":"4G",
               "u":"https:\/\/www.hanqiweb.co\/bapi\/futures\/v1\/private\/future\/agent\/agentcode\/get",
               "i":"192.168.0.106",
               "c":200
           }
       ],
       "session":"E01546E3-10FF-4BB3-A872-7E209AC4BAE3",
       "root":false,
       "appId":"com.czzhao.binance.dev",
       "client":"ios",
       "operator":"CMHK",
       "brand":"iPhone Xs",
       "buid":"24535964-D974-4CC3-A76E-969C1F02B88E",
       "sVersion":"1.0",
       "version":"2.34.0",
       "os":"14.4"
   }
   ```
   


## 4.WSS Monitor

1. Currently, logs are reported for 5 scenarios of WSS

   ```
   public enum WSSMonitorType: String {
       case connect
       case disconnect
       case subscribe
       case pingPong
       case recover
   }
   ```

   

2. WSS monitor report field list

   | Key  | Type        | Description                                                         |
   | ---- | ----------- | ------------------------------------------------------------ |
   | t    | String      | WSS                                                          |
   | u    | URL         | connection url                                                |
   | wt   | String      | WSSMonitorType（connect、disconnect、subscribe、pingPong、recover）                                 |
   | c   | Int         |（bc != nil ? -10000: 200）                      |
   | d    | TimeStamp   | Log report time                                                |
   | ct   | TimeStamp？ | Time-consuming（connect time、pingPong time、subscribe time）                       |
   | bc   | String?     | detail error message                                               |
   | wt_s | String?     | Type of disconnection （unexpected、manual、pingFailed） |
   | network | String   | network type, WIFI/3G/4G |
   | status | String   | start or end   |

3. The complete data format reported to the server

   ```json
   {
       "data":[
           {
               "u":"wss:\/\/nbstream.hanqiweb.co\/lvt\/stream",
               "t":"WSS",
               "status":"end",
               "ct":135,
               "d":1628677945875,
               "c":200,
               "wt":"connect",
               "network":"4G"
           }
       ],
       "operator":"中国移动",
       "root":false,
       "session":"C1750826-D810-402D-8BFE-4B5C56E8815E",
       "brand":"iPhone Xs",
       "version":"2.33.0",
       "buid":"24535964-D974-4CC3-A76E-969C1F02B88E",
       "client":"ios"
   }
   ```



