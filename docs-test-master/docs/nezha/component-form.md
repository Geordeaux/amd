---
id: component-form
title: Form
sidebar_label: Form
---

## mp-input

| Property    | Type           | Default Value | Required | Description                                                                                                         | Minimum Version |
| ----------- | -------------- | ------------- | -------- | ------------------------------------------------------------------------------------------------------------------- | --------------- |
| value       | String, Number |               |          | The initial content in the input box.                                                                               |                 |
| type        | String         | `text`        |          | The type of input.                                                                                                  |                 |
| password    | Boolean        | `false`       |          | Specifies whether the input is password-type content.                                                               |                 |
| placeholder | String         |               |          | The placeholder used when the input box is empty.                                                                   |                 |
| disabled    | Boolean        | `false`       |          | Specifies whether to disable the component.                                                                         |                 |
| maxlength   | Number         | `140`         |          | The maximum length. When it is set to `-1`, the maximum length is not limited.                                      |                 |
| autofocus   | Boolean        | `false`       |          | (Will be discarded soon. Use the focus instead.) Auto focus. The keyboard is automatically displayed.               |                 |
| focus       | Boolean        | `false`       |          | Gets focus.                                                                                                         |                 |
| bindinput   | Event handler  |               |          | Triggered when the user taps the keyboard. `event.detail = { value, keyCode }`, where keyCode is the key value.box. |                 |
| bindfocus   | Event handler  |               |          | Triggered when the input box is focused. `event.detail = { value }`                                                 |                 |
| bindblur    | Event handler  |               |          | Triggered when the input box is unfocused. `event.detail = { value }`.                                              |                 |

### Valid values of `type`

| Value    | Description                | Minimum Version |
| -------- | -------------------------- | --------------- |
| `text`   | Keyboard for text input.   |                 |
| `number` | Keyboard for number input. |                 |

## mp-label

| Property | Type   | Default Value | Required | Description                  | Minimum Version |
| -------- | ------ | ------------- | -------- | ---------------------------- | --------------- |
| for      | String |               |          | The ID of the bound control. |                 |

## mp-switch

| Property   | Type          | Default Value | Required | Description                                                              | Minimum Version |
| ---------- | ------------- | ------------- | -------- | ------------------------------------------------------------------------ | --------------- |
| checked    | Boolean       | `false`       |          | Specifies whether the item is selected.                                  |                 |
| disabled   | Boolean       | `false`       |          | Specifies whether to disable the component.                              |                 |
| type       | String        | `checkbox`    |          | The style. Valid values include `checkbox`.                              |                 |
| bindchange | Event handler |               |          | A change event triggered when checked changes. `event.detail = {value}`. |                 |

## mp-textarea

| Property       | Type          | Default Value | Required | Description                                                                                                         | Minimum Version |
| -------------- | ------------- | ------------- | -------- | ------------------------------------------------------------------------------------------------------------------- | --------------- |
| value          | String        |               |          | The initial content in the input box.                                                                               |                 |
| placeholder    | String        |               |          | The placeholder used when the input box is empty.                                                                   |                 |
| disabled       | Boolean       | `false`       |          | Specifies whether to disable the component.                                                                         |                 |
| maxlength      | Number        | `140`         |          | The maximum length. When it is set to `-1`, the maximum length is not limited.                                      |                 |
| autofocus      | Boolean       | `false`       |          | (Will be discarded soon. Use the focus instead.) Auto focus. The keyboard is automatically displayed.               |                 |
| focus          | Boolean       | `false`       |          | Gets focus.                                                                                                         |                 |
| bindinput      | Event handler |               |          | Triggered when the user taps the keyboard. `event.detail = { value, keyCode }`, where keyCode is the key value.box. |                 |
| bindfocus      | Event handler |               |          | Triggered when the input box is focused. `event.detail = { value }`                                                 |                 |
| bindblur       | Event handler |               |          | Triggered when the input box is unfocused. `event.detail = { value }`.                                              |                 |
| bindlinechange | Event handler |               |          | Called when the number of lines in the input box changes. `event.detail = {lineCount: 0}`                           |                 |
