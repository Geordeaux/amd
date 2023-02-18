---
id: dialog
title: Dialog
---

## Overview

## Best practices

## Usage

```tsx live title=Basic enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const [visible, setVisible] = React.useState(false);

  return (
    <>
      <Dialog
        visible={visible}
        sx={{ width: '360px', p: 'md' }}
      >
        <Flex flexDirection="column" alignItems="center">
          <WarningFilledB96 size={96} mb="sm" />
          <Text variant="title6" as="h6" mb="xs">Safety Tip</Text>
          <Text variant="body2" mb="md" color="t.third" textAlign="center">Either Email authentication or SMS authentication should be enabled.</Text>
          <Button sz="l" width="100%" onClick={() => setVisible(false)}>I understand</Button>
        </Flex>
      </Dialog>
      <Button sz='l' onClick={() => setVisible(true)}>Open Dialog</Button>
    </>
  )
}
```

```tsx live title=Confirm enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const [visible, setVisible] = React.useState(false);

  return (
    <>
      <Dialog
        visible={visible}
        sx={{ width: '360px', p: 'md' }}
      >
        <Flex flexDirection="column" alignItems="center">
          <AcLockB96 size={96} mb="sm" />
          <Text variant="title6" as="h6" mb="xs">Safety Tip</Text>
          <Text variant="body2" mb="md" color="t.third" textAlign="center">Your account has been locked.</Text>
          <Flex width="100%">
            <Button flex="1 0 0" variant="outline" colorStyle="second" sz="l" mr="xs" onClick={() => setVisible(false)}>Don’t unlock</Button>
            <Button flex="1 0 0" sz="l" onClick={() => setVisible(false)}>Unlock</Button>
          </Flex>
        </Flex>
      </Dialog>
      <Button sz='l' onClick={() => setVisible(true)}>Open Dialog</Button>
    </>
  )
}
```

```tsx live title=Click,mask,to,close enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const [visible, setVisible] = React.useState(false);

  return (
    <>
      <Dialog
        visible={visible}
        onMaskClick={(e)=>{
          setVisible(false)
          console.log('onMaskClick:',e)
        }}
        sx={{ padding: '50px' }}
      >
        Click Mask to close the Dialog
      </Dialog>
      <Button sz='l' onClick={() => setVisible(true)}>Open Dialog</Button>
    </>
  )
}
```

```tsx live title=Click,icon,to,close enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const [visible, setVisible] = React.useState(false);

  return (
    <>
      <Dialog
        visible={visible}
        sx={{ width: '400px', height: '300px', p:'md' }}
      >
        <V3CloseS24 size={14} sx={{float: 'right'}} color="t.third" cursor="pointer" onClick={() => setVisible(false)} />
        <Text variant="title6" as="h6" mb="xs">Title</Text>
        Click icon to close the Dialog
      </Dialog>
      <Button sz='l' onClick={() => setVisible(true)}>Open Dialog</Button>
    </>
  )
}
```

```tsx live title=Bad,Usage enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const [visible, setVisible] = React.useState(false);
  const ref = React.useRef();

  return (
    <Box ref={ref} onClick={(event) => {
      setVisible(true);
      console.log('click the box!');
    }}>
      <Dialog
        visible={visible}
        onMaskClick={() => {
          // Add this trick, comment next line to see what will happens
          if (ref.current && ref.current.contains(event.target)) {
            setVisible(false);
          }

          console.log('click the mask!');
        }}
        sx={{ width: '400px', height: '300px', p:'md' }}
      >
        <V3CloseS24 size={14} sx={{float: 'right'}} color="t.third" cursor="pointer" onClick={() => setVisible(false)} />
        <Text variant="title6" as="h6" mb="xs">Title</Text>
        Now, the dialog can't be closed
      </Dialog>
      <Button sz='l'>Open Dialog</Button>
    </Box>
  )
}
```

:::caution Usage notice
The onClick handler which set Dialog visible should in a sibling instead of parent.

If you add onClick on a parent of Dialog and setVisible, You rich the bug. **We don't recommand you do this**.
:::

## Implementation

|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  | :----:  |
| visible | boolean | true | false | Is Dialog visible |
| mask | boolean | false | true | whether the mask exists  |
| maskStyles | object | false | - | mask's styles |
| onMaskClick | Function | false | - | Dialog's mask click event |
