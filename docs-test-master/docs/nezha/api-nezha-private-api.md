---
id: api-nezha-private-api
title: Private APIs
sidebar_label: MP Private APIs
---

Private API on the mini program framework needs special permission:

```js
{
"name": "ADVANCED_ACTIONS",
"args": [
   "private-report-event",
   "private-two-fa",
   "private-get-user-profile",
   "private-kyc-show-dialog",
   ...
]
}
```

## Private APIS

### `__mp_private_api__.reportEvent`

Reports monitoring event.

#### Parameters

| Property | Type     | Default Value | Required | Description                |
| -------- | -------- | ------------- | :------: | -------------------------- |
| data     | `object` |               |    ✓     | Report data in json object |

#### Return Values

### `__mp_private_api__.twoFa`

Get the scene code information.

#### Parameters

| Property | Type     | Default Value | Required | Description |
| -------- | -------- | ------------- | -------- | ----------- |
| scene    | `string` | -             | ✓        | scene code  |

#### Return Values

**Object res**

| Property         | Type     | Default Value | Required | Description |
| ---------------- | -------- | ------------- | -------- | ----------- |
| emailVerifyCode  | `string` | -             | -        |             |
| googleVerifyCode | `string` | -             | -        |             |
| mobileVerifyCode | `string` | -             | -        |             |

### `__mp_private_api__.kycShowDialog`

Show KYC dialog and start KYC process.

#### Parameters

| Property | Type     | Required | Description                    |
| -------- | -------- | :------: | ------------------------------ |
| code     | `string` |    ✓     | error code from server side    |
| message  | `string` |    ✓     | error message from server side |

##### Value of `code`

| Value         | Description               |
| ------------- | ------------------------- |
| `"200003903"` | KYC denied                |
| `"200003904"` | KYC not completed         |
| `"200003905"` | KYC under review          |
| `"200003906"` | Unable to provide service |

### `__mp_private_api__.getUserProfile`

Get user profile.

#### Return Values

**Object res**

| Property | Type     | Default Value | Required | Description |
| -------- | -------- | ------------- | -------- | ----------- |
| uid      | `string` | -             | ✓        | user_id     |

### `__mp_private_api__.getAppSettings`

Get app settings.

#### Return Values

**Object res**

| Property        | Type     | Default Value | Required | Description      |
| --------------- | -------- | ------------- | -------- | ---------------- |
| currency        | `string` | -             | ✓        | currency         |
| paymentCurrency | `string` | -             | ✓        | payment currency |
