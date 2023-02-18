# Themis Web SDK
### A SDK helping web/electron application get integration with A/B test
[Git Entry](https://git.toolsfdg.net/mono/mono/tree/master/web/libs/themis)

--------------------------

## Why
this lib will help you do register to Strategy Distribution center for a browser of electron client. 
you need to pass your client level(so called "custom_user_info") and your application level(baseUrl and authHeader) information. SDE will distribute strategies to your client according your register information

## How
sdk will cache your register information and do register conditionally. a valid register will get a token to mark your sdk instance, and the token would be stored with your register information together.
and sdk will handle the polling request to get strategies from SDE, the request will be sent once your sdk get initialized, and then repeat regularly.
all the strategies' config will be stored as well.
but what has to be aware of is during the duration of the instance, the config you can get from sdk instance is the configuration obtained from SDE during the duration of the previous instance, which is in order to keep your application works in a consistence performance.（once the config you get from SDE has the property "force" whose value is true, then you can instantly get the latest config rather than the old one）

and we also provide some APIs to adapt to some special requirements, please read content following up.

## Brief Flow Chart
![flow chart](https://static.devfdg.net/static/themis/flow.png)

## Demo Code
you would suffer a latency of config, as flows grouping requires time, in most cases, sdk will get valid config at the first time of polling

```js
import sdk from '@binance/themis'  // you need add //libs/themis into your bazel deps (eg: deps.bzl)

(async ()=>{
let helper
const yourSubscriber=(config)=>{
  /* 
  config: { 
    flows?: any
    conditions?: any
  }
  */
}

try{
  /* 
    init the sdk, sdk will check any cache of register information and config in local storage (Not necessarily be the localstorage of browser)
  */
   helper = await sdk.init( 
    {
      device_id: 'some-pc', // can default on web
      uid: 'your-email-address', // we recommend web client to pass bnc-uuid from cookie in binance web application
      platform: 3, // 1: iOS, 2: Android, 3: web, 4: iOS miniapp， 5: Android miniapp, 6: Electron
      os_version: '1.0.0', // can default on web
      sdk_version: '1.0.0',
      app_version: '1.0.0',
      region: 'eu', // can default on web
      language: 'en',
      exchange_rate: 'usd',
      useuidastoken: false // default false, for specific usage: use uid as your query token, consult developers before you open this setting.
      custom_user_info: {// custom user info is used to mark your sdk instance, and data center will distribute config by strategy custom rules. and for the reason of compatibility, sdk will check whether the cached init info has custom use info, if it doesn't while the newly coming one has one, sdk will do a force register ignoring cached info.
          "age":18,
          "gender":"male",
          "language":"en", // overlap language in sdk key.
        }
      },
      { 
        authHeader:'[your basic auth header]', // echo -n "appkey:appsecret" | base64, get from ab developing team by biz developers, and this header has a one-one relationship with application.
        baseUrl:"abc" // base url point to strategy distribution engine, usually read THEMIS_ADDRESS from env(via shuvi) and pass it to sdk here
      }, 
      config => {  //subscriber callback, can default 
          /*
            you are getting the config(A) that stored in localstorage before init
            sdk will fetch latest config(B) regularly (according to "advice_polling_interval", if "advice_polling_interval" is 0, the interval will be 5 mins) from data center and config(B) will be stored in localstorage
            at the meanwhile config(A) is copied to memory
            sdk will keep providing config(A) unless it get a config(B) that contains "force:true".
            once sdk get a config contains "force:true", it will replace config(A) and config(B) with the latest config together.
            this callback and subscribe((config)=>{}) are the places where you can get latest config at the fist time
          */
      },
    )
  }catch(err){
    //init function may trigger signup and do a instantly fetch from data center, any error thrown during this process, shall be handled by developers
  }

  /*
    act in the same way with the third parameter of init except one case:
    sdk will deliver available config at once and the callback you passed to init will get the config right after init
    while the subscribe would always be invoked after init, so it would not get the config at once, it would get config when the first polling complete.
  */
  helper.subscribe(yourSubscriber）
       
  /* remove a subscribe function from helper's sub queue */
  helper.unsubscribe(yourSubscriber）) 

  /* 
    <IExportConfig>, you can only access this method after init the sdk
    always return config(A) from memory directly, without any action to sync with remote api.
    it not the place that you can get latest config at the first time
  */
  const currentConfig = helper.getAbConfig()
  })  
  
  /* 
    evt:
    {type:"access",path:"The path you use to access a certain property of the configuration",value:"the property you would access"}
    -------------------break change--------------------
    sdk will no longer send error event, as using event to handle error will introduce potential vulnerability to code. please use try catch to wrap all the api invocations.
    ---------------------------------------------------
  */

  /*
    log your access to big data center, what would the evt be like, please see Data Instruction below
  */
  helper.onEvent((evt) => { 
  })

  /* remove customized event callback, and help will behave in default way */
  helper.removeEventCallback() 
  

  /* 
    get config instantly for specific strategies, and the config you get will be merge into cached config and the config you can get from getAbConfig. but it won't influence the normal polling.
  */
  helper.getStrategyConfigInstantly([1,2,3])
})()

    
```

[Live Demo](https://abtest-admin.fe.devfdg.net/en/example)

[Demo Code](https://git.toolsfdg.net/mono/mono/blob/master/web/apps/abtest-admin-ui/src/pages/Example.tsx)

--------------

## Data Instruction

The config you get is an level-recursively proxied agent.
every access to any level of this agent would be record like this:
![sdk config demo](https://static.devfdg.net/static/themis/themis-sdk-access-noti-demo.png)

and this feature may enable you to record behavior

```json
{
  "flows":{ //flows test config your application matched
    "a1": "777",
    "a2": "888",
    "a3": "999",
    "a4": "000",
    "label": "666"
  },
  "conditions": { //condition test config your application matched
    "aa": "cc",
    "color": "green",
    "rrr": "43256"
  },
}

```

-------------
## Related Docs

[服务端与SDK交互接口/server api for SDK](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=74030643)

~~[Clickhouse打点收口/Clickhouse log](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=77471585)~~