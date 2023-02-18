---
id: portal
title: Portal
---

## Overview

Portal is able to inject the node to the certain layer.

## Usage

```tsx live title=size enableExportToCodePen enableSwitchTheme enableHideEditor
() => {
  const container = React.useRef(null)
  const [show, setShow] = React.useState(false)
  const handleClick = () => {
    setShow(!show)
  }

  return (
    <>
      <Button onClick={handleClick}>
        {show ? 'Unmount children' : 'Mount children'}
      </Button>

      <Box my={2} height={30} px={2} sx={{ border: '1px solid black' }} >
        It looks like I will render here.
        {show ? (
          <Portal container={container.current}>
            <Box>But I actually render here!</Box>
          </Portal>
        ) : null}
      </Box>
      <Box height={30} px={2} sx={{ border: '1px solid blue' }} ref={container} />
    </>
  )
}
```

## Implementation
|  name   | type  | isRequired | default |
|  :----:  | :----:  | :----:  | :----:  |
| children | React.ReactNode (do not use array for disablePortal mode) | true | |
| container | React.ReactInstance \| (() => React.ReactInstance \| null) \| null  | false | document.body (be careful for SSR)  |
| disablePortal | boolean  | false |  |


