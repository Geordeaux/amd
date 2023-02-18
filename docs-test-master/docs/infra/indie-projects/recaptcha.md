---
title: Recaptcha
---

## Overview

This is a library for distinguishing whether the current user is a real person or a bot.
If your business needs to prevent malicious users from using automated tools to achieve certain goals, then you may need to use this library to distinguish between real people and automated tools.

## Deployment
### Install
1. Download SDK from https://public.bnbstatic.com/static/js/captcha/captcha-sdk-v2.14.276.min.js
2. Import SDK to your page:
```html
<script src="captcha-sdk-v2.14.276.min.js"></script>
```

3. Call the initialization function to initialize the SDK
```javascript
const captcha = await initCaptchaSDK({
  bizId: '<your project business id>',
  staticHost: 'https://static.devfdg.net',
  apiHost: 'https://www.devfdg.net',
  lang: 'en',
  theme: 'auto',
})
```

### Configuration
The parameter configuration refers to the `SDKOption` object (key-value pairs) which is passed during the CAPTCHA verification.
It is the first parameter passed when calling the `initCaptchaSDK()`.

#### Params

| Property    | Type                      | Default Value         | Required | Description                                                                           |
|-------------|---------------------------|-----------------------|:--------:|---------------------------------------------------------------------------------------|
| bizId       | `string`                  |                       |    ✓     | Specify the business id of your project                                               |
| staticHost  | `string`                  |                       |    ✓     | Used to specify the location of the host for the static files                         |
| apiHost     | `string`                  |                       |    ✓     | Captcha's API backend url                                                             |
| i18nHost    | `string`                  | same as  `staticHost` |          | Used to specify the location of the host for the translation files                    |
| captchaHost | `string`                  | same as  `staticHost` |          | Used to specify the location of the host for the CAPTCHA static multimedia resources. |
| lang        | `string`                  |                       |    ✓     | Language of captcha UI                                                                |
| theme       | `'light', 'dark', 'auto'` |                       |    ✓     | Theme color of captcha UI                                                             |

## API
### `verify()`
call verify() to process the verification

```html
<div id="btn">Submit</div>
<script>
initCaptchaSDK({
  ...
}).then((captchaObj) => {
  document.getElementById('btn').addEventListener('click', function () {
    if (check()) { // Verify wether submission can proceed or not
        captchaObj.verify();
    }
  });
  captchaObj.onSuccess(function () {
    // Handle the logic for the user completing the verification
    // TODO
  })
});
</script>
```

### `getValidate()`
Use `getValidate()` if request has been applied to perform the HTTP request. Get the verification result from `onSuccess` if end user has successfully answer the captcha question. This result could be used to perform secondary verification in back-end. The `getValidate` will return an object, which contains `challenge` and `token`.

If the verification failed, the `getValidate()` will return `false`. You can decide whether to submit the form or continue with the second verification based on the verification result.

### `onSuccess(callback)`
Listen the verification success event.
Listen the verification success event to perform the secondary verification.
```javascript
const captchaObj = await initCaptchaSDK({
  ...
});
captchaObj.onSuccess(async ()=>{
  var result = captchaObj.getValidate();
  // ajax pseudocode. Secondary verification
  const resp = await fetch('/api/validate', {
    token: result.token,
    challenge: result.challenge,
    // Passing the data needed by server side, such as username, password, etc
  });
})
```

### `onError(callback)`
Listen the verification error event.
You can ask user to reload the webpage and retry the captcha if error occurs. Please find the example below.
```javascript
const captchaObj = await initCaptchaSDK({
  ...
});
captchaObj.onError(async ()=>{
  // Handle the logic when error occurs.
})
```

### `onClose(callback)`

When user closes the captcha challenge, it will trigger the callback function.

```javascript
const captchaObj = await initCaptchaSDK({
  ...
});
captchaObj.onClose(async ()=>{
  // User closes the CAPTCHA. For instance, you can prompt the user to complete the verification to proceed to the next step
})
```

## Typescript Declerations

If you are using Typescript, then this type declaration can help you use the SDK

```typescript
declare function initCaptchaSDK(option: initCaptchaSDK.SDKOption): Promise<initCaptchaSDK.CaptchaSDK>
declare namespace initCaptchaSDK {
  interface Result {
    token: string
    challenge: string
  }

  interface SDKOption {
    bizId: string
    staticHost: string
    apiHost: string
    i18nHost?: string
    captchaHost?: string
    lang: string
    theme: 'light' | 'dark' | 'auto'
  }

  class CaptchaSDK {
    constructor(option: SDKOption)
    getValidate(): Result | false
    verify(): void
    onSuccess(callback: () => void): void
    onError(callback: () => void): void
    onClose(callback: () => void): void
  }
}

```
## Demo

index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width,initial-scale=1.0" />
    <title>Captcha Demo</title>
    <style>
    </style>
    <script src="captcha-sdk-v2.14.276.min.js"></script>
  </head>
  <body>
    <div id="btn">Submit</div>
    <script src="demo.js"></script>
  </body>
</html>
```

demo.js

```javascript
initCaptchaSDK({
  bizId: 'bizId',
  staticHost: 'https://static.devfdg.net',
  apiHost: 'https://www.devfdg.net',
  lang: 'en',
  theme: 'auto',
}).then(captchaObj => {
  document.getElementById('btn').addEventListener('click', function () {
    captchaObj.verify()
  })
  captchaObj.onSuccess(function () {
    alert('result: ' + JSON.stringify(captchaObj.result))
  })
})
```

## Contacts

Front-end: Rabbit (chengfan.lin@binance.com)

Back-end: Daniel H (minhang.he@binance.com)
