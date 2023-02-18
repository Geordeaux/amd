---
id: ws-http-sdk-api
title: api
---

## choose the proper api wrapping method before continue

| private request                     | public request                    | stream            |
| ----------------------------------- | --------------------------------- | ----------------- |
| [private request](#private-request) | [public request](#public-request) | [stream](#stream) |

## private request

1. you may step in the folder something like `business\privateRequest\trade`

2. you can create a file something like `getAsset.ts`
3. doing the following step in the `getAsset.ts` file
   - Include the `withPrivateRequest` module
   - Specify the `return type`、 `responseName`、 `argument` type. You might find the specification in the `mono\protos\schema\com\WsResp.proto| WsReq.proto`. You can ask jack.jiang@binance.com for details.
   - Fill with `service`、`argmentName` and `responseName`

```tsx
import { withPrivateRequest } from '../../../sdk'

interface IGetAsset extends Record<string, unknown> {
  balanceValuation: {
    [index: number]: {
      asset?: string
      assetName?: string
      free?: string
      locked?: string
      freeze?: string
      withdrawing?: string
      test?: number
      plateType?: string
      btcValuation?: string
    }
  }
}

export const getAsset = withPrivateRequest<
  IGetAsset,
  'balanceValuationResp',
  {
    asset?: string
    needBtcValuation?: boolean
  }
>({
  service: 'asset',
  argName: 'balanceValuationReq',
  responseName: 'balanceValuationResp',
})

```

 Example 👉: ./src/business/privateRequest/trade/getAsset.ts

## public request

1. you may step in the folder something like `business\publicRequest\trade`

2. you can create a file something like `getAsset.ts`
3. doing the following step in the `getAsset.ts` file
   - Include the `withPublicRequest` module
   - Specify the `return type`、 `responseName`、 `argument` type. You might find the specification in the `mono\protos\schema\com\WsResp.proto| WsReq.proto`. You can ask jack.jiang@binance.com for details.
   - Fill with `service`、`argmentName` and `responseName`

```tsx
import { withPublicRequest } from '../../../sdk'

interface IGetAsset extends Record<string, unknown> {
  balanceValuation: {
    [index: number]: {
      asset?: string
      assetName?: string
      free?: string
      locked?: string
      freeze?: string
      withdrawing?: string
      test?: number
      plateType?: string
      btcValuation?: string
    }
  }
}

export const getAsset = withPublicRequest<
  IGetAsset,
  'balanceValuationResp',
  {
    asset?: string
    needBtcValuation?: boolean
  }
>({
  service: 'asset',
  argName: 'balanceValuationReq',
  responseName: 'balanceValuationResp',
})

```

 Example 👉: ./src/business/publicRequest/trade/getAsset.ts

## stream

1. you may step in the folder something like `business\stream\trade`

2. you can create a file something like `getExchangeRate.ts`
3. doing the following step in the `getExchangeRate.ts` file
   - Include the 'withStream' module
   - Specify the `return type`、 `responseName` type. You might find the specification in the `mono\protos\schema\com\WsResp.proto| WsReq.proto`. You can ask jack.jiang@binance.com for details.
   - Fill with `channel` and `responseName`

```tsx
import { withStream } from '../../../sdk'

interface IExchangeRate {
  exchangeRate: {
    baseAsset: string
    quoteAsset: string
    time: number
    exchangeRate: number
  }[]
}

export const getExchangeRate = withStream<IExchangeRate, 'exchangeRateMsg'>({
  channel: 'exchange_rate',
  responseName: 'exchangeRateMsg',
})

```

 Example 👉: ./src/business/stream/trade/getExchangeRate.ts
