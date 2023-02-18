---
id: lineChart
title: LineChart
---

## props

|  name   | type  | isRequired | default |
|  :----:  | :----:  | :----:  | :----:  |
| width | number | false | - |
| height | number |  | - |
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
| format | Function | false | `v => ${v}` | yAxis tick will be fixed by tickSize when not provide format function
| min | number | false | undefined | yAxis extent min value |
| max | number | false | undefined | yAxis extent max value |

## tooltipProps

|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  |:---: |
| isTrigger | boolean | false | true | is trigger tooltip |
| type | line \| shadow \| cross | false | cross | tooltip type |
| arrow | boolean | false | true | is show tooltip arrow |
| crossLineColor | string | false | #F0B90B | cross line color |
| crossIcon | string | false | - | hover icon base64 string |
| crossIconSize | { width: number, height: number } | false | - | hover icon size |
| render | ({left, top, xValue, string \| number, labels: Array<{ color: string, label: string, value: string}>}) | false | - | custom tooltip labels |
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

## lineProps

|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  | :----: |
| type | string | true | 'LINE' | - |
| data | number[] | true | [] | - |
| color | string | true | #F0B90B | - |
| lineWidth | number | false | 2 | - |
| isCurve | boolean | false | false | - |
| isShowMarker | boolean | false | true | - |
| formmatTooltip | Function | false | - | - |
| icon | string \| (v, index, arr) => string | false | - | custom icon |
| iconSize | { width: number, height: number } | false | - | custom icon size |


```ts
() => {
  const [limit, setLimit] = React.useState(10)
  const [legendBox, setLegendBox] = React.useState()
  const [icon, setIcon] = React.useState()
  const [[icon1, icon2]] = React.useState([`data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjkiIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyOSAyNCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTkuNDYzMDMgMjMuNDk2N0MxOS43ODg4IDIzLjQ5NjcgMjUuNDQwOSAxNC43NTA3IDI1LjQ0MDkgNy4xMzA2OEMyNS40NDA5IDYuODkwMTYgMjUuNDQwOSA2LjY0NDE4IDI1LjQ0MDkgNi4zODcyN0MyNi41MzQ4IDUuNTY3MTggMjcuNDczMyA0LjU1ODI5IDI4LjIxMjMgMy40MDgxNUMyNy4xODQzIDMuODY5NjIgMjYuMDkzOSA0LjE3NzIxIDI0Ljk3NjMgNC4zMjEwMUMyNi4xNTgzIDMuNTg4ODMgMjcuMDM3MyAyLjQ1NTY2IDI3LjQ1MjUgMS4xMjg3MUMyNi4zNTQ5IDEuODA2NzEgMjUuMTQzOCAyLjI4MDc4IDIzLjg3NzYgMi41MjgwOEMyMy4zNjM5IDEuOTQ5NjcgMjIuNzMzNCAxLjQ4Njg3IDIyLjAyNzYgMS4xNzAyNUMyMS4zMjE4IDAuODUzNjQyIDIwLjU1NjkgMC42OTA0MzEgMTkuNzgzMyAwLjY5MTQxMUMxOC4yNzk0IDAuNzE3MzA5IDE2Ljg0NjcgMS4zMzcwOCAxNS43OTggMi40MTU0N0MxNC43NDkzIDMuNDkzODUgMTQuMTY5OCA0Ljk0MzI3IDE0LjE4NTkgNi40NDczOUMxNC4xODcyIDYuODg2OTYgMTQuMjM2NyA3LjMyNTA2IDE0LjMzMzUgNy43NTM4M0MxMi4wODc0IDcuNjM0NDUgOS44OTI4OSA3LjAzNjE3IDcuODk3MDIgNS45OTkxMUM1LjkwMTE2IDQuOTYyMDYgNC4xNTAxMyAzLjUxMDIgMi43NjEzOCAxLjc0MDkzQzIuMDQ0MjcgMy4wMDkzNSAxLjgyNDQ4IDQuNDk5MTIgMi4xNDQ3MyA1LjkyMDU5QzIuNDY0OTkgNy4zNDIwNiAzLjMwMjQyIDguNTkzNjMgNC40OTQxOSA5LjQzMTk4QzMuNjA4MzIgOS4zOTgxIDIuNzQzNzggOS4xNTA1NSAxLjk3NDI0IDguNzEwNDNWOC44MTQyOUMxLjk2OTEyIDEwLjEyMDkgMi40MTA4NCAxMS4zOSAzLjIyNjEyIDEyLjQxMTFDNC4wNDE0IDEzLjQzMjIgNS4xODEyNCAxNC4xNDM5IDYuNDU2NTggMTQuNDI4MUM1Ljk3MjcxIDE0LjU2MzQgNS40NzIxNSAxNC42Mjk3IDQuOTY5NzUgMTQuNjI0OUM0LjYxNTE2IDE0LjYyNyA0LjI2MTQgMTQuNTkwMyAzLjkxNDc2IDE0LjUxNTZDNC4yNTk2OSAxNS42NDk1IDQuOTUyNjMgMTYuNjQ2MSA1Ljg5NTQzIDE3LjM2NDJDNi44MzgyNCAxOC4wODIzIDcuOTgzMSAxOC40ODU2IDkuMTY3ODUgMTguNTE2OUM3LjE5NDI0IDIwLjEwOTYgNC43MzQ0MiAyMC45Nzc3IDIuMTk4MzUgMjAuOTc2N0MxLjc1ODAxIDIwLjk4MDcgMS4zMTc5IDIwLjk1NTIgMC44ODA5ODEgMjAuOTAwMkMzLjQyNDU4IDIyLjU4OTggNi40MDk0MyAyMy40OTI4IDkuNDYzMDMgMjMuNDk2N1oiIGZpbGw9InVybCgjcGFpbnQwX2xpbmVhcikiLz4KPGRlZnM+CjxsaW5lYXJHcmFkaWVudCBpZD0icGFpbnQwX2xpbmVhciIgeDE9IjAuODU5MTE2IiB5MT0iMTIuMDk0MSIgeDI9IjI4LjIzNDIiIHkyPSIxMi4wOTQxIiBncmFkaWVudFVuaXRzPSJ1c2VyU3BhY2VPblVzZSI+CjxzdG9wIHN0b3AtY29sb3I9IiNGMEI5MEIiLz4KPHN0b3Agb2Zmc2V0PSIwLjI4IiBzdG9wLWNvbG9yPSIjRjFCQzBGIi8+CjxzdG9wIG9mZnNldD0iMC41NyIgc3RvcC1jb2xvcj0iI0Y0QzQxQyIvPgo8c3RvcCBvZmZzZXQ9IjAuODYiIHN0b3AtY29sb3I9IiNGOEQyMzAiLz4KPHN0b3Agb2Zmc2V0PSIwLjk5IiBzdG9wLWNvbG9yPSIjRkJEQTNDIi8+CjwvbGluZWFyR3JhZGllbnQ+CjwvZGVmcz4KPC9zdmc+Cg==`, 'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjgiIGhlaWdodD0iMjgiIHZpZXdCb3g9IjAgMCAyOCAyOCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTE0LjIyNSAyLjg5Mjg4QzE0LjIyNSAyLjg5Mjg4IDE0LjQ1NjQgMy4xMjkxNiAxNC40NTY0IDMuMjMwNDJDMTQuNDU2NCAzLjIzMDQyIDE0LjYzIDMuODA0MjQgMTQuOTE0NSAzLjg1NzI5QzE0LjkxNDUgMy44NTcyOSAxNS45MjIzIDQuMDU5ODEgMTYuMzYxMSAzLjU3Mjc5QzE2LjUxNyAzLjM4MjE3IDE2LjcxNTggMy4yMzEyMSAxNi45NDEzIDMuMTMyM0MxNy4xNjY3IDMuMDMzMzkgMTcuNDEyNSAyLjk4OTM1IDE3LjY1ODMgMy4wMDM3OUMxNy42NTgzIDMuMDAzNzkgMTkuMzI2NyAzLjA5MDU4IDIwLjEwMyAzLjg1NzI5QzIwLjE2MjMgMy45MjM1OSAyMC4xOTUxIDQuMDA5NDMgMjAuMTk1MSA0LjA5ODM5QzIwLjE5NTEgNC4xODczNSAyMC4xNjIzIDQuMjczMTggMjAuMTAzIDQuMzM5NDlMMTcuNjU4MyA2Ljc4OTA4QzE3LjY1ODMgNi43ODkwOCAxNS45NTYxIDguODQzMjcgMTcuNDI2OCA5LjQ3MDEzQzE4LjA4OTkgOS41ODUzMSAxOC42OTY2IDkuOTE1NzggMTkuMTUzMSAxMC40MTA0QzE5LjUzOTMgMTEuMDM3NSAyMC4wNDk3IDExLjU3OSAyMC42NTI3IDEyLjAwMTdDMjEuMDAzMSAxMi4xNTk3IDIxLjMxMjQgMTIuMzk2NCAyMS41NTY2IDEyLjY5MzJDMjEuODAwOCAxMi45OTAxIDIxLjk3MzMgMTMuMzM5MiAyMi4wNjA4IDEzLjcxMzVDMjIuMDYwOCAxMy43MTM1IDIyLjczNTkgMTQuOTU3NiAyMi45NDggMTUuMzI4OUMyMy4yODkxIDE2LjEzMTEgMjMuNTE3OSAxNi45NzY1IDIzLjYyNzkgMTcuODQxMkMyMy41MTk2IDE5LjA0NjcgMjMuMjI4NSAyMC4yMjg4IDIyLjc2NDggMjEuMzQ2OEMyMi43NjQ4IDIxLjM0NjggMjEuNjQxMyAyNC4xNjc3IDE5LjA3NTkgMjQuMTY3N0MxNy44Njc0IDI0LjE4MzEgMTYuNjcwNCAyNC40MDQ5IDE1LjUzNjYgMjQuODIzNUMxNS41MzY2IDI0LjgyMzUgMTMuNTQ5OSAyNS41MzIzIDExLjc4OTggMjQuOTA1NUMxMC45NzAzIDI0LjUzNzkgMTAuMTI0IDI0LjIzMzIgOS4yNTgyOCAyMy45OTQxQzkuMjU4MjggMjMuOTk0MSA3LjUwMzA2IDIzLjc5NjQgNi44MDg2OSAyMy4yMjc0QzYuODA4NjkgMjMuMjI3NCA1LjA5Njg2IDIyLjgwMzEgNS4yMDc3NyAyMC42NjY5QzUuMjA3NzcgMjAuNjY2OSA1LjI2MDgxIDIwLjEyNjggNC41MzI2OSAxOC42NjU3QzQuNTMyNjkgMTguNjY1NyAzLjkwMSAxNy40Njk5IDUuMTExMzMgMTUuOTYwNkM1LjU2NDY0IDE1LjI5MjEgNS44Njg4OCAxNC41MzQgNi4wMDM0MSAxMy43Mzc2QzYuNDgzNzMgMTIuODQ4MSA3LjEzOTk3IDEyLjA2NTUgNy45MzIyMiAxMS40Mzc1QzguMzEwNzYgMTAuOTM1NiA4Ljc2MjY2IDEwLjQ5MzQgOS4yNzI3NCAxMC4xMjU5QzkuODA3NzUgOS44NzkxOSAxMC4yOTY5IDkuNTQzMjkgMTAuNzE5NCA5LjEzMjU5QzEwLjcxOTQgOS4xMzI1OSAxMS45MDA3IDcuNDc4NjMgOS4wNzk4NiA2LjM0MDYzQzkuMDc5ODYgNi4zNDA2MyA3LjY5NTk0IDYuMTM4MTEgNy4yOTU3MSA1LjAwMDExQzcuMjk1NzEgNS4wMDAxMSA3LjEyMjEyIDQuMDg4NzQgOS4wNzk4NiA0LjE5OTY1QzkuMDc5ODYgNC4xOTk2NSAxMC40MzQ5IDQuNTEzMDggMTAuMTc0NSAzLjU3Mjc5QzEwLjE3NDUgMy41NzI3OSA5Ljg1NjIxIDIuNDM0NzkgMTEuNjc0MSAyLjg5Mjg4QzExLjY3NDEgMi44OTI4OCAxMy4zNDI1IDMuMDYxNjUgMTMuNTQ1MSAyLjgzNTAxTDEzLjg4NzQgMi44NjM5NUwxNC4yMjUgMi44OTI4OFoiIGZpbGw9InVybCgjcGFpbnQwX2xpbmVhcikiLz4KPHBhdGggZD0iTTEzLjc5MDkgMjIuMDE3MVYyMC45NDE4QzEzLjIyMTkgMjAuOTMwMSAxMi42NjM1IDIwLjc4NDggMTIuMTYwOSAyMC41MTc2QzExLjY1ODQgMjAuMjUwNCAxMS4yMjU2IDE5Ljg2ODggMTAuODk3NyAxOS40MDM1TDExLjQ2MTkgMTguOTIxM0MxMS43MjE1IDE5LjMwNzQgMTIuMDY2OCAxOS42Mjg0IDEyLjQ3MDcgMTkuODU5MkMxMi44NzQ3IDIwLjA5IDEzLjMyNjUgMjAuMjI0NSAxMy43OTA5IDIwLjI1MjJWMTYuNTM5MkgxMy42MzY2QzExLjgxODcgMTYuMjMwNiAxMS4xNTMzIDE1LjM1NzggMTEuMTUzMyAxNC4xMjgyQzExLjE1MzMgMTIuNjgxNiAxMi4yMjg2IDExLjkyNDYgMTMuNzkwOSAxMS44NDc0VjEwLjc0MzJIMTQuMzU1MVYxMS43OTQ0QzE0Ljg2NDIgMTEuODA5OCAxNS4zNjI4IDExLjk0MjcgMTUuODEyIDEyLjE4MjhDMTYuMjYxMiAxMi40MjI5IDE2LjY0ODggMTIuNzYzNiAxNi45NDQ1IDEzLjE3ODNMMTYuMzgwNCAxMy42NjA1QzE2LjE1NTggMTMuMzMxOCAxNS44NTg0IDEzLjA1OTMgMTUuNTExMyAxMi44NjQ0QzE1LjE2NDIgMTIuNjY5NiAxNC43NzY4IDEyLjU1NzUgMTQuMzc5MiAxMi41MzdWMTUuOTcwMkwxNC42MTA3IDE2LjAxODVDMTYuNDU3NSAxNi4zNTEyIDE3LjA2OTkgMTcuMjI0IDE3LjA2OTkgMTguNDI5NUMxNy4wNjk5IDE5Ljk0MzYgMTYuMDE4NyAyMC44NDA1IDE0LjM3OTIgMjAuOTY1OVYyMi4wNDEySDEzLjc5MDlWMjIuMDE3MVpNMTMuNjY1NiAxNS44MTU5TDEzLjc5MDkgMTUuODQ0OVYxMi41MzdDMTIuNjE0MyAxMi42MTQxIDExLjg5NTkgMTMuMTQ5NCAxMS44OTU5IDE0LjEyMzRDMTEuODcxOCAxNS4xNTA1IDEyLjU2MTMgMTUuNTg0NSAxMy42NjU2IDE1LjgxNTlaTTE0LjQ2NiAxNi42MzU3SDE0LjM2NDdWMjAuMjUyMkMxNS41NzAzIDIwLjE1MDkgMTYuMjkzNiAxOS40ODU1IDE2LjI5MzYgMTguNDM0M0MxNi4yOTM2IDE3LjM4MzEgMTUuNjM3OCAxNi44MTQxIDE0LjQ2NiAxNi42MzU3WiIgZmlsbD0iYmxhY2siIHN0cm9rZT0iYmxhY2siIHN0cm9rZS13aWR0aD0iMC4yIi8+CjxkZWZzPgo8bGluZWFyR3JhZGllbnQgaWQ9InBhaW50MF9saW5lYXIiIHgxPSI0LjM1NDI3IiB5MT0iMTMuOTgzNiIgeDI9IjIzLjYxMzUiIHkyPSIxMy45ODM2IiBncmFkaWVudFVuaXRzPSJ1c2VyU3BhY2VPblVzZSI+CjxzdG9wIHN0b3AtY29sb3I9IiNGMEI5MEIiLz4KPHN0b3Agb2Zmc2V0PSIwLjI4IiBzdG9wLWNvbG9yPSIjRjFCQzBGIi8+CjxzdG9wIG9mZnNldD0iMC41NyIgc3RvcC1jb2xvcj0iI0Y0QzQxQyIvPgo8c3RvcCBvZmZzZXQ9IjAuODYiIHN0b3AtY29sb3I9IiNGOEQyMzAiLz4KPHN0b3Agb2Zmc2V0PSIwLjk5IiBzdG9wLWNvbG9yPSIjRkJEQTNDIi8+CjwvbGluZWFyR3JhZGllbnQ+CjwvZGVmcz4KPC9zdmc+Cg=='])
  const xAxis = React.useMemo(() => ({
    data: CandlestickData.slice(0, limit).map((_, index) => 1569859200000 + index * 24 * 60 * 60 * 1000),
    splitInterval: 0,
    tickInterval: (_, i) => i % 2 === 0,
    formmatTooltip: () => '',
    formmat: 'YYYY-MM-DD',
  }), [limit])
  const series = React.useMemo(() => [{
    name: 'A',
    type: 'Line',
    color: '#F0B90B',
    lineWidth: 2,
    data: [1050, 990, 900, 950, 800, 700, 400, 200, 100, 50],
    iconSize: { width: 30, height: 30 },
    icon: (_, i) => i % 3 === 1 ? icon2 : '',
  },], [limit, icon1, icon2])
  const yAxis = React.useMemo(() => ({
    tickSize: 1,
    isLeft: true,
  }), [])
  const tooltip = React.useMemo(() => ({
    type: 'snap',
    isFloat: true,
    isFixTop: false,
    isShowHoverLine: true,
    crossIconSize: { width: 30, height: 30 },
    crossIcon: icon1,
    render: ({ left, top, xValue, labels }, i) => {
      return <Box
        bg="#12161C"
        color="#8D6207"
        sx={{
          position: 'absolute',
          left: `${left}px`,
          top: `${top}px`,
          padding: '12px',
          transform: 'translate(-50%, -150%)',
          borderRadius: '8px',
          border: '1px solid #AEB4BC',
        }}>{labels.map(({ color, label, value }, index) => (
          <Flex alignItems="center" key={index}>
            <Box sx={{ width: '8px', height: '8px', background: color, borderRadius: '100%', marginRight: '8px' }} />
            <Box>{label}:</Box>
            <Box sx={{ marginLeft: '12px'}}>{value}</Box>
          </Flex>
        ))}</Box>
    },
  }), [])
  const legend = React.useMemo(() => ({
    show: true,
    container: legendBox,
  }), [legendBox])
  return(
    <>
      <Button type="primary" onClick={() => setLimit(l => l === 10 ? 30 : 10)}>update data</Button>
      <Box ref={setLegendBox} />
      <Chart
        height={400}
        xAxis={xAxis}
        yAxis={yAxis}
        tooltip={tooltip}
        series={series}
        legend={legend}
      />
    </>
  )
}
```

## xAxis is time data

```ts
() => {
  const limit = 7
  const xAxis = React.useMemo(() => ({
    data: CandlestickData.slice(0, limit).map((_, index) => 1569859200000 + index * 24 * 60 * 60 * 1000),
    formmat: 'MM-DD',
    boundaryGap: 20
  }), [])
  const yAxis = React.useMemo(() => ({
    tickSize: 2,
    isLeft: true
  }), [])
  const series = React.useMemo(() => [{
    name: 'Cumulative btc trend',
    type: 'LINE',
    isCurve: true,
    data: [4, 4, 4, 4, 4, 2, 2],
  }], [])
  const tooltip = React.useMemo(() => ({
    type: 'line',
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

## curve line

```ts
() => {
  const xAxis = React.useMemo(() => ({
    data: ['>10%', '7-10%', '5-7%', '3-5%', '0-3%', '0%', '0-3%', '3-5%', '5-7%', '7-10%', '>10%'],
    tickInterval: 0,
    boundaryGap: true,
  }), [])
  const yAxis = React.useMemo(() => ({
    tickSize: 2,
    visible: true,
  }), [])
  const series = React.useMemo(() => [{
    name: 'Cumulative btc trend',
    type: 'Line',
    isCurve: true,
    data: [8.50, 12.11, 16, 8, 12, 3.34, 9.67, 15, 9, 5, 9],
  }], [])
  const tooltip = React.useMemo(() => ({
    type: 'line',
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
