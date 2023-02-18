---
id: mask
title: Mask
---

## Overview

Pop-up mask

## Best practices

Used to control the visible area of the pop-up layer, the pop-up layer can be closed。

## Usage

```tsx live title=Basic enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const [visible, setVisible] = React.useState(false)
  return (
    <>
      <Button onClick={()=> {
        setVisible(true)
      }}>Open mask</Button>
      <Mask visible={visible} onClick={()=> {
        setVisible(false)
      }} />
    </>
  ) 
}
```

```tsx live title=Custom enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const [visible, setVisible] = React.useState(false);

  return (
    <>
      <Button onClick={()=> {
        setVisible(true)
      }}>Open mask</Button>
      <Mask 
        visible={visible}
        sx={{
          position: 'absolute',
          width: '500px',
          left: '50%'
        }}
        onClick={()=> {
          setVisible(false)
        }}/>
    </>
  ) 
}
```

## Implementation

|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  | :----:  |
| visible | boolean | true | false | Is Mask visible |
