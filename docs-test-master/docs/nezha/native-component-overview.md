---
id: native-component-overview
title: Overview
sidebar_label: Overview
---

## Overview

### Valid types of the component `type`

| Value         | Description           |
| ------------- | --------------------- |
| `input`       | Input component.      |
| `textarea`    | TextArea component.   |
| `canvas`      | Canvas component.     |
| `cover-view`  | CoverView component.  |
| `cover-image` | CoverImage component. |

### Action: `native-component-create`

`payload`

| Property   | Type       | Default Value | Required | Description                                                         |
| ---------- | ---------- | ------------- | -------- | ------------------------------------------------------------------- |
| rendererId | `number`   |               | ✓        |                                                                     |
| instanceId | `string`   |               | ✓        |                                                                     |
| type       | `string`   |               | ✓        | The type of the component                                           |
| props      | `object`   |               | ✓        | Component properties, varies according to the type.                 |
| zIndexDiff | `object`   |               |          | Changed zIndex of other native-components compared to the previous. |
| parent     | `string`   |               |          | parent instanceId for creating cover-view                           |
| children   | `string[]` |               |          | children instanceId for creating cover-view                         |

**Common props:**

| Property | Type      | Default Value | Required | Description                                                 |
| -------- | --------- | ------------- | -------- | ----------------------------------------------------------- |
| x        | `number`  |               | ✓        |                                                             |
| y        | `number`  |               | ✓        |                                                             |
| width    | `number`  |               | ✓        |                                                             |
| height   | `number`  |               | ✓        |                                                             |
| children | `string`  |               |          | Text content for cover-view                                 |
| src      | `string`  |               |          | Path of image for cover-image                               |
| hide     | `boolean` | false         |          | Whether to hide native component.                           |
| zIndex   | `number`  |               |          | converted absolute zIndex compared to all native components |

### Action: `native-component-destroy`

| Property   | Type     | Default Value | Required | Description               |
| ---------- | -------- | ------------- | -------- | ------------------------- |
| rendererId | `number` |               | ✓        |                           |
| instanceId | `string` |               | ✓        |                           |
| type       | `string` |               | ✓        | The type of the component |

### Action: `native-component-update`

| Property   | Type     | Default Value | Required | Description                                   |
| ---------- | -------- | ------------- | -------- | --------------------------------------------- |
| rendererId | `number` |               | ✓        |                                               |
| instanceId | `string` |               | ✓        |                                               |
| type       | `string` |               | ✓        | The type of the component                     |
| props      | `object` |               | ✓        | Changed props compared to the previous props. |

### Action: `native-component-invoke`

| Property   | Type     | Default Value | Required | Description                         |
| ---------- | -------- | ------------- | -------- | ----------------------------------- |
| rendererId | `number` |               | ✓        |                                     |
| instanceId | `string` |               | ✓        |                                     |
| type       | `string` |               | ✓        | The type of the component           |
| method     | `string` |               | ✓        | 'focus' method to invoke on native. |
| args       | `[]`     |               | ✓        | method arguments                    |

type = `input` | `textarea`, method = `focus`

```js
{
  rendererId: number
  instanceId: string
  type: 'input' // 'input', 'textarea'
  method: 'focus'
  args: []
}
```

### Event: `native-component-event`

| Property   | Type     | Default Value | Required | Description               |
| ---------- | -------- | ------------- | -------- | ------------------------- |
| rendererId | `number` |               | ✓        |                           |
| instanceId | `string` |               | ✓        |                           |
| type       | `string` |               | ✓        | The type of the component |
| eventName  | `string` |               | ✓        | Name of the event.        |
| eventData  | `object` |               |          | Data of the event.        |

#### type = `input`

```js
{
  rendererId: number
  instanceId: string
  type: 'input'
  eventName: 'input'
  eventData: {
    value: string
    cursor: number
    keyCode: number
  }
}
{
  rendererId: number
  instanceId: string
  type: 'input'
  eventName: 'focus'
  eventData: {
    value: string
    height: number
  }
}
{
  rendererId: number
  instanceId: string
  type: 'input'
  eventName: 'blur'
  eventData: {
    value: string
  }
}
{
  rendererId: number
  instanceId: string
  type: 'input'
  eventName: 'confirm'
  eventData: {
    value: string
  }
}
{
  rendererId: number
  instanceId: string
  type: 'input'
  eventName: 'keyboardHeightChange'
  eventData: {
    height: number
    duration: number
  }
}
```

#### type = `textarea`

```js
{
  rendererId: number
  instanceId: string
  type: 'textarea'
  eventName: 'input'
  eventData: {
    value: string
    cursor: number
    keyCode: number
  }
}
{
  rendererId: number
  instanceId: string
  type: 'textarea'
  eventName: 'focus'
  eventData: {
    value: string
    height: number
  }
}
{
  rendererId: number
  instanceId: string
  type: 'textarea'
  eventName: 'blur'
  eventData: {
    value: string
  }
}
{
  rendererId: number
  instanceId: string
  type: 'textarea'
  eventName: 'confirm'
  eventData: {
    value: string
  }
}
{
  rendererId: number
  instanceId: string
  type: 'textarea'
  eventName: 'keyboardHeightChange'
  eventData: {
    height: number
    duration: number
  }
}
{
  rendererId: number
  instanceId: string
  type: 'textarea'
  eventName: 'lineChange'
  eventData: {
    height: number
    heightRpx: number
    lineCount: number
  }
}
```

#### type = `cover-image`

```js
{
  rendererId: number
  instanceId: string
  type: 'cover-image'
  eventName: 'load'
  eventData: {
    src: string
  }
}
{
  rendererId: number
  instanceId: string
  type: 'cover-image'
  eventName: 'error'
  eventData: {
    src: string
    errMsg: string
  }
}
```

#### type = `cover-view`

```js
{
  rendererId: number
  instanceId: string
  type: 'cover-view'
  eventName: 'touchstart' | 'touchmove' | 'touchend'
  eventData: {
    touches: Touch[]
    changedTouches: Touch[]
  }
}
{
  rendererId: number
  instanceId: string
  type: 'cover-view'
  eventName: 'touchcancel'
  eventData: {}
}
```

Touch interface

```js
interface Touch {
  identifier: number
  pageX: number
  pageY: number
  clientX: number
  clientY: number
}
```

### Styling

Now we only support:

- color?: hex string
- backgroundColor?: hex string
- fontSize?: number px
- fontWeight?: number 100~900
- maxHeight?: number px
- minHeight?: number px
- lineHeight?: number px
- zIndex?: number // converted absolute value start from 1
- whiteSpace?: string // 'normal', 'nowrap'
- paddingTop?: number px
- paddingRight?: number px
- paddingBottom?: number px
- paddingLeft?: number px
- textAlign?: string // 'left', 'center', 'right'

#### Example

```json
{
  "payload": {
    "props": {
      "style": {
        "color": "#00000080",
        "fontSize": 16
      },
      "placeholderStyle": {
        "fontWeight": 400
      }
    }
  }
}
```
