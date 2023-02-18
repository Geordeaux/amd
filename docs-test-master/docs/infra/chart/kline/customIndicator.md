---
id: customIndicator
title: CustomIndicator
---
## props

|  name   | type  | isRequired | default | desc |
|  :----:  | :----:  | :----:  | :----:  | :----: |
| defaultProps | DefaultProps | true | - |  -  |
| paramMetas | number | true | - | - |
| getResult | Function | true | - | callback(IndicatorResult[]) |
| subscribe | Function | true | - | callback(data) |
| getLabel | Function | true | - | Label[]  |
| getMobileLabel | Function | true | - | 同上(移动端) |
| unSusbcribe | Function | true | - | 取消订阅 |

### DefaultProps

```js
interface DefaultProps {
  readonly title: string // 指标标题
  readonly isHistBase: boolean // 是否是主图指标
  readonly isScaleCenter?: boolean // 坐标轴是否从中间延伸(区别底部往上延伸) 舍弃！！
  readonly isFormatNum?: boolean // 数字是否转换成(K, M, G...)带单位数字
  readonly isCustom?: boolean // 是否自定义指标
  readonly isDiffSource?: boolean // 是否数据源非k线主图数据
  readonly precision?: number // 指标面板小数精确位数
}
```

### ParamMetas

```js
enum ParamType {
  INT = 'INT',
  FLOAT = 'FLOAT',
  SOURCE = 'SOURCE',
  SELECT = 'SELECT',
}
enum Source {
  HIGH = 'high',
  LOW = 'low',
  OPEN = 'open',
  CLOSE = 'close',
}
interface ParamMetaBase {
  label: string
  key: string
}
interface ParamMetaNumber extends ParamMetaBase {
  type: ParamType.INT | ParamType.FLOAT
  defaultValue: number
}
interface ParamMetaSource extends ParamMetaBase {
  type: ParamType.SOURCE
  defaultValue: Source
}
interface ParamMetaSelect extends ParamMetaBase {
  type: ParamType.SELECT
  defaultValue: ParamType.INT | ParamType.FLOAT
  options: Array<ParamType.INT | ParamType.FLOAT | Source>
}
type ParamMeta = ParamMetaNumber | ParamMetaSource | ParamMetaSelect

// eg.
// input
{ type: 'INT', defaultValue: 0, label: 'Length', key: 'length1' }
// select
{ type: 'SELECT', defaultValue: 'open', label: 'source', key: 'source1', options: ['open', 'close'] }

```

### IndicatorResult

```js
interface IndicatorResult {
  data: PlotItem[] // 数据{ time, value }[]
  borderData?: PlotItem[] // just for area data
  chartType: ChartType // 类型 
  color?: string // 单一颜色直接传color
  colors?: ColorTableKeys[] // 每个数据对应的颜色，可实现不同颜色的线段，直方图，填充图
  labelColors?: ColorTableKeys[] // 每条数据对应的label文字颜色
  lineWidth?: number // 线宽
  zIndex: number // 图形叠加顺序(越大越往上)
  visible?: boolean // 图形是否可见，用于只显示label
  yAxisIndex?: string // 不同量级数据支持多个坐标轴，默认不传yAxisIndex的为默认坐标轴
}
enum ChartType {
  Line = 'LINE',
  Bar = 'BAR',
  CandleBar = 'CANDLEBAR',
  Area = 'AREA',
  Point = 'POINT',
  CrossLine = 'CROSSLINE',
  Transcation = 'TRANSCATION',
}
```

### Label
```js
interface Label {
  label: string
  key: string
  styles?: Partial<CSSStyleDeclaration>
}
```

```js
export const customIndicator = (() => {
  const defaultProps = {
    title: 'Test',
    isHistBase: false,
    isScaleCenter: true,
    isDiffSource: true,
  }
  let ws
  const paramMetas = [{ type: 'INT', defaultValue: 0, label: 'Length', key: 'asd' }]
  function getData(params, callback) {
    const queryString = ['symbol', 'interval', 'startTime', 'endTime']
      .filter(key => !!params[key])
      .map(key => `${key}=${params[key]}`)
      .join('&')
    fetch(`https://www.binance.com/api/v1/klines?${queryString}`)
      .then(res => res.json())
      .then(res => {
        const data = res
          .map(([time, open, high, low, close, volume, _, quantity]) => ({
            time: +time,
            open: +open,
            high: +high,
            low: +low,
            close: +close,
            volume: +volume,
            quantity: +quantity,
          }))
          .sort((a, b) => a.time - b.time)
        callback({ data, isLoadAll: false })
      })
  }
  function getResult(seriesData, _, callback) {
    const data = seriesData.map(({ time, close }) => ({ time, value: close }))
    const zeroBase = data[data.length - 1]?.value
    const bar = seriesData.map(({ time, close }) => ({ time, value: close }))
    callback([
      { data, chartType: 'LINE', color: 'line.color4', zIndex: 1 },
      {
        data: bar,
        chartType: 'BAR',
        colors: bar.map(({ value }) => (value > zeroBase ? 'volume.upBarColor' : 'volume.downBarColor')),
        labelColors: bar.map(({ value }) => (value > zeroBase ? 'volume.upTextColor' : 'volume.downTextColor')),
        zeroBase,
      },
    ])
  }
  function subscribe(params, callback) {
    ws = new WebSocket(
      `wss://stream.binance.com/stream?streams=${params.symbol.toLowerCase()}@kline_${params.interval}`,
    )
    try {
      ws.onmessage = e => {
        const res = JSON.parse(e.data)
        const { t, o, h, l, c, v, q } = res.data.k
        callback({ time: t, open: +o, high: +h, low: +l, close: +c, volume: +v, quantity: +q })
      }
    } catch (e) {
      console.error(e.message)
    }
    return () => {
      ws.close()
    }
  }
  function unSusbcribe() {
    ws && ws.close()
  }
  function getLabel(time, results) {
    return [
      { key: 'title', label: 'Test Ind' },
      ...results.map((result, index) => {
        const ind = result.data.findIndex(v => v.time === time)
        return {
          key: `${index}`,
          label: '' + result.data[ind]?.value,
          styles: { color: result.labelColors?.[ind] || 'line.color4' },
        }
      }),
    ]
  }
  function getMobileLabel() {
    return []
  }
  return {
    defaultProps,
    paramMetas,
    getData,
    getResult,
    subscribe,
    getLabel,
    getMobileLabel,
  }
})()

```

### Support to display mutilple bars

![asd](https://static.devfdg.net/static/mono-static/docs-ui/img/mutilple-bars.png)

```js
function getResult(seriesData, _, callback) {
  const data = seriesData.map(({ time, close }) => ({ time, value: close }))
  const zeroBase = data[data.length - 1]?.value
  const bar = seriesData.map(({ time, close }) => ({ time, value: close }))
  const volumes = seriesData.map(({ time, volume }) => ({ time, value: volume }))
  callback([
    { data, chartType: 'LINE', color: 'line.color4', zIndex: 1 },
    {
      data: bar,
      chartType: 'BAR',
      colors: bar.map(({ value }) => (value > zeroBase ? 'volume.upBarColor' : 'volume.downBarColor')),
      labelColors: bar.map(({ value }) => (value > zeroBase ? 'volume.upTextColor' : 'volume.downTextColor')),
      zeroBase: 0,
    },
    {
      data: volumes,
      chartType: 'BAR',
      colors: bar.map(({ value }) => (value > zeroBase ? 'volume.upBarColor' : 'volume.downBarColor')),
      labelColors: bar.map(({ value }) => (value > zeroBase ? 'volume.upTextColor' : 'volume.downTextColor')),
      zeroBase: 0,
      yAxisIndex: 'volume',
    },
  ])
}

```

