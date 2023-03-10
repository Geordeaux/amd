---
id: mp-migration
title: Miniprogram Migration From `bmp` to `shuvi`
sidebar_label: Migrate from bmp cli
---

### Add specific version of shuvi in dependencies

```json
"dependencies": {
  "shuvi": "0.0.1-pre.10"
},
```

### edit `package.json` `scripts`
```json
"scripts": {
"dev": "shuvi dev",
"build": "shuvi build"
},
```

### generate configFile `shuvi.config.js`

config from project/bmp.config/index
```javascript
const path = require('path')

// page routes
const pages = [
  'pages/mp/home/index',
  'pages/mp/customAllocation/index',
  'pages/mp/pendingHarvest/index',
  'pages/mp/launchpad/index',
  'pages/mp/subscription/index',
  'pages/mp/purchase/index',
  'pages/mp/launchpool/index',
  'pages/mp/records/index',
  'pages/mp/lottery/index',
  'pages/mp/lpDetail/index',
  'pages/mp/webview/index',
  'pages/mp/subscriptionHoldingList/index',
  'pages/mp/holdingCoin/index',
]

const defaultConfig = {
  platform: {
    name: 'mp', // this is a miniprogram
    target: 'bmp', // work under name, enum ['bmp', 'weapp'], we will support more in future
  },
  // support web routes way, 
  // eg: { path: /:user/:name, component: pages/mp/home/index }
  // must navigate by methods under useRouter()
  // first routes will be index page, can be modify in app.config.js['default']['entryPage']: 'pages/mp/home/index'
  routes: pages.map(pathName => {
    return {
      path: pathName,
      component: pathName,
    }
  }),
  plugins: [
    api => {
      api.tap('bundler:configTarget', {
        name: 'runtime-react',
        fn: (config, { name }) => {
          // your config from project/bmp.config/index
          config.resolve.alias.set('@binance/uikit-widget', path.resolve(__dirname, '../../libs/uikit-taro'))
          config.resolve.alias.set('@binance/uikit-core', path.resolve(__dirname, '../../libs/uikit-taro'))
          config.resolve.alias.set('@', path.resolve(__dirname, './src'))
          // no need alias react
          // config.resolve.alias.set('react', path.resolve(__dirname, path.resolve('../../nezha/web/node_modules/react')))
          config.resolve.alias.set('@binance/fingerprint', path.resolve(__dirname, './src/adapters/fingerprint'))
          config.resolve.alias.set('@binance/ws-http-sdk', path.resolve(__dirname, './src/adapters/ws-http-sdk'))
          // already support @shuvi/app apis, no need adapters
          // config.resolve.alias.set('@shuvi/app', path.resolve(__dirname, '../src/adapters/shuvi'))
          config.resolve.alias.set('@binance/uikit-core', path.resolve(__dirname, '../../libs/uikit-taro'))
          config.resolve.alias.set('@binance/uikit-widget', path.resolve(__dirname, '../../libs/uikit-taro'))
          config.resolve.alias.set('@binance/uikit-icons', path.resolve(__dirname, './src/adapters/icons/components'))
          config.resolve.alias.set('@binance/uikit', path.resolve(__dirname, './src/components/replaceUikit'))
          return config
        },
      })
    },
  ].filter(Boolean),
}

module.exports = defaultConfig
```

run command `yarn run dev`, then binance miniprogram cli load `project/dist/bmp`

### tips

1. support @shuvi/app apis, eg: import { getRuntimeConfig, useParams, useRouter, useCurrentRoute, RouterView, withRouter } from '@shuvi/app'
1. support web routes way, but must navigate by methods under useRouter()
1. component return no value should return null instead of undefined;
   ```javascript
   const Test = React.memo(({ children = null }) => children);
   ```
1. resolve file by extensions: a.bmp.js>a.js;
1. not support less
