---
id: native-ui
title: NativeUI
sidebar_label: NativeUI
---

## Overview

Native UI are a set of UI that are rendered by native side. The different between Same-layer Component is that Native UIs are not associated with any dom elements.

### Protocols

RPC actions and events that are related to native UI.

### Valid types of the component `type`

| Value           | Description          |
| --------------- | -------------------- |
| `selector`      | Common picker.       |
| `multiSelector` | Multi-column picker. |
| `timePicker`    | Time picker.         |
| `datePicker`    | Date picker.         |

#### Action: `native-ui-show`

`payload`

| Property | Type     | Default Value | Required | Description                                         |
| -------- | -------- | ------------- | -------- | --------------------------------------------------- |
| type     | `string` |               | ✓        | The type of the UI component                        |
| props    | `object` |               | ✓        | Component properties, varies according to the type. |

`response`

| Property | Type     | Example                      | Description |
| -------- | -------- | ---------------------------- | ----------- |
| id       | `string` | Instance id of the component |

#### Action: `native-ui-hide`

| Property | Type     | Default Value | Required | Description |
| -------- | -------- | ------------- | -------- | ----------- |
| id       | `string` |               | ✓        | Instance id |

#### Action: `native-ui-update`

| Property | Type     | Default Value | Required | Description                                   |
| -------- | -------- | ------------- | -------- | --------------------------------------------- |
| id       | `string` |               | ✓        | Instance id                                   |
| props    | `object` |               | ✓        | Changed props compared to the previous props. |

#### Event: `native-ui-event`

| Property | Type     | Default Value | Required | Description        |
| -------- | -------- | ------------- | -------- | ------------------ |
| id       | `string` |               | ✓        | Instance id        |
| name     | `string` |               | ✓        | Name of the event. |
| data     | `object` |               |          | Data of the event. |

### Types

#### selector

Props:

| Property | Type       | Default Value | Required | Description                                                                                     |
| -------- | ---------- | ------------- | -------- | ----------------------------------------------------------------------------------------------- |
| range    | `string[]` | []            |          | Items to be selected                                                                            |
| value    | `number`   | 0             |          | Indicates the sequence number (starting from 0 in the subscript) of the item selected in range. |

Event: `change`

Fired when value changes.

```ts
{
  // the index of the selected value, starts from 0
  value: number
}
```

Event: `cancel`

Fired when user cancel the operation.

#### multiSelector

Props:

| Property | Type         | Default Value | Required | Description                                                                                     |
| -------- | ------------ | ------------- | -------- | ----------------------------------------------------------------------------------------------- |
| range    | `string[][]` | []            |          | Items to be selected                                                                            |
| value    | `number[]`   | []            |          | Indicates the sequence number (starting from 0 in the subscript) of the item selected in range. |

Event: `change`

Fired when value changes.

```ts
{
  // the index of the selected value, starts from 0
  value: number[]
}
```

Event: `columnChange`

Fired when value changes.

```ts
{
  // the index of the selected value, starts from 0
  value: number
  // the index of the selected column
  column: number
}
```

Event: `cancel`

Fired when user cancel the operation.

#### timePicker

Props:

| Property | Type     | Default Value | Required | Description                                                |
| -------- | -------- | ------------- | -------- | ---------------------------------------------------------- |
| value    | `string` | Today         |          | The selected time, in the form of "hh:mm".                 |
| start    | `string` |               |          | The start of the valid time range, in the form of "hh:mm". |
| end      | `string` |               |          | The end of the valid time range, in the form of "hh:mm".   |

Event: `change`

Fired when value changes.

```ts
{
  value: string
}
```

Event: `cancel`

Fired when user cancel the operation.

#### datePicker

Props:

| Property | Type     | Default Value | Required | Description                                                                     |
| -------- | -------- | ------------- | -------- | ------------------------------------------------------------------------------- |
| value    | `string` | Today         |          | The selected date, in the form of "YYYY-MM-DD".                                 |
| start    | `string` |               |          | The start of the valid date range, in the form of "YYYY-MM-DD".                 |
| end      | `string` |               |          | The end of the valid date range, in the form of "YYYY-MM-DD".                   |
| fields   | `string` | `"day"`       | ✓        | The granularity of the picker. Valid values include "year", "month", and "day". |

Event: `change`

Fired when value changes.

```ts
{
  value: string
}
```

Event: `cancel`

Fired when user cancel the operation.
