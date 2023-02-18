---
id: arch-webview-app
title: WebView based WebApp Framworks
sidebar_label: WebView App
---

import Mermaid from '@theme/Mermaid'

## Introduction

WebApp framework support the normal webapp as a mini program application running in the WebView.

WebApp framework re-uses the bridge of mini program framework to call plugins of native side. So it's a simple version of mini program.

## JS-SDK

WebApp framework will inject a js sdk to webview in the runtime. The developers can call the js sdk to call plugins to use native capablity of Binance app.

![js-sdk-validation](https://static.devfdg.net/static/mono-static/docs-ui/img/arch/js-sdk-validation-flow.jpg)

Validator API is used to verify the parameters to initialize the JSSDK. The process :

1. Native request validation api with parameters ( clientId, appId, url, timestamp, nonceStr, signautre )
2. Golang service will check if the url matches the domain set by the developer in the application.developmentSettings.jsSecureDomain ( string[] )
3. Golang service will send the parameters (clientId, url, timestamp, nonceStr, signature) to Token Service (backend) to validate if the signature is valid
4. Return the state and expiration time to the native as response.


### Related api

**POST** /bapi/mp-api/v1/apps/verify-js-signature 

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

**POST** /oauth-api/v1/public/verify-js-signature?nonce={}&clientId={}&timestamp={}&url={}&signature={}

```ts
interface Response {
  "code":"000000",
  "message":null,
  "data": {
   "verifyResult":true,
   "expireTime":1597678265592
   },
 "success":true
}
```

Or if ticket is expired
```ts
{
"code":"050003",
"message":"Ticket is expired.",
"data":null,
"success":false
}
```

## Related documentation

https://docs.google.com/document/d/1PL1iq1xrxWb3HldPuze8EESt5F527b9Yw7X-CAvm_Ew/edit

## Upgrade JS-SDK Process.
First, you need to know the sdk is running on webView and mini-program can call api through it.

Before elaborating processes, need to know that.
1. sdkVersion : Request to run mini-program minimum version of sdk.
2. specifiedSdkVersion : Request to run mini-program at the specific version of sdk.

There are 4 processes.

1. Background upgrade : When user opens binance app, if there is no default sdk, it will unarchive the sdk.zip(default-common-sdk.zip in Android) and set it as `default sdk`.
Then deleting sdk files except `default sdk`.
After requesting `apps/sdk?currentVersion= "default-sdk-version"`, it will update default sdk depends on updateDefaultSdk which is from backend. 
![Background upgrade process](https://static.devfdg.net/static/mono-static/docs-ui/img/arch/mutiple-sdk-background-sdk-process.png)

2. Use specific version of sdk for mini-program : In `detail` api, there is specifiedSdkVersion in response. If specifiedSdkVersion is not empty, we will load specifiedSdkVersion to run mini-program.
![Specific version process](https://static.devfdg.net/static/mono-static/docs-ui/img/arch/mutiple-sdk-open-mp-specific-version.png)

3. Use preview version of sdk for mini-program : The channel which is not published is preview channel. It will request `app/sdk` and update defaultSDK base on updateDefaultSDK. After that, it compares defaultSDK with `sdkVersion` to determine should the process continue.
![Preview version process](https://static.devfdg.net/static/mono-static/docs-ui/img/arch/mutiple-sdk-open-mp-review-version.png)

4. Use formal version of sdk for mini-program : The channel is published. If defaultSDK satisfies `sdkVersion`, it won't request to server, just use defaultSDK to run mini-program.
If defaultSDK doesn't satisfy `sdkVersion`, it will request `apps/sdk` api, and update defaultSDK base on updateDefaultSDK. After that, it compares the defaultSDK with `sdkVersion` to determine should the process continue.
![Formal version process](https://static.devfdg.net/static/mono-static/docs-ui/img/arch/mutiple-sdk-open-mp-formal.png)
