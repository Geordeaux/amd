# Themis Engine

## Web API
### 1. Server Address
dev  : https://www.devfdg.net/bapi/themis/api

qa   : https://www.qa1fdg.net/bapi/themis/api

saas :  https://api.saasexch.com/bapi/themis/api/


### 2. API Spec

#### 0. Basic Auth
All APIs will be check Basic Auth (User: **appkey** Password: **appsecret**)

ref:
https://en.wikipedia.org/wiki/Basic_access_authentication

return 
```json
{
    "code": 2001,
    "msg": "basic access error",
    "data": null
}
```
```json
{
    "code": 2002,
    "msg": "appkey is wrong",
    "data": null
}
```
```json
{
    "code": 2003,
    "msg": "appsecret is wrong",
    "data": null
}
```

#### 1. User Sign Up API
##### **Instruction**
1. Sign up user's info and then asynchronously trigger traffic splitting computation.
2. After success, you will get a token as user identifier(or you can activate uid_as_token). Themis only recognize this token as each user and decide the variants by token.
   
a)  please use "uid" as trace ID(like bnc_uuid) for data analysis task. 

b)  if uid_as_token is true ,
1. themis will use provided uid as token instead of generating a new token,
2. and  cross-platform mode get activated and platform is going to be set to zero. 
3. when cross-platform mode, user wil be grouped for each strategy no matter which application.

Method：POST

Path：/user/signup

Body:
```json
// uid(str): trace id is like bnc_uuid
// device_id(str): unique id, web optional
// platform(int): 0: cross-platform, 1: iOS, 2: Android, 3: web, 4: iOS miniapp， 5: Android miniapp, 6: Electron
// os_version str: web optional
// app_version(str)
// region(str)
// language(str)
// exchange_rate(str)
// uid_as_token(bool): (optional)backend will not compute random token. instead, use uid as token and return it.(default false)

{
    "uid": "...",           
    "device_id": "...",
    "platform": 0,
    "os_version": "...",
    "sdk_version": "...",
    "app_version": "...",
    "region": "...",
    "language": "...",
    "exchange_rate": "...",
    "uid_as_token": false,
 }
```
request success, return status code 200 but code 0, body:
```json
{
    "code": 0,
    "msg": "ok",
    "data": {
        "aid": "60ff7be635a92a1ed019b5a3",
        "token": "e1b0372b104ec6bd909d752a369e0057"
    }
}
```

request fail, also return status 200 but code depends on error type, body:
```json
{
	"code": 1000,
	"msg": "xxxxxx",
	"data": null
}
```
#### 2. User Info Update 
##### **Instruction**
when app upgrade SDK version, device id, language and exechange_rate, need to invoke this api.。

Before SDK call this API, please check if update the same data again.

**This SDK is Asynchronous.**

Method：POST

Path：/user

Body:
```json
{
	"sdk_version": "v1,0,0", // no update, if empty
    "device_id": "mac-1234", // no update, if empty
    "language": "en", 		 // no update, if empty
    "exchange_rate":"usd",	 // no update, if empty
    "token": "66add0941e49087f66890ff10a637a8d" // must have     
}
```

request success, return status code 200 but code 0, body:
```json
{
    "code": 0,
    "msg": "ok",
    "data": "update will get done async"
}
```

request fail, also return status 200 but code depends on error type, body:
```json
{
	"code": 1000,
	"msg": "xxxxxx",
	"data": null
}
```

### 3. Async Query API
#### Instruction
Because of async computing, it will not return the latest traffic splitting but ensure **eventual consistency**.

Maximum: 1000 strategies

Method：GET

Path：/strategy/query?token=xxxx

token(str): user token

if success, status code 200, return
```json
{
	"flows": [
		{
			"id": "1",					// Strategy ID
			"strategy_desc": "...",     // Strategy Desc
			"group_id": "1",			// Strtegy group ID
			"group_desc":"..."          // Strtegy group Desc
		    "payload": {				// map tpye
				"TitleTextColor":"red", // may be "yellow" in group 2			
			} 		
		},
		{
			"id": "2",					   // Strategy ID
		 	"strategy_desc": "...",    	   // Strategy Desc
			"group_id": "2",			   // Strtegy group ID
			"group_desc":"...",            // Strtegy group Desc
    		"payload": {				   // Map tpye 
				"TurnOnTradingFunc":"yes"，//may be "no" in group 1			
			}
		},
	],
	"conditions": {
		"payload": {					// Map Type
			"a":"b",
			"c":"d",
		} 	
	},
	"advice_polling_interval": 300, 	// Suggested Polling Period(unit:second)
	"force": true, 						// if needed to activate right now.
	"request_id": "..."					// query version
}
```

if there is no strategy, return 

```json
{
    "code": 0,
    "msg": "ok",
    "data": {
        "flows": [],
        "conditions": {
            "payload": {}
        },
        "advice_polling_interval": 300,
        "force": false,
        "request_id": "901daaba-b627-2f3d-bd48-71e1386c07d6"
    }
}
```

#### 4 Sync Query API

Method：POST

Path：/strategy/query?token=xxxx

token（string）：  user token

Body:
```json
{
	"strategy_id": [1,3,5] // max number: 15 // only one condition type
}
```
if success, return
```json
{
	"code":0,
	"msg" : "ok",
	"data": 
	[
		{
			"id": "1",					// Strategy ID
			"strategy_desc": "...",    	   // Strategy Desc
		    "type": "flow".             // strategy Type
			"group_id": "1",			// Strtegy group ID
			"payload": {				// map tpye
				"TitleTextColor":"red", // may be "yellow" in group 2			
			} 		
		},
        {
			"id": "3",	
			"strategy_desc": "...",    	   // Strategy Desc
	        "type": "flow",         
			"group_id": "2",			
			"payload": {				
				"TradeFunction":"off",  			
			} 		
		},
        {
			"id": "5",
			"strategy_desc": "...",    	   // Strategy Desc         
			"type": "condition",			
			"payload": {				
				"ShowPrompt":"no",
			} 		
		},
	]
}
```
if fails, return status code but code depends on error type, body
```json
{
	"code": 1000,
	"msg": "..."
}
```