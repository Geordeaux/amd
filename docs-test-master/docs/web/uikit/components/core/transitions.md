---
id: transitions
title: Transitions
---

## Overview

Fade out animation effect

## Best practices

Basic display and hiding effects for elements

## Usage

```tsx live enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const [state, setState] = React.useState(true);

  return (
    <>
      <Button sz='l' onClick={() => setState(!state)}>Toggle button</Button>
      <FadeTransition show={state}>
        <Box mt="xs" p="xs" bg="primary">Fade Transition Element</Box>
      </FadeTransition>
    </>
  )
}
```

## Implementation

|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  | :----:  |
| show | boolean | true | false | fade in/out |
| duration | number  | false | 300 | Transition time, unit: **millisecond** |
| children | JSX.Element  | true | - | Transition's element |
