---
id: configuration
title: Configuration
---

## k线配置信息

### 配置

|  name  |    props    |    description   |    eg    |
| :----: |   :-----:   |    :---------:   |   :----- :  |
|  精度   |   candlestick.tickSize  |  主图k线价格精度  | candlestick: { tickSize: 2 }  |
|  精度取整   |   candlestick.isFloor  |  主图k线价格精度是否向下取整  | candlestick: { isFloor: true }  |
|  主图类型 | candlestick.type | k线类型(Line \| Bar) | candlestick: { type: 'Bar' }  |
|  网格    | grid.show  |  是否显示网格 | grid: { show: false }  |
|  绘图工具栏 | isShowDrawPanel | 是否展示工具栏 | isShowDrawPanel: true |
|  自定义指标 | isShowCustomIndicator | 是否展示自定义指标 | isShowCustomIndicator: false |
|  主题  |  theme | 主题名称(light \| dark) | theme: 'dark'  |


### 覆盖颜色

```ts
const ColorTable = {
  background: ColorMap.White, // 背景色
  'candle.upBarColor': ColorMap.CandleGreen, // k线阳线柱子颜色
  'candle.downBarColor': ColorMap.CandleRed, // k线阴线柱子颜色
  'candle.lineColor': '#5a9ad8', // k线趋势线颜色(切换为分时线)
  'frame.borderLineColor': ColorMap.Gray50, // k线各区域边界线颜色
  'text.defaultColor': ColorMap.Gray600, // k线最高价格最低价格颜色
  'axis.tickLineColor': ColorMap.Gray300, // 价格轴和时间轴刻度线颜色
  'axis.tickTextColor': ColorMap.Gray300, // 价格轴与时间轴刻度文字颜色
  'cross.labelTextColor': ColorMap.White, // 价格轴markPrice文字颜色
  'cross.borderLineColor': ColorMap.Gray600, // 鼠标hover十字线颜色
  'cross.defaultLabelTextColor': ColorMap.Gray600, // 鼠标hover坐标轴label文字颜色
  'cross.defaultLabelBgColor': ColorMap.Gray10, // 鼠标hover坐标轴label背景颜色
  'cross.defaultLabelBorderColor': ColorMap.Gray50, // 鼠标hover坐标轴label border颜色
  'volume.upBarColor': ColorMap.Green300, // volume指标阳线柱子颜色
  'volume.downBarColor': ColorMap.Red300, // volume阴线阳线柱子颜色
  'volume.maLineColor': ColorMap.LineColor1, // volume指标ma线颜色
  'volume.upTextColor': ColorMap.Green500, // volume指标上涨文字颜色
  'volume.downTextColor': ColorMap.Red500, // volume指标下跌文字颜色
  'boll.fillColor': ColorMap.AreaFillColor1, // boll指标面积图填充颜色
}
```

ColorTable里提供的配置信息可以覆盖， 通过传递customTheme属性来自定义样式

eg

```ts
const customProps = {
  customTheme: {
    light: {
      'candle.upBarColor': '#FF0000'
      // ...more
    },
    dark: {
      'candle.upBarColor': '#FF0000'
      // ...more
    }
  },
  candlestick: {
    tickSize: 4,  // 精度
    type: 'Line',   //线图
  },
  grid: {
    show: false,  // 网格是否展示
  },
  isShowDrawPanel: true, // 绘图栏是否展示
  theme: 'dark',  // k线主图
}

return <BasicKline {...options, ...customProps } />
```
