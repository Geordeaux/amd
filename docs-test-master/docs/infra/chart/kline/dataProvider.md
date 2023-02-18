---
id: dataProvider
title: Data
---

DataProvider提供了k线业务拉取历史数据和连接socket数据类，负责实现具体和后端交互的逻辑操作

#### getBarDatas(params, callback)

k线初始化和分段拉取历史数据时，会调用getBarDatas方法，将interval以及symbol等属性回传过来，通过callback接受数据

#### subcribe(params, callback)

订阅ws数据

#### unSubcribe(params）

取消订阅ws

```ts
type SyncParams = {
  startTime?: number
  endTime?: number
  limit?: number
  interval: string
  symbol:  string | { [k: string]: string | number }
}

// data: k线数据
// isLoadAll: k线接口数据是否全部拉完(左移分段加载数据)
interface LoadDataCallback{
  (params: { data: CandlestickItem[]; isLoadAll: boolean }): void
}

abstract class DataProvider {
  public abstract getBarDatas(params: SyncParams, callback: LoadDataCallback): void
  public abstract subcribe(params: SyncParams, callback: (data: CandlestickItem) => void)
  public abstract unSubcribe(params: SyncParams)
}
// example:
class CustomDataProvider extends DataProvider {
  private _socket: WebScoket
  function getBarDatas(params = {}, callback) {
    const queryString = Object.keys(params)
      .map(key => `${key}=${params[key]}`)
      .join('&')
    fetch(`https://www.binance.com/api/v1/klines?${queryString}`)
      .then(res => res.json())
      .then(res => {
        const result = res.map(([t, o, h, l, c, v, _, q]) => ({ time: +t, open: +o, high: +h, low: +l, close: +c, volume: +v,quantity: +q}))
        callback({ data: result, isLoadAll: false })
      })
  }
  function subcribe({ interval, symbol }, callback) {
    this._socket = new WebSocket(`wss://stream.binance.com/stream?streams=${symbol.toLowerCase()}@kline_${interval}`)
    try {
      this._socket.onmessage = e => {
        const res = JSON.parse(e.data)
        const { t, o, h, l, c, v, q } = res.data.k
        callback({ time: t, open: +o, high: +h, low: +l, close: +c, volume: +v, quantity: +q })
      }
    } catch (e) {
      console.error(e.message)
    }
  }

  function unSubcribe() {
    this._socket && this._socket.close()
    this._socket = null
  }
}

return <BasicKline dataProvider={new CustomDataProvider()} />
```

也可提供对象形式的Dataprovider， 例如

```js
const dataProvider = React.useMemo(() =>{
    let _socket
    function getBarDatas(params = {}, callback) {
      fetch(url)
        .then(data => {
          callback({ data, isLoadAll: false })
        })
    }
    function subcribe({ interval, symbol }, callback) {
      _socket = new WebSocket(`wss://stream.binance.com/stream?streams=${symbol.toLowerCase()}@kline_${interval}`)
      try {
        _socket.onmessage = e => {
          const res = JSON.parse(e.data)
          const { t, o, h, l, c, v, q } = res.data.k
          callback({ time: t, open: +o, high: +h, low: +l, close: +c, volume: +v, quantity: +q })
        }
      } catch (e) {
        console.error(e.message)
      }
    }

    function unSubcribe() {
      _socket && _socket.close()
      _socket = null
    }
    return {
      getBarDatas,
      subcribe,
      unSubcribe,
    }
  }, [])
```
