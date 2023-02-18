---
id: overview
title: Overview
---

## Getting started

### Add package as follow:

```ts
import { BasicKline, DataProvider } from '@binance/candlestick
```

### Props

```ts
type BasicKlineProps = Partial<CandlestickProps> & {
  dataProvider: DataProvider // 提供api和ws数据
  interval: string // '1m' | '5m' eg...
  symbol: string | { [k: string]: string | number } // 'BTCUSDT' | { contractType: 'PERPETUAL', pair: 'BTCUSDT' }
  baseAsset?: string // 'BTC' 成交量指标title使用
  quoteAsset?: string // 'USDT' 成交量指标title使用
  namespace: string // k线内部存储的key，默认放在indexDB里面
  isStock?: boolean // 是否去掉成交量相关指标（stock-token-ui项目没有成交量，设为true)
  setLoading?(status: boolean): void // k线初始化拉数据时callback， 可以用来显示loading遮罩
  getI18n(key: stirng, config?: { defaultValue: string }): void // k线翻译从kline-ui中取(crowdin)
  dispatchError: (data: any) => void // k线错误上报方法，目前监控k线柱子断线
}
// 更多自定义信息查看配置信息
```

### 使用

```ts
const Kline = () => {
  const options = React.useMemo(() => {
    return {
      symbol: 'BTCUSDT',
      interval: '1m',
      baseAsset: 'BTC',
      quoteAsset: 'USDT',
      namespace: 'spot',
      isShowDrawPanel: true,
      theme: 'dark',
      setLoading: isLoading => console.log(isLoading),
      isStock: true,
      dataProvider: new DataProvider(),
    }
  }, [])
  return <BasicKline {...options} style={{height: '400px', width: '800px'}} />
}
```




