---
id: pre-6-server-middleware
title: Shuvi Server Middleware
sidebar_label: pre-6-server-middleware
---

## Replace caviar.config/router.js with shuvi serverMiddleware

> Please refer to full migration of **home-ui** in [PR#12399](https://git.toolsfdg.net/mono/mono/pull/12399/files).

Things to do:
- Remove `caviar.config/router.js`. We don't need `caviar.config/router.js`, `@binance/cacheable-ssr` and `@binance/roe-define-router` modules anymore.
  ```diff
  // caviar.config/config.js 
  Error.stackTraceLimit = Number.POSITIVE_INFINITY

  module.exports = {
    caviar: {
      plugins: [],
    },
  - router: require('./router'),
    port: 3000,
  }
  ```
- Rename folder `config` to `shuvi.config`
- Add serverMiddleware in `shuvi.config.js`
  - Switch *server middleware* from caviar to shuvi. Now there are also some [built-in middleware](/docs/infra/shuvi/shuvi-preset-binance#pluginsshuvi-plugin-server-middleware) defined in `@binance/shuvi-preset-binance`, and add options as follow:
  
  ```js
  // shuvi.config.js
  [
    "@binance/shuvi-preset-binance",
    {
      serverMiddleware: {
        csp: require('./shuvi.config/csp') // https://git.toolsfdg.net/mono/mono/blob/master/apps/oauth-ui/shuvi.config/csp.js
      },
      ssrCache: { // Note: remove this if your project does not use SSR redis cache
        namespace: "app-name-ssr",
        getKey: require('./shuvi.config/utils/getKeyByTheme'), // https://git.toolsfdg.net/mono/mono/blob/master/apps/oauth-ui/shuvi.config/utils/getKeyByTheme.js
      },
    },
  ];
  ```

See [Shuvi/shuvi-preset-binance](/docs/infra/shuvi/shuvi-preset-binance) for more details.

### Add Custom Server Middleware

If you want to add your own server middleware, you will need to add a custom `src/server.js` in shuvi and put the implementation of middleware in `src/api` directory.

```js
// src/server.js
import CommitVersion from './api/version'

export const serverMiddleware = [
  { path: '/en/shuvi-test/version', handler: CommitVersion },
].filter(Boolean)
```

## MISC

### COMMIT_HEAD

`@binance/shuvi-preset-binance` inject `COMMIT_HEAD` into `runtimeConfig`.

```diff
// shuvi.config.js
runtimeConfig: {
-  COMMIT_HEAD: process.env.COMMIT_HEAD,
}
```

### isDev

Replace `CAVIAR_DEV` with `NODE_ENV`:

```diff
- const isDev = process.env.CAVIAR_DEV === 'true'
+ const isDev = process.env.NODE_ENV === 'development'
```
