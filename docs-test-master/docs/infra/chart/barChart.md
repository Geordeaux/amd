---
id: barChart
title: BarChart
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
| formmat | string \| Function | false | `v => ${v}` | 'YYYY-MM-DD hh:mm:ss' when data is time list
| splitInterval | number \| (v, index, arr) => boolean | false | 70 | nearby distance of highlight points  |
| tickInterval | number \| (v, index, arr) => boolean | false | 70 | xAxis ticks nearby distance px  |
| boundaryGap | boolean \| number | false | true | blank on both x-axis sides  |
| formmatTooltip | Function | false | - | tooltip formmat x Data

## yAxisProps

|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  |:---: |
| name | string | true | '' | tooltip show name |
| tickSize | number | false | 2 | data precision |
| visible | boolean | false | true | show or hidden y-axis |
| isLeft | boolean | false | false | y-axis is left or right |
| isFloate | boolean | false | false | y-axis tick text is above split line |
| formmat | Function | false | `v => ${v}` | yAxis tick will be fixed by tickSize when not provide formmat function
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


## barProps

|  name   | type  | isRequired | default |
|  :----:  | :----:  | :----:  | :----:  |
| type | 'bar' | true | 'BAR' |
| data | number[] | true | [] |
| isShowValue | boolean | false | false |
| isShowMarker | boolean | false | true |
| histBase | number | false | 0 |
| barWeight | [0-1] | false | 0.8 |
| maxBarWidth | number | false | - |
| formmatTooltip | Function | false | - |


```ts
() => {
  const limit = 20
  const xAxis = React.useMemo(() => ({
    data: CandlestickData.slice(0, limit).map((_, index) => String.fromCharCode(index + 65)),
    formmatTooltip: v => v.repeat(3)
  }), [])
  const series = React.useMemo(() => [{
    name: 'Cumulative btc trend',
    type: 'Bar',
    barWidth: 10,
    data: CandlestickData.slice(0, limit).map(item => item.close),
    formmatTooltip: v => `${v}USD`
  }], [])
  const tooltip = React.useMemo(() => ({
    type: 'shadow',
    isFloat: true,
    shadowWeight: 1,
  }), [])
  const ref = React.useRef()
  const save = React.useCallback(() => {
    const instance = ref.current
    if (instance) {
      console.log(instance.getChartData('image/png', 0.2))
    }
  }, [])
  return(
    <>
    <Button type="primary" onClick={() => save()}>Save</Button>
    <Chart
      ref={ref}
      height={400}
      xAxis={xAxis}
      tooltip={tooltip}
      series={series}
    />
    </>
  )
}
```

## xAxis is time

```ts
() => {
  const limit = 20
  const xAxis = React.useMemo(() => ({
    data: CandlestickData.slice(0, limit).map((_, index) => 1569859200000 + index * 24 * 60 * 60 * 1000),
    formmat: 'MM/DD',
  }), [])
  const series = React.useMemo(() => [{
    name: 'Cumulative btc trend',
    type: 'Bar',
    data: CandlestickData.slice(0, limit).map(item => item.close),
  }], [])
  const tooltip = React.useMemo(() => ({
    type: 'shadow',
  }), [])
  return(
    <Chart
      height={400}
      xAxis={xAxis}
      tooltip={tooltip}
      series={series}
    />
  )
}
```

## custom xAxis text

```ts
() => {
  const xAxis = React.useMemo(() => ({
    data: ['>10%', '7-10%', '5-7%', '3-5%', '0-3%', '0%', '0-3%', '3-5%', '5-7%', '7-10%', '>10%'],
    tickInterval: 0,
  }), [])
  const yAxis = React.useMemo(() => ({
    tickSize: 0,
    visible: false,
    min: 0
  }), [])
  const series = React.useMemo(() => [{
    name: 'Cumulative btc trend',
    type: 'Bar',
    histBase: 0,
    barWeight: 0.6,
    isShowValue: true,
    data: [8, 12, 16, 8, 12, 0, 9, 15, 9, 5, 9],
    barColor: (v, index, arr) => index === Math.floor(arr.length / 2) ? '#E6E8EA' : index < Math.floor(arr.length / 2) ? '#02C076' : '#F84960',
  }], [])
  const tooltip = React.useMemo(() => ({
    isTrigger: false,
  }), [])
  return(
    <Chart
      height={400}
      xAxis={xAxis}
      yAxis={yAxis}
      tooltip={tooltip}
      series={series}
    />
  )
}
```

## histBase & barStyle

```ts
() => {
  const xAxis = React.useMemo(() => ({
    data: [10, 20, 60, 80, 100],
    formmat: (v, index, array) => index > 0 ? `${array[index - 1]}<=x<${v}` : `<${v}`
  }), [])
  const yAxis = React.useMemo(() => ({
    tickSize: 0,
    isLeft: true,
  }), [])
  const series = React.useMemo(() => [{
    name: 'Cumulative btc trendJDJAKDSKJSDjasdjsdjsdjd',
    type: 'Bar',
    histBase: 0,
    isShowValue: true,
    data: [-8, 12, 40, 32, -12],
    barColor: (v) => v > 0 ? '#02C076' : '#F84960',
  }], [])
  const tooltip = React.useMemo(() => ({
    type: 'shadow',
    isFloat: true,
    arrow: false
  }), [])
  return(
    <Chart
      height={400}
      xAxis={xAxis}
      yAxis={yAxis}
      tooltip={tooltip}
      series={series}
    />
  )
}
```

## maxValue and minValue is equal

```ts
() => {
  const xAxis = React.useMemo(() => ({
    data: [10, 20, 60, 80, 100],
    formmat: (v, index, array) => index > 0 ? `${array[index - 1]}<=x<${v}` : `<${v}`
  }), [])
  const yAxis = React.useMemo(() => ({
    tickSize: 0,
    isLeft: true,
    min: 0,
  }), [])
  const series = React.useMemo(() => [{
    name: 'Cumulative btc trend',
    type: 'Bar',
    histBase: 0,
    isShowValue: true,
    data: [10, 10, 10, 10, 10],
    barColor: (v) => v > 0 ? '#02C076' : '#F84960',
  }], [])
  const tooltip = React.useMemo(() => ({
    type: 'shadow',
  }), [])
  return(
    <Chart
      height={400}
      xAxis={xAxis}
      yAxis={yAxis}
      tooltip={tooltip}
      series={series}
    />
  )
}
```
