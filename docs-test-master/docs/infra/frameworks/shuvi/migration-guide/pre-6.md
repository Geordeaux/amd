---
title: Migration Guide (version rc.32)
sidebar_label: pre-6
---
# Shuvi Migration Guide

This guide describe how to upgrade shuvi from `'0.0.1-rc.36'` to `'0.0.1-pre.10'`

------------
## Step 1: Basic changes
caviar will be removed during upgrading.
### 1. Remove directory `caviar.config/`
### 2. Remove caviar from `BUILD.bazel` and `deps.bzl`
1. Change `"//libs/caviar-shuvi-preset"` to `"//libs/shuvi-webpack5-hack"`
2. Change `"//libs/shuvi-preset-binance"` to `"//libs/shuvi-preset-binance-v2"`
### 3. Remove caviar from npm scripts
Replace all the npm scripts containing ***caviar*** with corresponding scripts in the following
```json
{
  "start": "NODE_OPTIONS=--max_old_space_size=4096 shuvi dev --port 3000",
  "start:cache": "RENDER_CACHE_ENABLED=true yarn start",
  "build": "NODE_OPTIONS=--max_old_space_size=4096 shuvi build",
  "build-analyze": "yarn build --analyze",
  "serve": "shuvi serve --port 3000 --host 0.0.0.0",
  "serve:cache": "RENDER_CACHE_ENABLED=true yarn serve",
  "start:electron": "ELECTRON=true shuvi dev",
  "build:electron": "NODE_OPTIONS=--max_old_space_size=4096 ELECTRON=true shuvi build --target=spa",
  "build:electron-analyze": "yarn build:electron --analyze",
  "inspect": "shuvi inspect --verbose"
}
```
### 4. Remove caviar from dependencies

remove all dependencies about ***caviar***, eg:
```json
{
  "caviar": "^6.0.1",
  "@caviar/cli": "^2.1.1",
  "@caviar/sandbox-env-plugin": "^2.0.1"
}
```
### 5. Add specific version of shuvi in dependencies
Execute `yarn add shuvi@0.0.1-pre.10` at your app path.

### 6. Change preset at `shuvi.config.js`
Change `'@binance/shuvi-preset-binance'` to `'@binance/shuvi-preset-binance-v2'`

--------------
## Step 2: Changes on import path
In the old version of shuvi, shuvi exports types, constants and modules through various packages such as `'shuvi'`, `'@shuvi/types'`, `'@shuvi/core'`, `'@shuvi/app'` which seems to be messy.

So in the new version of shuvi, all the exports have converged to `'@shuvi/app'` for runtime and `'shuvi'` for non-runtime.

In this way, imports that come from `'@shuvi/app'`, `'@shuvi/app/xxx'` and `'shuvi'` don't need change.

For those imports that from `'shuvi/lib/constants'`, `'@shuv/types'`, `'@shuvi/core'` or other `'@shuvi/xxxx'`,

- If it is used at a runtime module, which means the module is a runtime-plugin or it locates at `src` directory, change this import path to `'@shuvi/app'`.

- Otherwise, change this import path to `'shuvi'`. Non-runtime situation contains plugins and presets.

More specifically, check the table below

| Runtime | old                                                                         | new                                                                    |
| ------- | --------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Yes     | `import { IInitAppPlugins } from '@shuvi/core'`                             | `import { Runtime } from '@shuvi/app'` <br/> `Runtime.IInitAppPlugins` |
| Yes     | `import { matchRoutes } from '@shuvi/core/lib/app/app-modules/matchRoutes'` | `import { matchRoutes } from '@shuvi/app'`                             |
| Yes     | `import { Runtime } from '@shuvi/types'`                                    | `import { Runtime } from '@shuvi/app'`                                 |
| Yes     | `import { xxx } from '@shuvi/types/src/runtime'`                            | `import { Runtime } from '@shuvi/app'` <br/> `Runtime.xxx`             |
| No      | `import { xxx } from '@shuvi/types'`                                        | `import { Runtime } from 'shuvi'`                                      |
| No      | `import { xxx } from 'shuvi/lib/constants'`                                 | `import { xxx } from 'shuvi'`                                          |

--------------
## Step 3: Changes on routes
In the new version of shuvi, the route path that matches all has changed from `'/*'` to `'/_other(.*)'`.

So you need to edit the file `shuvi.config/routes.js` at your app path. e.g.

- old
 ```javascript
 {
   path: '/*',
   component: 'notFound',
 },
 ```
- new
  ```javascript
  {
    path: '/_other(.*)',
    component: 'notFound',
  },
    ```

--------------
## Step 4: Changes on serverMiddleware and `api.addServerMiddleware`

In the new version of shuvi, the format of server middleware has changed from koa-style to express-style.

- old
  ```javascript
  export default (ctx, next) => {
    ctx.body = '200 OK';
  };
  ```
- new
  - new serverMiddleware is base on node request and response
  - `req`: An instance of [http.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage)
  - `res`: An instance of [http.ServerResponse](https://nodejs.org/api/http.html#http_class_http_serverresponse)
   ```javascript
    export default (req, res, next) => {
      res.statusCode = 200;
      res.end('200 OK');
    };
   ```

--------------
## Step 5: Changes on `api.addAppFile`
The function signature of `api.addAppFile` has changed.
- old
  ```javascript
  api.addAppFile(File.file(fileName, {
    content: () => `export []`,
  }))
  ```
- new
  ```javascript
  api.addAppFile({
    name: fileName,
    content: () => `export []`,
  })
  ```

And added files will be under `./shuvi/app/files` instead of the previous `.shuvi/app`. Please note that this may affect your code.

## Step 6: Running and testing
Once the above is done, you need to run and test your app locally.

Then you need to do a regression testing at qa/dev environment to make sure the upgrading is no problem.

Finally, merge the changes to `master` branch and verify your app at grayscale and production environment.

--------------
## Support
Please feel free to contact us if you encounter any issues.

Shuvi team: Myron.li, Repraance N, Tom W
