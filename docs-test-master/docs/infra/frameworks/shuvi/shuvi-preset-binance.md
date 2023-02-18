---
id: shuvi-preset-binance
title: shuvi-preset-binance
---

`shuvi-preset-binance` allow you to maintain a shuvi project with less effort. It includes several recommend plugins.

## Installation

1. Add to bazel dependency
2. Configure shuvi

```diff
# BUILD.bazel
deps = [
    "//libs/caviar-shuvi-preset",
+   "//libs/shuvi-preset-binance",
    ":node_modules",
]

// shuvi.config.js
const config = {
  presets: [
+   ['@binance/shuvi-preset-binance', options],
  ],
}
```

> Notes: It is recommend to use generator to [create a new app](/docs/infra/shuvi/overview#create-a-new-app), and `shuvi-preset-binance` is included by default.

## Features
- Opinionated plugins includes:
  - Google Analytics and Google Tag Manager
  - Inject Font and Common CSS
  - Inject `COMMIT_HEAD` into runtimeConfig and set as ETAG
  - Favicon automatic setup
- Options that can be configured:
  - redux
  - serverMiddleware
    - csp
    - cacheResponse
  - nonce
  - ssrCache
- Others:
  - allow electron development

## Prerequisites
- Envs are needed to run this presets successfully
```.env
GTM_TRACKING_ID=xxx
GA_TRACKING_ID=xxx
```

## Options

```ts
interface Options {
  redux?: boolean
  nodeExternals?: WebpackExternalsPlugin['option']
  serverMiddleware?: ServerMiddlewareOptions
  nonce?: boolean
  ssrCache?: SSRCacheOptions
}
```

### Example

```js
// shuvi.config.js
[
  "@binance/shuvi-preset-binance",
  {
    redux: true,
    serverMiddleware: {
      csp: require("./shuvi.config/csp"),
    },
    ssrCache: {
      namespace: "shuvi-test-ssr",
      getKey: require("./shuvi.config/utils/getKeyByTheme"),
    },
    nonce: false,
  },
];
```

## Important Plugins

### @shuvi/plugin-redux

- *Enabled: controlled by options*
- *Options: boolean. Default: `true`*

To disable redux plugin:

```diff
# shuvi.config.js
[
  '@binance/shuvi-preset-binance',
  {
+   redux: false,
  },
],
```

If enabled, please provide a createStore method to `src/plugin.js`

```js
// src/plugin.js
// This is a file where shuvi load runtime plugins
import createStore from './model/store'

const initPlugins = ({ applyPluginOption }) => {
  applyPluginOption('redux', { createStore })
}

export default initPlugins
```

### @binance/shuvi-plugin-electron

- *Enabled: always*
> `process.electron` is set to true. See [Shuvi/Electron/Getting started](/docs/infra/shuvi/electron#getting-started) for more details.

### @shuvi/plugin-server-externals

- *Enabled: always*
- *Options: [WebpackExternalsPlugin['option']](https://github.com/liady/webpack-node-externals#configuration). Default: `undefined`*

Automatically use node-external for your packages in node.js environment. You can add additional options:

```diff
# shuvi.config.js
[
  '@binance/shuvi-preset-binance',
  {
+   nodeExternals: {
+     allowlist: ['uuid/v4'],
+   },
  },
],
```

### @shuvi/sourcemap-plugin

- *Enabled: always*

Enable sourcemap in production.

### @binance/shuvi-plugin-commit-head

- *Enabled: always*

Inject `COMMIT_HEAD` into **runtimeConfig**.

### plugins/shuvi-plugin-external-react

- *Enabled: always* (disabled in electron)

Add `react` and `react-dom` as webpack externals for web applications.

### plugins/shuvi-plugin-ga

- *Enabled: always* 
- Env needed:
```.env
GA_TRACKING_ID=xxx
```

To insert GA script:

```diff
# shuvi.config.js
[
  '@binance/shuvi-preset-binance',
  {
+   ga: { GA_TRACKING_ID: process.env.GA_TRACKING_ID },
  },
],
```

> Note: Make sure don't add duplicated script tag in `document.js`.

### @binance/shuvi-plugin-gtm

- *Enabled by default*
- Env needed:
```.env
GTM_TRACKING_ID=xxx
```


### plugins/shuvi-plugin-nonce

- *Enabled: controlled by options*
- *Options: boolean. Default: `undefined`*

To add csp `nonce` attributes to all `script` and `noscript` tags:

```diff
# shuvi.config.js
[
  '@binance/shuvi-preset-binance',
  {
+   nonce: true
  },
],
```

> Note: By adding nonce option, it will enable both [plugins/shuvi-plugin-nonce](#pluginsshuvi-plugin-nonce) and nonce of csp-middleware in [plugins/shuvi-plugin-server-middleware](#csp-middleware)

### plugins/shuvi-plugin-etag

- *Enabled: always*

To add `<meta http-equiv="etag" content="COMMIT_HEAD">`:

> Note: Make sure don't add duplicated script tag in `document.js`.

### plugins/shuvi-plugin-favicon

- *Enabled: always*

To add `<link rel="shortcut icon" type="image/x-icon" href="https://static.devfdg.net/static/images/common/favicon.ico">`:

To use a custom favicon:

- Env
```.env
FAVICON_PATH="https://www.favicon.com/favicon.ico"
```

```diff
# shuvi.config.js
[
  '@binance/shuvi-preset-binance',
  {
+   faviconHref: "https://www.favicon.com/favicon.ico"
# or using a env
+   faviconHref: process.env.FAVICON_PATH
  },  
]

```

### plugins/shuvi-plugin-fonts

- *Enabled: always*

Add fonts style

### plugins/shuvi-plugin-ssr-cache

It will only be applied when `RENDER_CACHE_ENABLED=true`. To add ssr cache:

- Env needed:
```.env
REDIS_WRITE=
REDIS_READ=
```

```diff
# shuvi.config.js
[
  '@binance/shuvi-preset-binance',
  {
+   ssrCache: {
+     namespace: "project-name-ssr",
+     getKey: require("./shuvi.config/utils/getKeyByTheme"),
+   },
  },
],
```

If enabled, it will attempt to connect to **redis** and fallback to memory cache.

> Note: [getKey](https://git.toolsfdg.net/michael-xu/shuvi-test/blob/master/shuvi.config/utils/getKeyByTheme.js) should return unique cache key.

### plugins/shuvi-plugin-sentry

- *Enabled: always*

Add env to enable:

```diff
# .env
+ SENTRY_DSN=... # Required
```

- Env Optional:

```.env
SENTRY_ENV= # Optional, sets the environment for Sentry
```

With `shuvi-plugin-sentry` plugin, you don't have to install any dependency. The plugin will:

1. Automatically initialize Sentry only in **prod**.
2. Automatically report errors at top of App via `componentDidCatch`.
3. Make `Sentry` available as a global variable, and you can use `Sentry` api anywhere.
4. `Sentry.captureException` is a noop function in **dev**.

### plugins/shuvi-plugin-server-middleware

- *Enabled: when options is defined*
- *Options: [ServerMiddlewareOptions](https://git.toolsfdg.net/mono/mono/blob/db1404d0fde6c0ca6bc78c04592cd23bb34f6679/libs/shuvi-preset-binance/src/plugins/shuvi-plugin-server-middleware.ts#L11). Default: `undefined`*

> Note: Server middleware will not be applied at build and inspect phase.

To apply built-in server middleware by setting:

```diff
# shuvi.config.js
[
  '@binance/shuvi-preset-binance',
  {
+   serverMiddleware: {},
  },
],
```

#### logger middleware

Override *console.log* with log4js

#### GET /health-check handler

Response 200

#### GET /favicon.ico handler

Response 200 without content

#### GET /version handler

Response `COMMIT_HEAD`

#### parseCookies middleware

Access via `req.parsedCookies`

#### cacheResponse middleware

Add `cache-control` to response header. The value will be `'max-age=120, must-revalidate'` for all pages by default. You can also customize each page by adding options:

```diff
# shuvi.config.js
[
  '@binance/shuvi-preset-binance',
  {
    serverMiddleware: {
+     cacheResponse: [
+       { handler: '/no-cache', options: false }, # 'no-cache, no-store, must-revalidate'
+       { handler: '/cached', options: 1 * 60 * 1000 } # 'max-age=60, must-revalidate'
+     ],
    },
  },
],
```

> Note: `cache-control` will be override if [ssr-cache middleware](#ssrcache-middleware) is enabled at same time.

#### csp middleware

It will only be applied in production. To add additional csp:

```diff
# shuvi.config.js
[
  '@binance/shuvi-preset-binance',
  {
    serverMiddleware: {
+     csp: {
+       all: [],
+       scripts: [],
+       styles: [],
+       connects: [],
+       imgs: [],
+       medias: [],
+       frames: [],
+     },
    },
  },
],
```

To enable nonce:

```diff
# shuvi.config.js
[
  '@binance/shuvi-preset-binance',
  {
    serverMiddleware: {
      csp: {
        all: [],
        scripts: [],
        styles: [],
        connects: [],
        imgs: [],
        medias: [],
        frames: [],
      },
    },
+   nonce: true,
  },
],
```

> Note: By adding nonce option, it will enable both [plugins/shuvi-plugin-nonce](#pluginsshuvi-plugin-nonce) and nonce of csp-middleware in [plugins/shuvi-plugin-server-middleware](#csp-middleware)

### plugins/shuvi-plugin-federation

- *Enabled: always*
Separation of SSR business header and footer micro services

- Env Need
```.env
CACHE_PROXY_HOST=xxx
```

> Note: if you use both of `Header` and `Footer`. The configuration of header and footer must be set to the same value at the same time.Otherwise, normal rendering cannot be performed 
```diff
# getFederation.js
const ignoreLngs = [/./i]
module.exports = ({ isDev }) => ({
  header: true,  # use micro header service
  footer: true,  # use micro footer service
  getParams: (_, appContext) => {
    const { req, locale = 'en' } = appContext
    const theme = req.headers ? req.headers['x-theme'] : 'light'
    const ignoreTheme = ignoreLngs.some(reg => reg.test(locale))
    return {
      type: 'com',
      origin: `https://${req.headers.host}`,
      currentTheme: ignoreTheme ? theme : '',
    }
  },
  getHeaders: isDev ? () => {} : null,
})
```

```diff
#app.web.js
  static async getInitialProps(ctx) {
    const {
      locale = 'en', // route language params key
    } = ctx.params
+   ctx.appContext.locale = locale
    //...
}
```

```diff
# shuvi.config.js
[
  '@binance/shuvi-preset-binance',
  {
+   federation: getFederation({ isDev }),
  },
],
```

> Note: `getParams` is micro services params. `getHeaders` Do not modify..
       




