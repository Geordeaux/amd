---
id: flex
title: Flex
---

## Overview
Responsive flexbox layout component

## Usage

```tsx live title=flex enableSwitchTheme enableExportToCodePen enableHideEditor
<Flex>
  <Box
    p={3}
    width={1/2}
    color='white'
    bg='primary'>
    Flex
  </Box>
  <Box
    p={3}
    width={1/2}
    color='white'
    bg='secondary'>
    Box
  </Box>
</Flex>
```

## Implementation

📦  Ergonomic, responsive React layout and grid system.
The original *Box* component™ since 2015

- Primitive styled components for all your layout needs
- Customize styles inline with the `sx` prop
- Ergonomic responsive array-based values
- Support for component variants
- Themeable and compatible with the Theme Specification
- Built with Styled System
- Works with Theme UI
- Built with Emotion, with support for Styled Components

### Variants

Reflexbox components also accept a `variant` prop, which allows you to define commonly used styles,
such as cards, badges, and CSS grid layouts, in your theme object for reuse.

Add a `variants` object to your theme and include any variants as style objects. These styles can reference other values in your theme such as colors, typographic styles, and more.
### Styled System Props

Reflexbox conforms to the Theme Specification and includes many common Styled System props.
The `Box` and `Flex` components accept the following props:

#### Space Props

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

#### Layout Props

Prop | Theme Key
---|---
`width` | `sizes`
`height` | `sizes`
`minWidth` | `sizes`
`maxWidth` | `sizes`
`minHeight` | `sizes`
`maxHeight` | `sizes`

#### Typography Props

Prop | Theme Key
---|---
`fontFamily` | `fonts`
`fontSize` | `fontSizes`
`fontWeight` | `fontWeights`
`lineHeight` | `lineHeights`
`letterSpacing` | `letterSpacings`
`fontStyle` | N/A
`textAlign` | N/A

#### Color Props

Prop | Theme Key
---|---
`color` | `colors`
`backgroundColor`, `bg` | `colors`
`opacity` | N/A

#### Flexbox Props

Prop | Theme Key
---|---
`alignItems` | N/A
`alignContent` | N/A
`justifyItems` | N/A
`justifyContent` | N/A
`flexWrap` | N/A
`flexDirection` | N/A
`flex` | N/A
`flexGrow` | N/A
`flexShrink` | N/A
`flexBasis` | N/A
`justifySelf` | N/A
`alignSelf` | N/A
`order` | N/A

