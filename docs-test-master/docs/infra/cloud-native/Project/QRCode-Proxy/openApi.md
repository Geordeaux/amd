---
id: openApi
title: Open Api
---

# GetContent
Method：POST

Prod： [www.binance.com](https://www.binance.com/)
Qa ： [www.qa1fdg.net](https://www.qa1fdg.net/)
Dev：  [www.devfdg.net](https://www.devfdg.net/)

Uri：/bapi/fe/qrcode/get_content

## Request
```jsx
{
    "qrCode" : "xxxxxxxxx",
}
```

## Response
```jsx
{
  "code": "000000",
  "message": "",
  "data": {
    "qrCode": "DEEPLINK443a643dbeb34983b47fcc023831b44b",
    "status": "SCAN",
    "actionType": "DEEPLINK",
    "confirmContent": null,
    "deepLinkContent": {
        "path": "/p2p/advShare?param1=value1|m2=value2"
    },
    "extendInfo": null,
    "enableUrlRedirect": true,
        "urlContent": {
        "path": "/download?key1=value1&key2=value2",
        "domainPrefix": "www"
      }
    },
    "success": true,
    "messageDetail": null
}
```
## Response code

We support Binance QRCode And ThirdParty-QRCODE
this doc is just for ThirdParty-QRCODE.
So some field are useless for you except deepLinkContent

| Filed   | 含义     |
| :----- | :---------------------- |
| deepLinkContent | Path is Deeplink Configured with custom parameters                  |


| Code | 含义                                                         | 推荐处理方式 |
| --------- | :----------------------------------------------------------- | :------ |
| 000000     | ok |     |
| 000001     | unknown error | throw error msg    |
| 000002     | illegal parameter | invalid QR code    |
| 000003     | external service error | throw error and retry    |
| 100001005     | login required | need login     |
