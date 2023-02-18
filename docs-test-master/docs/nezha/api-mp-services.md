---
id: api-mp-services
title: Api for Services
sidebar_label: Services
---

## API_HOST

* dev: https://www.devfdg.net/
* qa: https://www.qa1fdg.net/
* prod: https://alb.fe-com-istio-api.bin.internal/ (internal service)

## APIs

### `POST` /bapi/mp/v1/internal/deeplinks

Get deeplinks, mininum Android/IOS version required for apps.

* params
```ts
{
  items: Array<{
    appId: string // mini=program appId
  }>
  platform: string // andriod or ios.
  appVersion: string // currently installed app version.
}
```

* response
```ts
Array<{
  appId: string
  deeplink: string
}>
```
