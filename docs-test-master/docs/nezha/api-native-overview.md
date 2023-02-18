---
id: api-native-overview
title: Native API Overview
sidebar_label: Overview
---

## Introduction

> We need the app to provide `native` api to the **mini-program worker** via `bridge` in order for the mini-program to have native capabilities.

## API call process

1. user call worker api `// just like mp.xxx(params)`
2. worker call native api via bridge(rpc) `// just like rpc.invoke(message)`
3. native call callbackFn if callbackId is exists 

## API Params

```ts
interface BridgeResponse {
  errMsg: string;
  data: object | null;
}

interface BridgeParams {
  [key: string]: string | number | object | Array<string | number | object>;
}

interface BridgeCommonParams {
  success?(res: BridgeResponse): void;

  fail?(res: BridgeResponse): void;

  complete?(res: BridgeResponse): void;
}
```

all the apis that **user call worker** have the followed format: 

```ts
mp.xxx(params: BridgeParams & BridgeCommonParams)
```

our native bridge has defined [**NativeRPCMessage**](arch-bridge.md#native-rpc-message) interface, so all the apis that **worker call app** have the followed format:  

```ts
nativeRPC.invoke(msg: NativeRPCMessage)
```
