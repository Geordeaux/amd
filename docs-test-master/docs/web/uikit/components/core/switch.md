---
id: switch
title: Switch
---

## Overview
A Switch represents a physical switch that allows someone to choose between two mutually exclusive options.  For example, “On/Off”, “Show/Hide”. Choosing an option should produce an immediate result.


## Usage

```tsx live enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
  <Label mb="xs">
    <Switch mr="xs" />
    <Text variant="formLabel">Label</Text>
  </Label>
  <Switch />
</Box>
```

```tsx live enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
  <Label mb="xs">
    <Switch reverseDirection mr="xs" />
    <Text variant="formLabel">Label</Text>
  </Label>
  <Switch reverseDirection defaultChecked />
</Box>
```

```tsx live enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
  <Label mb="xs">
    <Switch size={16} mr="xs" />
    <Text variant="formLabelSmall">Label</Text>
  </Label>
  <Switch size={16} />
</Box>
```

```tsx live enableExportToCodePen enableSwitchTheme enableHideEditor
() => {
  const [checked, setChecked] = React.useState(true)

  const onChange = (e) => {
    console.log(`Switch change: ${e.target.checked}`)
    setChecked(e.target.checked)
  }

  return <Switch checked={checked} onChange={onChange} name="switch1" />
}
```



```tsx live enableExportToCodePen enableSwitchTheme enableHideEditor
() => {
  const onChange = () => {
    console.log('Should Not Change!')
  }

  return (
    <>
      <Switch mr="sm" defaultChecked={true} disabled onChange={onChange} />
      <Switch mr="sm" disabled onChange={onChange} />
    </>
  )
}
```


```tsx live enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
  <Switch mr="sm" defaultChecked={true}  color="primary" />
  <Switch mr="sm" defaultChecked={true} color="sell" />
</Box>
```

```tsx live enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
  <Switch mr="sm" size={16} />
  <Switch mr="sm" size={24}  />
  <Switch mr="sm" size={32}  />
  <Switch mr="sm" size={40}  />
</Box>
```



```tsx live enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
  <Switch mr="sm" size={40} icon={<ModeLightV2S24 color="primary" />} />
</Box>
```
## Implementation

|  name   | type  | isRequired | default |
|  :----:  | :----:  | :----:  | :----:  |
| checked | boolean  | false | false |
| onClick |  (event: React.MouseEvent&lt;HTMLButtonElement, MouseEvent>) => void (with event.target.checked)  | false |  |
| color | string | false |  |
| size | number | false | 24 |
| disabled |  boolean | false |  |
| icon |  ReactNode | false | null |
| reverseDirection |  boolean | false | false |

