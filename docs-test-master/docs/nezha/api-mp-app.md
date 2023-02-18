---
id: api-mp-app
title: Api for App
sidebar_label: App
---

## API_HOST

* dev: https://www.devfdg.net/
* qa: https://www.qa1fdg.net/
* prod: https://www.binance.com/
## Applications

### `GET` /bapi/mp-api/v1/apps/search

search **published** mini program

* params

```ts
{
  text: string
  page: number
  count: number
}
```

* response

data payload
```ts
Array<{
  appId: string
  name: string
  description: string
  avatar: string
  version: string
  revision: string
  isBinanceApp: boolean
}>
```
**Note**: An app's info will be returned only if it is currently **ACTIVE**.


### `GET` /bapi/mp-api/v1/apps/:appId/details

get detail of app that has been **published** by appId
* params

**Note**: You can use the `version` and `revision` to calculate that whether need to download or update, 
if native requests this api for the first time, these two params are `null`.

```ts
{
  appId: string
  version?: string
  revision?: string
  channel?: string
}
```

* response

```ts
{
  data: {
    appId: string // the id of mini-program
    clientId: string
    name: string // name of the mini-program
    description: string // description of the mini-program
    sdkVersion: string // minimum sdk version to run the mini-program
    avatar: string
    needUpdate: boolean // tell native whether need download resource, the default value is `true`
    downloadUrl: string // includes the resource of mini-program
    timestamp: string // the publish time of resource, unit: Unix timestamp
    maxAge: number // the max age of use local cache, unit: millisecond
    version: string // the version of Nezha(MP).
    revision: string // md5 value of the resource.zip file
    oauthRedirectUri: string // **Will be removed in the future** the redirectUri for oauth service
    oauthScope: string // scope of oauth.
    isActive: bool // whether app is currently active.
    permissions: Array<{
      name: string // permission name
      args: Array<string>
    }>
    developmentSettings: {
      serviceDomain: Array<string> // Valid service domains, only domains in the list that the mp can initiate a valid request (bn.request, bn.connectSocket, bn.uploadFile, bn.downloadFile).
      webViewDomain: Array<string> // Valid webview domains, only domains in the list that can be open in the web-view of mp.
      certificates: Array{
        domain: string // domain
        keys: Array<string> // array of base64 encoded key content

       }
       preFetchUrl: string // used for the pre-fetch
       backgroundFetchUrl: string // used for background-fetch
    }
  }
  code: string // error code
  success: bool // whether the request succeed.
  message: string // error message
  messageDetail: null // always null for now.
}
```

#### Channels
The native side can request a certain channel from the details apis:

| Channel Name | Description                                                                                        | Permission                                                                    |
|--------------|----------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| published    | The version marked as the published for the application                                            | All the user                                                                  |
| auditing     | The version marked as the auditing version, and not reviewed yet                                   | Only the reviewers                                                            |
| experience   | The version marked as the experience version. Only the develop version can be marked as experience | Only the users of the application, and the users have the role of experience. |
| preview      | The version marked ad the preview version.                                                         | only the uploader him self can access to the preview channel                  |


#### Current Permissions
* **USE_BINANCE_APP_LOGIN**    
bn.login will use the Login process of Binance App; instead of the Oauth login for the 3rd part apps.

* **USE_PRIVATE_REQUEST_HEADERS**   
bn.request will include private headers, like the session of users

* **ALLOW_NO_NAV_CONTROLS**  
Mini program apps don't show the top right bottons if they have this permissions

* **USE_BINANCE_DYNAMIC_DOMAINS**  
Mini program apps with this permissions will get dynamic domains injected by the `process.env` with keys: 
  - STATIC_HOST: cdnUrl
  - API_HOST: domain
  - SITE_HOST: webviewUrl
  - WS_HOST: wssUrl
  - STATIC_HOST_SHARE: cdnUrlPub

* **CAN_JUMP_TO_DEEPLINK**  
Mini program can jump to a deeplink address inside binance app.


### `POST` /bapi/mp-api/v1/apps/verify-js-signature

Request: 
```ts
interface request {
  clientId: string
  appId: string
  url: string
  timestamp: number
  nonceStr: string
  signature: string
}
```

Response:
```ts
interface response {
	"data": {
		"verifyResult": boolean, // return from remove
		"expireTime": string
	},
	"code": string,
	"success": boolean,
	"message": string // message
}
```
