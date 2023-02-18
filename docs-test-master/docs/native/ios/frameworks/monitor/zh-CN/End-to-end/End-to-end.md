---
id: End-to-end
title: End-to-end
hide_title: true
---

# End-to-end

端到端目前包括以下两个部分数据的采集：

- HTTP请求监控（t=API）
- WS状态的监控（t=WSS）

## 一.上报接口

POST请求

dev：`https://www.devfdg.net/000000/isamfy`

qa: `https://www.qa1fdg.net/000000/isamfy`

生产环境

path： `bapi/bigdata/v1/public/monitor/trace/{event_id}/{event_token}`

域名：使用配置接口`traceLogDomain`字段，默认获取第一个，如果没有获取到配置，则使用`log.bntrace.com`作为兜底逻辑。

阿波罗配置接口：`/bapi/composite/v2/public/common/config/app/dynamic-config`

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




## 二.通用的数据格式

| Field |  Schema type    |   Summary   |  Example    |  Orign    |
| ----- | ---- | ---- | ---- | ---- |
| client   | string      | client type           | ios, android, electron, web                                  | clienttype         |
| version  | string      | version code          | 2.33.0                                                       | versioncode / etag |
| buid     | string      | device id             | 24535964-D974-4CC3-A76E-969C1F02B88E                         | BNC-UUID           |
| browser  | string      | // group by space     | Chrome 89.0.4389.86                                          | browser            |
| platform | string | mac / windows / linux | mac | platform |
| os | string | system version | 14.0.1 | system-version |
| brand | string | device brand | iPhone Xs | brand |
| session | string | app session id | C1750826-D810-402D-8BFE-4B5C56E8815 | session-id |
| root | boolean | app jailbreaked |      | jailbreaked |
| appId | string | app name | com.czzhao.binance | bundleId |
| operator | string | network operator | 中国电信 | operator |
| data | array | Monitor data list | [{  "t": "", // monitor type, required }] | data-list |
| sVersion | string | Schema version | 1.0 1: MAJOR version, 现有通用字段变更(e.g: 命名调整，删减), "1.0": 2.0 0: MINOR version, 新增通用字段, "1.0": 1.1 | / |

以上字段均为通用的字段。



## 三.HTTP 数据监控

目前HTTP监控Hook Binance 中所有的请求，时间均以毫秒(ms)为单位。

1. HTTP 监控上报字段列表

   | Key  | 类型      | 说明                                                     |
   | ---- | --------- | -------------------------------------------------------- |
   | t    | String    | 固定取值为API                                            |
   | d    | TimeStamp | 开始时间                                                 |
   |  p   | String?   | page name (android)                                               |
   | u    | URL       | 请求的接口地址                                           |
   | tid | String    | 业务API请求的x-trace-id header                           |
   | host | String    | url host                                                      |
   | s    | int       | HTTP Status                                              |
   | c    | int       | 固定取值（error != nil ? -10000 : 200）                  |
   | reqs | long      | request包体大小                                          |
   | ress | long      | response包体大小                                         |
   | dns  | long      | dns耗时                                                  |
   | tls  | long      | tls耗时                                                  |
   | con  | long      | socket建连耗时                                           |
   | ct   | long      | 网络请求总耗时                                             |
   | bc   | String?   | 取response里面的message字段|
   | req  | long      | 发送请求耗时                                             |
   |l                   | long                          | 请求等待时间latency: responseStart- requestEnd |
   | res  | long      | 接受数据并且读取完耗时                                   |
   | reqh  | long      | 请求时发送报头数据的时间(android)      |
   | reqb  | long      | 在请求时发送主体数据的时间(android)     |
   | resh  | long      | 接收头数据和读取数据的时间(android)     |
   | resb  | long      | 接受主体数据和读取数据的时间(android)     |
   | busc | String?   | 取response里面的code字段    |
   | i | String?   | 远端 IP 地址    |
   | h | String?   | 网络协议名 h2, http/1.1, spdy/3.1    |
   | network | String   | network type, WIFI/3G/4G |
   | tlsV | String   | tls version, TLS_1_2    |
   | cn? | String   | app channel binance or google (android)   |
   | pkg? | String   | 包名 (android)   |
   
2. 上报给后台的数据格式

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
   


## 四.WSS 数据监控

1. 目前针对WSS有5个场景进行了日志上报

   ```
   public enum WSSMonitorType: String {
       case connect
       case disconnect
       case subscribe
       case pingPong
       case recover
   }
   ```

   

2. WSS 监控上报字段列表

   | Key  | 类型        | 说明                                                         |
   | ---- | ----------- | ------------------------------------------------------------ |
   | t    | String      | WSS                                                          |
   | u    | URL         | 建立连接的URL                                                |
   | wt   | String      | WSSMonitorType对应的五种类型                                 |
   | c   | Int         | 固定取值（bc != nil ? -10000: 200）                     |
   | d    | TimeStamp   | 打点时间                                                     |
   | ct   | TimeStamp？ | 耗时（如connect、pingPong、subscribe）                       |
   | bc   | String?     | 具体的报错信息                                               |
   | wt_s | String?     | 主要是提供给断开连接使用，固定取值（unexpected、manual、pingFailed） |
   | network | String   | network type, WIFI/3G/4G |
   | status | String   | start or end   |

   

3. 上报给服务器的JSON 数据格式

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



