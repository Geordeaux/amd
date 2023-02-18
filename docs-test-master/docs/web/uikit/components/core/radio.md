---
id: radio
title: Radio
---

## Overview
Accessible and themeable form radio input component.


## Usage

```tsx live title=default enableExportToCodePen enableSwitchTheme enableHideEditor
() => {
  let [value, setValue] = React.useState(1)
  return (
    <>
      <Radio value={1} name='radio' checked={value === 1} onChange={setValue} label="备选项1"/>
      <Radio value={2} name='radio' checked={value === 2} onChange={setValue} label="备选项2"/>
    </>
  )
}
```


```tsx live title=direction enableExportToCodePen enableSwitchTheme enableHideEditor
() => {
  let [value, setValue] = React.useState('1')
  return (
    <>
      <Radio value="1" name='radio' checked={value==='1'}  onChange={setValue} label="备选项1"/>
      <Radio value="2" name='radio' checked={value==='2'}  onChange={setValue} direction='left' value="2" label="备选项2"/>
      <Radio value="3" name='radio' checked={value==='3'}  onChange={setValue} direction='top' value="3" label="备选项3" />
      <Radio value="4" name='radio' checked={value==='4'}  onChange={setValue} direction='bottom' value="4" label="备选项4"/>
    </>
  )
}
```


```tsx live title=disabled enableExportToCodePen enableSwitchTheme enableHideEditor
() => {
  let [value, setValue] = React.useState('1')
  return (
    <>
      <Radio value="1" checked={value==='1'} onChange={setValue} disabled={true} label="备选项1"/>
      <Radio value="2" checked={value==='2'} onChange={setValue} disabled={true} label="备选项2"/>
    </>
  )
}
```

```tsx live title=Uncontrol enableExportToCodePen enableSwitchTheme enableHideEditor
<Radio value="1"  onChange={console.log} label="备选项"/>
```


```tsx live title=RadioGroup enableExportToCodePen enableSwitchTheme enableHideEditor
() => {
  let [value, setValue] = React.useState('1')
  return (
    <RadiosGroup value={value} onChange={setValue}>
      <Radio value="1" label="备选项"/>
      <Radio value="2" label="备选项"/>
      <Radio value="3" label="备选项"/>
    </RadiosGroup>
  )
}
```

```tsx live title=RadioGroupWithLayout enableExportToCodePen enableSwitchTheme enableHideEditor
() => {
  let [value, setValue] = React.useState('1')
  return (
    <RadiosGroup layout="Vertical" value={value} onChange={setValue}>
      <Radio value="1" label="备选项"/>
      <Radio value="2" label="备选项"/>
      <Radio value="3" label="备选项"/>
    </RadiosGroup>
  )
}
```

## Implementation

### Radio Props

| name        | type    |  default  |  isRequired  |
| --------    | ----- | ---- |---- |
| value       | string / number      |       |   true  |
| checked     | boolean     |   false    |   true  |
| disabled    | boolean      |   false    |  false   |
| direction   | 'top' \| 'bottom' \| 'left' \| 'right'      |   'right'    |  false   |
| onChange    | Function    | () => {} | false |
| name        | string     |      | false |
| label        | string \| ReactNode    |      | false |

### RadioGroup Props

| name        | type    |  default   |  isRequired  |
| --------    | -----   | ----       |----          |
| value       | string / number      |              |   false  |
| name        | string               |              |   true  |
| onChange    | Function             | () => {}     | false |
| children    | Radio[]              |              | true
| layout      | 'Vertical' \| 'Horizontal' | undefined       | false


