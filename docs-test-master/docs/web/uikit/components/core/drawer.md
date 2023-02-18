---
id: drawer
title: Drawer
---

## Overview

Floating panel that slides out of the edge of the screen

## Best practices

The drawer slides in from the edge of the parent form to cover part of the content of the parent form. The user does not need to leave the current task when operating in the drawer, and can smoothly return to the original task after the operation is completed.

* When an additional panel is needed to control the content of the parent form, this panel is called out when needed. For example, control the interface display style and add content to the interface.
* When you need to insert a temporary task in the current task flow, create or preview additional content. For example, display the terms of the agreement, create sub-objects

## Usage

<!-- ```tsx live enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  let [direction, setDirection] = useState('top');
  let [visible, setVisible] = useState(false);

  function getStyle(direct) {
    if (['top', 'bottom'].indexOf(direct) !== -1) {
      return {
        width: '100%',
        height: '200px'
      }
    }

    return {
      width: '200px',
      height: '100%'
    }
  }

  return (
    <>
      <RadiosGroup value={direction} onChange={setDirection}>
        <Radio value='top' label='top'/>
        <Radio ml={2} value='right' label='right'/>
        <Radio ml={2} value='bottom' label="bottom"/>
        <Radio ml={2} value='left' label="left"/>
      </RadiosGroup>
      <br />
      <Button onClick={() => setVisible(!visible)}>Open</Button>
      <Drawer visible={visible} bg='#fff' maskBg='rgba(0,0,0,0.45)' direction={direction} outerClick={() => setVisible(!visible)}>
        <Box style={ getStyle(direction) }>{ direction.toUpperCase() }</Box>
      </Drawer>
    </>
  )
}
``` -->

```tsx live title=Top enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  let [visible, setVisible] = React.useState(false);

  const direction = 'top';

  return (
    <>
      <Button sz='l' onClick={() => setVisible(!visible)}>Open Drawer</Button>
      <Drawer visible={visible} bg='#fff' maskBg='rgba(0,0,0,0.45)' direction={direction} outerClick={() => setVisible(!visible)}>
        <Box height='200px'>{ direction.toUpperCase() }</Box>
      </Drawer>
    </>
  )
}
```

```tsx live title=Right enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  let [visible, setVisible] = React.useState(false);

  const direction = 'right';

  return (
    <>
      <Button sz='l' onClick={() => setVisible(!visible)}>Open Drawer</Button>
      <Drawer visible={visible} maskBg='rgba(0,0,0,0.45)' direction={direction} outerClick={() => setVisible(!visible)}>
        <Box width='200px'>{ direction.toUpperCase() }</Box>
      </Drawer>
    </>
  )
}
```

```tsx live title=Bottom enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  let [visible, setVisible] = React.useState(false);

  const direction = 'bottom';

  return (
    <>
      <Button sz='l' onClick={() => setVisible(!visible)}>Open Drawer</Button>
      <Drawer visible={visible} maskBg='rgba(0,0,0,0.45)' direction={direction} outerClick={() => setVisible(!visible)}>
        <Box height='200px'>{ direction.toUpperCase() }</Box>
      </Drawer>
    </>
  )
}
```

```tsx live title=Left enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  let [visible, setVisible] = React.useState(false);

  const direction = 'left';

  return (
    <>
      <Button sz='l' onClick={() => setVisible(!visible)}>Open Drawer</Button>
      <Drawer visible={visible} maskBg='rgba(0,0,0,0.45)' direction={direction} outerClick={() => setVisible(!visible)}>
        <Box width='200px'>{ direction.toUpperCase() }</Box>
      </Drawer>
    </>
  )
}
```

## Implementation

|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  | :----:  |
| visible | boolean | true | false | Is Drawer visible |
| bg | string | false | '#ffffff' | Drawer's backgroud |
| maskBg | string | false | - | Drawer's mask backgroud |
| childProps | Object | false | {} | Drawer internal elements |
| direction  | 'top' \| 'bottom' \| 'left' \| 'right' | false | 'right' | Drawer direction |
| outerClick | Function | false | - | Drawer's mask click event |
