---
id: electron
title: Electron
---

## Getting started

Add scripts as follow:

```diff
# package.json
{
  "scripts": {
+   "start:electron": "ELECTRON=true shuvi dev",
+   "build:electron": "ELECTRON=true shuvi build --target=spa"
  }
}
```

For local development: use the devtool by running:

```bash
$ DEVTOOL_URL=http://localhost:8080 yarn start:electron --port 3003
```

For local build: `OUTPUT_PATH` and `PUBLIC_PATH` can be set to modify the build result. E.g: 

```bash
$ PUBLIC_PATH=binance-resources://micro-app/xxx/ OUTPUT_PATH=xxx/electron-desktop/resources/app/xxx yarn build:electron
```
