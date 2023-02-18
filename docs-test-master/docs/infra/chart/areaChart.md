---
id: areaChart
title: AreaChart
---

## props

|  name   | type  | isRequired | default |
|  :----:  | :----:  | :----:  | :----:  |
| width | number | false | - |
| height | number | false | - |
| xAxis | xAxisProps | true | - |
| yAxis | yAxisProps | false | - |
| series | series | true | true | - |
| tooltip | tooltipProps | false | - |
| legend | legendProps | false | - |
| background | string  | false | #ffffff |


## xAxisProps

|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  |:---: |
| data | Array<string \| number> | true | [] | - |
| format | string \| Function | false | `v => ${v}` | 'YYYY-MM-DD hh:mm:ss' when data is time list
| splitInterval | number \| (v, index, arr) => boolean | false | 70 | nearby distance of highlight points  |
| tickInterval | number \| (v, index, arr) => boolean | false | 70 | xAxis ticks nearby distance px  |
| boundaryGap | boolean | false | true | blank on both x-axis sides  |
| formmatTooltip | Function | false | - | tooltip formmat x Data

## yAxisProps

|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  |:---: |
| name | string | true | '' | tooltip show name |
| tickSize | number | false | 2 | data precision |
| visible | boolean | false | true | show or hidden y-axis |
| isLeft | boolean | false | false | y-axis is left or right |
| isFloate | boolean | false | false | y-axis tick text is above split line |
| formmat | Function | false | `v => ${v}` | yAxis tick will be fixed by tickSize when not provide format function
| min | number | false | undefined | yAxis extent min value |
| max | number | false | undefined | yAxis extent max value |

## tooltipProps

|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  |:---: |
| isTrigger | boolean | false | true | is trigger tooltip |
| type | line \| shadow \| cross | false | cross | tooltip type |
| arrow | boolean | false | true | is show tooltip arrow |
| crossLineColor | string | false | #F0B90B | cross line color |
| lineColor | string | false | rgba(0, 0, 0, 0.05) | line color |
| shadowColor | string | false | rgba(0, 0, 0, 0.05) | shadow fill color |
| shadowWeight | number[0-1] | false | 0.9 | shadow width weight |
| isShowAxisLabel | boolean | false | true | show tooltip axis labels(Area) |
| isShowHoverLine | boolean | false | true | show tooltip hover line or shadow |
| isShowLegend | boolean | false | true | show tooltip legend |
| isFloat | boolean | false | false | tooltip is placed another box |
| isFixTop | boolean | false | true | tooltip's position is fixed top(isFloat = true) |


## legendProps

|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  |:---: |
| show | boolean | false | false | is show legend |
| container | HTMLElement | false | null | legend wrap container |

## areaProps

|  name   | type  | isRequired | default |
|  :----:  | :----:  | :----:  | :----:  |
| type | string | true | 'LINE' |
| data | number[] | true | [] |
| color | string | true | #F0B90B |
| lineWidth | number | false | 1 |
| isShowBorder | boolean | false | false |
| isShowMarker | boolean | false | true |
| borderColor | string | false | '' |
| isCurve | boolean | false | false |
| formmatTooltip | Function | false | - |


```ts
() => {
  const limit = 360
  const [data, setData] = React.useState([])
  const xAxis = React.useMemo(() => ({
    data: data.slice(0, limit).map((_, index) => 1601481600000 + index * 60 * 1000 ),
    splitInterval: 0,
    boundaryGap: true,
    formmat: 'hh:mm',
  }), [data])
  const series = React.useMemo(() => [{
    name: 'Cumulative PNL',
    type: 'Area',
    color: '#6076FF',
    isCurve: false,
    isShowBorder: true,
    borderColor: '#02C076',
    lineWidth: 2,
    data: data.slice(0, limit).map(item => item.open)
  }], [data])
  const tooltip = React.useMemo(() => ({
    isTrigger: true,
    type: 'cross',
    isFloat: true,
    isFixTop: false,
    isShowHoverLine: true,
    isShowAxisLabel: true,
  }), [])
  const yAxis = React.useMemo(() => ({

    formmat: v => +v.toFixed(2)
  }), [])
  React.useEffect(() => {
    const t1 = setTimeout(() => {
      setData(CandlestickData)
    }, 100)
    return () => {
      clearTimeout(t1)
    }
  }, [])
  return(
    <Chart
      height={400}
      xAxis={xAxis}
      yAxis={yAxis}
      series={series}
      tooltip={tooltip}
    />
  )
}
```
## Area border is curve
```ts
() => {
  const limit = 20
  const xAxis = React.useMemo(() => ({
    data: CandlestickData.slice(0, limit).map((_, index) => index),
    boundaryGap: false
  }), [])
  const series = React.useMemo(() => [{
    name: 'Cumulative PNL',
    type: 'Area',
    color: '#AF7D08',
    isCurve: true,
    data: CandlestickData.slice(0, limit).map(item => item.open)
  }], [])
  return(
    <Chart
      height={400}
      xAxis={xAxis}
      series={series}
    />
  )
}
```

## gradient color

```ts
() => {
  const xAxis = React.useMemo(() => ({
    data: ['>10%', '7-10%', '5-7%', '3-5%', '0-3%', '0%', '0-3%', '3-5%', '5-7%', '7-10%', '>10%'],
    tickInterval: 0,
    boundaryGap: false,
  }), [])
  const yAxis = React.useMemo(() => ({
    tickSize: 0,
    formmat: v => `${v.toFixed(0)}%`
  }), [])
  const series = React.useMemo(() => [{
    name: 'Cumulative PNL',
    type: 'Area',
    isCurve: true,
    isShowBorder: true,
    borderColor: '#F0B90B',
    lineWidth: 2,
    data: [8, 12, 16, 8, 12, 3, 9, 15, 9, 5, 9],
    color: [[0, '#F8D12F'],
            [1, 'rgba(240, 185, 11, 0)']]
  }], [])
  return(
    <Chart
      height={400}
      xAxis={xAxis}
      yAxis={yAxis}
      series={series}
    />
  )
}
```

## xAxis is time data

```ts
() => {
  const limit = 50
  const xAxis = React.useMemo(() => ({
    data: CandlestickData.slice(0, limit).map((_, index) => 1569859200000 + index * 60 * 60 * 1000),
    formmat: 'hh:mm',
    boundaryGap: false,
    splitInterval: 0,
  }), [])
  const yAxis = React.useMemo(() => ({
    tickSize: 2,
    isFloate: true,
  }), [])
  const series = React.useMemo(() => [{
    name: 'Cumulative PNL',
    type: 'Area',
    data: CandlestickData.slice(0, limit).map(item => item.close),
    isShowBorder: false,
    borderColor: 'rgba(1, 179, 109)',
    lineWidth: 2,
    color: [[0, 'rgba(1, 179, 109)'], [1, 'rgba(74, 179, 133, 0.05)']]
  }], [])
  return(
    <Chart
      height={400}
      xAxis={xAxis}
      yAxis={yAxis}
      series={series}
    />
  )
}
```
