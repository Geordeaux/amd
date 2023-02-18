---
id: space
title: Space
---

## Overview
React component for applying responsive margin and padding to child elements without a wrapping HTML container. Built with Styled System.


## Usage

```tsx enableExportToCodePen enableSwitchTheme enableHideEditor
import React from 'react'
import Space from '@rebass/space'

// Apply margin to child components without a wrapping <div>
const App = props => (
  <Space mx={3} my={[ 2, 3 ]}>
    <h1>Hello</h1>
    <h2>Hi</h2>
    <button>Beep</button>
  </Space>
)
```

## Implementation

The Space component uses Styled System's space utility to add margin and padding props.

Prop | Theme Key
---|---
`margin`, `m`         | `space`
`marginTop`, `mt`     | `space`
`marginRight`, `mr`   | `space`
`marginBottom`, `mb`  | `space`
`marginLeft`, `ml`  | `space`
`marginX`, `mx`  | `space`
`marginY`, `my`  | `space`
`padding`, `p`         | `space`
`paddingTop`, `pt`     | `space`
`paddingRight`, `pr`   | `space`
`paddingBottom`, `pb`  | `space`
`paddingLeft`, `pl`    | `space`
`paddingX`, `px`  | `space`
`paddingY`, `py`  | `space`

