---
id: methods
title: Methods
---

通过传递ref属性可以取到k线实例，调用实例方法，用法：

```js
const candleRef = useRef()

useEffec(() => {
  const candlestickInstance = candleRef.current?.getCandlestick()
  candlestickInstance?.xxx()
}, [])

return <BasicKline ref={candlesRef} />
```

指标框打开或者关闭(非受控)

```ts
candlestickInstance.showIndicatorsDialog() 
```

添加指标

```ts
candlestickInstance.addIndicatorByName(name, config: { params, styles })
```

删除指标

```ts
candlestickInstance.closeIndicatorByName(name: string)
```

获得所有支持的指标

```ts
candlestickInstance.getAllIndicators()
```

设置指标可见性

```ts
candlestickInstance.setIndicatorVisibleByName(name: string, visible: boolean)
```

添加自定义指标

```ts
candlestickInstance.registerCustomIndicator(name, indicatorBase)

// eg

type Result = {
  chartType: string
  data: Array<{time: number, value: number}>,
  ...
}

const indicatorName = 'test'
const customIndicator = {
  defaultProps: {
    title: name,
    isHistBase: false,
    isScaleCenter: true,
    isCustom: true,
  },
  getResult(seriesData, params): Result[] {
    const highData = data.map(({ time, high }) => ({ time, value: high }))
    return [{ chartType: 'LINE', data: res, color: '#ff0000' }]
  },
  getLabel(time: number, results: Result[]) {
    return [{ label: name }]
  },
}

```

去掉自定义指标

```ts
candlestickInstance.removeCustomIndicatorByName(name)
```

设置币对信息(成交量title)

```ts
candlestickInstance.setSymbolInfo(params: { baseAsset: string, quoteAsset: string })
```

往前移

```ts
candlestickInstance.goForward()
```

往后移

```ts
candlestickInstance.goNext()
```

放大

```ts
candlestickInstance.zoomIn(data)
```

缩小

```ts
candlestickInstance.zoomOut()
```

加载保存数据

```ts
candlestickInstance.loadChart()
```


### 订阅事件

```ts
candlestickInstance.on(event_name, [namespace], callback)
// eg

chartReady: void  // k线数据加载完毕
afterTimeShifted: {
  minTime: number // k线数据最小时间
  maxTime: number // k线数据最大时间
  shiftedMinTime: number // k线时间轴最左侧时间
  shiftedMaxTime: number // k线时间轴最右侧时间
} // k线平移事件
showIndicatorDialog: void // 指标对话框展示隐藏事件
saveChartNeeded: { data: StoreData[] } // k线指标、标注等自定义数据保存事件
selectAnnotation: { data: Annotation } // 选取标注事件
unSelectAnnotation: void // 取消选取标注事件
allIndicatorsChange: { data: IndicatorDescription[] } // k线支持所有的指标改变事件
selectIndicatorsChange: { data: Array<{ name: string; isMain: boolean }> } // 所选指标改变事件
indicatorDialogVisibleChange: { data: boolean } // 指标对话框可见性事件
customIndicatorClick: { data: { name: string; isMain: boolean } } // 自定义指标点击事件

useEffect(() => {
  candlestickInstance.on('saveChartNeeded', 'spot', ({ data }) => {
    localstorage.setItem('spot.kline', JSON.stringify(data))
  })
  candlestickInstance.on('chartReady', 'spot', () => {
    const storage = JSON.parse(localstorage.getItem('spot.kline')
    candlestickInstance.loadChart(storage)
  })
  return () => {
    candlestickInstance.off('saveChartNeeded', 'spot')
    candlestickInstance.off('chartReady', 'spot')
  }
}, [])

```



