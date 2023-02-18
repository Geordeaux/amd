# Themis Portal
## Web API
### 1. Service Address
dev  ： https://abtest-admin.fe.devfdg.net/bapi/themis/web

saas ：https://themis.blsdkrgjf.io/bapi/themis/web/
### 2. API Spec
#### a. Okta
1 Create okta session
   
Method : GET

Path: /okta/session

Query parameter: code={code}&redirect_uri={redirect_uri}

if success, return HTTP status code 200 and code 0, body
```json
{
    "code": 0,
    "msg": "ok",
    "data": {
        "session_token":"...",
        "expires_in":3600
    }
}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{
    "code": 1000,
    "msg": "xxxxxx",
    "data": null
}
```

2 Get okta session info
Method : POST

Path: /okta/session

request with cookie: bnc-themis-session ={session}

if success, return HTTP status code 200 and code 0, body
```json
{
    "code": 0,
    "msg": "ok",
    "data": {
        "email":"..."
    }
}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{
    "code": 1000,
    "msg": "xxxxxx",
    "data": null
}
```
3 Delete okta session
Method : DELETE

Path: /okta/session

request with cookie: bnc-themis-session ={session}

if success, return HTTP status code 200 and code 0, body
```json
{
    "code": 0,
    "msg": "ok",
    "data": null
}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{
    "code": 1000,
    "msg": "xxxxxx",
    "data": null
}
```

4. Get All User Emails
maximum: 500

Method: Get

Path: /okta/user

request with cookie: bnc-themis-session ={session}

if success, return HTTP status code 200 and code 0, body

```json
{
    "code": 0,
    "msg": "ok",
    "data": {
        users: ["...","..."] // user email list
    }
}
```

#### b. Authorization

**App Admin can read/edit the app and create/edit/read the strategies that belong to that app.**

**App Operator can only read the app and create/read the strategies that belong to that app but can only edit his strategy.** 

**App/Strategy Owner are fully authorized to the App/Strategy.**

request with cookie: bnc-themis-session ={session}

1 Update app admin/operator (only admin)
Method： PUT

Path：/access/{app_id}

Body : 
```json
{
    "adminer":  [ "...", "..." ], // user emails, expected latest admin list
    "operator": [ "...", "..." ], // user emails, expected latest operator list
}
```
if success, return HTTP status code 200 and code 0, body
```json
{
    "code": 0,
    "msg": "ok",
    "data": null
}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{
    "code": 1000,
    "msg": "xxxxxx",
    "data": null
}
```

2 List app auth

Method：GET

Path：/access/{appid}
```
if success, return HTTP status code 200 and code 0, body
```json
{
    "code": 0,
    "msg": "ok",
    "data": [
        {
            "user":"...",       // user email
            "role":"Admin"  // role
        },
        {
            "user":"...",       // user email
            "role":"Operator",  // role
        }
    ]
}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{
    "code": 1000,
    "msg": "xxxxxx",
    "data": null
}
```

e. Application

**request with cookie: bnc-themis-session ={session}**

1 Get One App Info

Method：GET

Path：/app/aid/{appid}
if success, return HTTP status code 200 and code 0, body
```json
{
    "code": 0,
    "msg": "ok",
    "data": {
        "name":  "...",
        "alias": "...",
        "aid":   "...",
        "key":   "...",
        "secret":"...",
        "bundle_id": "...",
        "platform": 3, //int type （1: iOS, 2: Android, 3: web, 4: iOS miniapp， 5: Android miniapp 6: Electron）
        "is_discard": "...",
        "owner_email": "...",
        "icon_url": "...",
        "version" : ["1.0.0","1.0.1",...]
    }
}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{
    "code": 1000,
    "msg": "xxxxxx",
    "data": null
}
```

2 Create New App 

Method：POST

Path：/app

Body
```json
{
    "name":  "...",         // required, global unique
    "alias": "...",         // required, global unique
    "bundle_id": "...",     // optional
    "platform": 1,          // required, int type （1: iOS, 2: Android, 3: web, 4: iOS miniapp， 5: Android miniapp 6: Electron）
    "owner_email": "...",   // required, add to app adminer automatically
    "icon_url": "...",      // optional
    "version" : ["1.0.0","1.0.1","..."] // optional, if no, give empty list
}
```
if success, return HTTP status code 200 and code 0, body
```json
{
    "code": 0,
    "msg": "ok",
    "data": {
    }
}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{
    "code": 1000,
    "msg": "xxxxxx",
    "data": null
}
```

3 Update One App Info 

Method：PUT

Path：/app/{appid}

Body:

```json
{
    "alias": "...", // if empty str or absent filed, api will not update the filed         
    "bundle_id": "...",                
    "owner_email": "...",  
    "icon_url": "...", 
    "key":"...",
    "secret":"...",
    "version" : ["1.0.0","1.0.1","..."]
}
```
if success, return HTTP status code 200 and code 0, body
```json
{
    "code": 0,
    "msg": "ok",
    "data": {
    }
}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{
    "code": 1000,
    "msg": "xxxxxx",
    "data": null
}
```

5 Get One App by Name
Method：GET

Path：/app/name/{appname}

if success, return HTTP status code 200 and code 0, body
```json
{
    "code": 0,
    "msg": "ok",
    "data": {
        "name":  "...",
        "alias": "...",
        "aid":   "...",
        "key":   "...",
        "secret":"...",
        "bundle_id": "...",
        "platform": 3, //int type （1: iOS, 2: Android, 3: web, 4: iOS miniapp， 5: Android miniapp 6: Electron）
        "is_discard": "...",
        "owner_email": "...",
        "icon_url": "...",
        "version" : ["1.0.0","1.0.1",...]
    }
}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{
    "code": 1000,
    "msg": "xxxxxx",
    "data": null
}
```
#### f. Strategy Configuration

request with cookie: bnc-themis-session ={session}

1 Create New Strategy 新建策略
Method：POST

Path：/strategy

Body:
```json
{
    "appid":"xxxx",
    "type":0,//0=flow 1=condition 2=universal-flow
    "desc": "strategy description",
    "creator": "" // must-have
}
```
if success, return HTTP status code 200 and code 0, body
```json
{
 "code": 0,
 "msg": "ok",
 "data": {
   "test_id":1,
    "desc":"",// 实验描述
    "conf": null,
    "type": 1
    "created_time":"2021-01-02 12:01:12",
    "updated_time":"2021-01-02 12:01:12",
    "creator":"alex.g@binance.com",   
    "status" : 1 // //0=start 1=draft 2=delete 3=rollout
    }
  }
}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{
    "code": 1000,
    "msg": "xxxxxx",
    "data": null
}
```

2 Binding Keys for Strategy

Method：POST

Path：/strategy/bindkey/{test_id}

**Each key can only consist of alphabet, number, or underscore, otherwise will get error msg in UI.**

Body:
```json
{
    "keys": ["a","b"]//必传，最大10个
}
```
if success, return HTTP status code 200 and code 0, body
```json
{ "code": 0, "msg": "ok", "data": null}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{ "code": 10000, "msg": "key is duplicated", "data": null }
```
3 Save Strategy 

Method：PUT

Path：/strategy/{test_id}

Body:
```json
{
    "desc":"",// 实验描述
    "conf": {
            //...
    }      
}
```
**conf （0: flow, 1:condition, 2: universal-flow）**

if strategy  type = 0 , conf will be 
```json
{
        "custom_rule":"...",// str: user-defined rule. ex: "lang==en && age < 18". if empty string, that is no rule.
        "group":{                               
            "1":{                               // group_id ： must > 1
                "id": 1,                        // correspond to group_id
                "desc":"Control",               // desc: group description
                "percent":{"start":0,"end":49}, // percent:  int <100
                "data":{                        // data: when hit, this kv pairs will be distributed.
                    "button_color":"red"
                }
            },
            "2":{
                "id": 2,
                "desc":"Test",
                "percent":{"start":50,"end":99},
                "data":{
                    "button_color":"green"
                }
            }
        }
                  
}
```
if strategy  type = 1, conf will be 

```json

{
    "data":{
            "1":{ //platform：平台（1: iOS, 2: Android, 3: web, 4: iOS miniapp， 5: Android miniapp 6: Electron） 
                "aa":"cc"
            },
            "2":{
                "ae":"uc"
            },
    }      
}

```
if strategy  type = 2, conf will be 
```json
{
        "custom_rule":"...",       // str: user-defined rule. ex: "lang==en && age < 18". if empty string, that is no rule.
        "layer_seed": "...",       // layer_seed: use it for hash salt. length less than 50.
        "total_buckets" : 1000,    // optional 100, 1000 and 10000.index strat from 0.
        "universal_group":{
             "percent":5,   // 5% in universal group, 95% for strategy grouping. maximum:15%.
             "seed": "...", // seed: use it for hash salt to split universal group. length less than 50.
             "data":{       // data: if user hits universal group, distribute this k-v pairs for user.
                    "button_color":"iamuniversal",
                },
        "rollout":{             // if status is rollout,themis use this configure to distribute strategy. check if it exists, once strategy-operator activate rollout.
            "rollout_group":"1", // use which group to rollout for all users.
            "rest_group":"2",
            "percent": 30,      // 30% rollout 30% users to rollout group,70% to rest group
        },
        "group":{                                
            "1":{                                  // group_id ： need>=1.
                "id": 1,                           // consistent with group_id. use group_id for internal usage.
                "desc":"avoid_group",              // group desccription. need string check.
                "percent":{"start":0,"end":199},   // bucket range. strat from 0. check sum of every group is eaual to total_buckets.
                "data":{                           // data: give these k-v pairs when hit this gorup
                    "button_color":"red"
                }
            },
            "2":{
                "id": 2,
                "desc":"control_group",
                "percent":{"start":200,"end":299}, // start and end need to be in total bucket range
                "data":{
                    "button_color":"green"
                }
            },
             "3":{
                "id": 3,
                "desc":"test_group",
                "percent":{"start":300,"end":399},
                "data":{
                    "button_color":"blue"
                }
            } ,
              "4":{
                "id": 4,
                "desc":"rest_group",
                "percent":{"start":400,"end":999},
                "data":{
                    "button_color":"red"
                }
            },
         }
                  
}
```

if success, return HTTP status code 200 and code 0, body
```json
{ "code": 0, "msg": "ok", "data": null}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{ "code": 10000, "msg": "....", "data": null }
```
4 Activate Strategy 
Method：GET

Path：/strategy/start/{strategy_id}

if success, return HTTP status code 200 and code 0, body
```json
{ "code": 0, "msg": "ok", "data": null}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{ "code": 10000, "msg": "....", "data": null }
```

5 Delete Strategy

Method：DELETE

Path：/strategy/{test_id}

if success, return HTTP status code 200 and code 0, body
```json
{ "code": 0, "msg": "ok", "data": null}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{ "code": 10000, "msg": "....", "data": null }
```

6 Lookup Strategy
Method：POST

Path：/strategy/list/{page}/{limit}

limit：每页几个 ( max: 200)

page：分页数

Body: ( empty fields mean no limit, {}: return all strategies, {"test_id":12}:use test id as search index)

```json

{
    "test_id": 54,
    "appid": "xxx1",
    "type": 0, //0=flow 1=condition
    "desc": "use LIKE operator to search",
    "status": 1,
    "start": "Thu, 29 Jul 2021 07:56:36 CST",// rfc1123 search created >= "Thu, 29 Jul 2021 07:56:36 CST"
    "end": "Thu, 29 Jul 2021 07:56:36 CST", // rfc1123 search created < "Thu, 29 Jul 2021 07:56:36 CST"
    "creator": "xxx@binance.com" // user email
}
```

if success, return HTTP status code 200 and code 0, body
```json
{
    "code": 0,
    "msg": "ok",
    "data": {
        "list":[
           {
            "test_id":1,
            "desc":"test-description",
            "status": 0, //0=start 1=draft 2=deleted 3=rollout
            "type":0,    //0=flow 1=condition 3=universal
            "creator":"创建人的账号(公司邮箱)",
            "created_time":"Thu, 29 Jul 2021 07:56:36 CST",//rfc1123 
            "updated_time":"Thu, 29 Jul 2021 07:56:36 CST",//rfc1123 
            "fulldoes_group_id": 0, // if not 0, it's full flow.
           "action" : ["read","edit"],   // can read(detail) and edit based on user authorization
            "app":{
                   "name":  "...",
                    "alias": "...",
                    "aid":   "...",
                    "platform": 3, //int type （1: iOS, 2: Android, 3: web, 4: iOS miniapp， 5: Android miniapp 6: Electron）
                    "is_discard": "...",
                    "owner_email": "...",
                    "icon_url": "...",
                }
           }
        ],
       "total": 20,
       "limit": 10,
       "page":1
    }
}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{ "code": 10000, "msg": "...", "data": null }
```
7 Lookup Keys by Strategy ID 查询策略绑定key
Method：GET

Path：/strategy/keylist/{strategy_id}

if success, return HTTP status code 200 and code 0, body
```json
{ "code": 0, "msg": "ok", "data": ["a","b","c"] }
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{ "code": 10000, "msg": "....", "data": null }
```

8 Get Strategy Detail
Method：GET

Path：/strategy/detail/{strategy_id}
if success, return HTTP status code 200 and code 0, body
```json
{ "code": 0,
  "msg": "ok",
  "data": {
    "test_id":1,
    "desc":"",
    "conf": {
        //... 
    },
    "type": 1 ,
    "status": 0,
    "created_time":"2021-01-02 12:01:12",
    "updated_time":"2021-01-02 12:01:12",
    "creator":"alex.g@binance.com",
    "fulldoes_group_id": 0, // if not 0, it's full flow.  
    }
}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{ "code": 1000, "msg": "...", "data": null }
```

9 Fulldoes Flow type Strategy 
Method：POST

Path：/strategy/fulldoes/{strategy_id}

Body:
```json
{
    "group_id":1 //required
}
```

if success, return HTTP status code 200 and code 0, body
```json
{ "code": 0, "msg": "ok", "data": null}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{ "code": 10000, "msg": "....", "data": null }
```

10 Rollout Strategy 
Method：POST

Path：/strategy/rollout/{strategy_id}

Body:
```json
{
    "rollout_group":"1",   //required
    "rest_group": "2",     //required
    "percent": 30,
}
```

if success, return HTTP status code 200 and code 0, body
```json
{ "code": 0, "msg": "ok", "data": null}
```
if fails, HTTP status code 200 but code(in json) depends on error type, body
```json
{ "code": 10000, "msg": "....", "data": null }
```

g. Log View

1 Get app events list 

Method：GET

Path：/app/event/user/:userId/app_id/:appId

```json
{
  
    "code": 0,
    "msg": "ok",
    "data": [
        {
            "event_id": 1,               // 事件id
            "timestamp": 1222344455,     // 时间戳，按时间戳由远至近排序返回
            "operator_email": "string",  // 操作者邮箱
            "action": "string",          // 操作类型(etc. "create app", "update app", "delete app"...)
        },
    ]
}
```

2 Get event detail for a specific app 
Method：GET

Path：/app/event/detail/eventId/user/:userId/app_id/:appId

response：

```json
{
    "code": 0,
    "msg": "ok",
    "data": "string",         // JSON string
}
```

3 Get strategy events list

Method：GET

Path：/strategy/event/user/:userId/strategy_id/:strategyId



response：

```json
{
  
    "code": 0,
    "msg": "ok",
    "data": [
        {
            "event_id": 1,              // event id
            "timestamp": 12223445,      // 时间戳，按时间戳由远至近排序返回
            "operator_email": "string", // 操作者邮箱
            "action": "string",         // 操作类型(etc. "create strategy", "update strategy", "delete strategy"...)
        },
    ]
}
```

4 Get event detail for a specific strategy 
Method：GET

Path：/strategy/event/detail/:eventId/user/:userId/strategy_id/:strategyId

response：

```json
{
  "code": 0,
  "msg": "ok",  "data": "string",            // JSON string
}
```


