---
id: button
title: Button
---

## Overview
Buttons give people a way to trigger an action.


## Usage

```tsx live title=size enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
  <p>Size</p>
  <Button sz="tiny" variant="default" mr={2}>Tiny</Button>
  <Button sz="small" variant="default" mr={2}>Small</Button>
  <Button sz="normal" variant="default" mr={2}>Normal</Button>
  <Button sz="large" variant="default" mr={2}>Large</Button>
</Box>
```


```tsx live title=MinWidth enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
<p>MinWidth</p>
<Button sz="tiny" variant="default" mr={2}>M</Button>
<Button sz="small" variant="default" mr={2}>M</Button>
<Button sz="normal" variant="default" mr={2}>M</Button>
<Button sz="large" variant="default" mr={2}>M</Button>
</Box>
```

```tsx live=true enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
<p>Content Adaptable (Customize Content)</p>
<Button sz="small" variant="default" mr={2}>
  <Box width="190px"> Fill Container (内容定宽) </Box>
</Button>
<Button sz="small" variant="default">
  <Box width="100%">Hug Contents (跟随内容长度)</Box>
</Button>
</Box>
```

```tsx live=true enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
<p>Primary Button</p>
<Button sz="normal" variant="default" mr={2}>Content</Button>
<Button sz="normal" variant="default" inactive mr={2}>Inactive</Button>
<Button sz="normal" variant="default" disabled>Disabled</Button>
</Box>
```


```tsx live=true enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
<p>Secondary Button</p>
<Button sz="normal" variant="default" colorStyle="secondary" mr={2}>Content</Button>
<Button sz="normal" variant="default" colorStyle="secondary" inactive mr={2}>Inactive</Button>
<Button sz="normal" variant="default" sx={{color: '#B7BDC6'}} colorStyle="secondary" disabled>Disabled</Button>
</Box>
```

```tsx live=true enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
<p>Round Button</p>
<Button sz="normal" variant="round" px="12px" py="6px" mr={2}>Content</Button>
<Button sz="normal" variant="round" px="12px" py="6px" inactive mr={2}>Inactive</Button>
<Button sz="normal" variant="round" px="12px" py="6px" disabled>Disabled</Button>
</Box>
```

```tsx live=true enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
<p>Gray Button</p>
<Button sz="normal" variant="graytype" mr={2} px="24px" py="8px">
  <Flex width="132px" alignItems="center" justifyContent="center">
    <CheckS24/>
    <Box ml="5px" sx={{ textAlign: 'left' }}>
      <Text sx={{ opacity: .7, lineHeight: '12px' }}>Title</Text>
      <Text  sx={{ lineHeight: '16px' }}>Content</Text>
    </Box>
  </Flex>
</Button>
<Button sz="normal" variant="graytype" inactive mr={2} px="24px" py="8px">
  <Flex width="132px" alignItems="center" justifyContent="center">
    <CheckS24/>
    <Box ml="5px" sx={{ textAlign: 'left' }}>
      <Text sx={{ opacity: .7, lineHeight: '12px' }}>Title</Text>
      <Text  sx={{ lineHeight: '16px' }}>Inactive</Text>
    </Box>
  </Flex>
</Button>
</Box>
```

```tsx live=true enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
<p>Text Button</p>
<Button sz="normal" variant="text" mr={2}>Content</Button>
<Button sz="normal" variant="text" disabled>Disabled</Button>
</Box>
```

```tsx live=true enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
<p>Special Button</p>
<Button sz="normal" variant="default" colorStyle="sell" mr={2}>
  <Box as="p" sx={{ textAlign: 'center', width: '132px' }}>Sell</Box>
</Button>
<Button sz="normal" variant="default" colorStyle="buy" mr={2}>
  <Box as="p" sx={{ textAlign: 'center', width: '132px' }}>Buy</Box>
</Button>
<Button sz="normal" variant="default" colorStyle="destructive" mr={2}>
  <Box as="p" sx={{ textAlign: 'center', width: '132px' }}>Destructive</Box>
</Button>
<br/><br/>
<Button sz="large" variant="default"  colorStyle="flatprimary" mr={2}>flatprimary</Button>
<Button sz="normal" variant="default"  colorStyle="flatprimary" mr={2}>flatprimary</Button>
<Button sz="small" variant="default"  colorStyle="flatprimary" mr={2}>flatprimary</Button>
<Button sz="tiny" variant="default"  colorStyle="flatprimary" mr={2}>flatprimary</Button>
<br/><br/>
<Button sz="large" variant="default"  colorStyle="flatpure" mr={2}>flatpure</Button>
<Button sz="normal" variant="default"  colorStyle="flatpure" mr={2}>flatpure</Button>
<Button sz="small" variant="default"  colorStyle="flatpure" mr={2}>flatpure</Button>
<Button sz="tiny" variant="default"  colorStyle="flatpure" mr={2}>flatpure</Button>
<br/><br/>
<Button sz="dwarf" colorStyle="secondary">Isolated</Button>
<Button sz="dwarf" colorStyle="secondary" ml={2}>25x</Button>
<br/><br/>
<Box width="256px">
  <Button sz="giant" variant="default" colorStyle="buy">
    BUY BTC
  </Button>
</Box>
<br/><br/>
</Box>
```
## Props & API
|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  | :----:  |
| variant | 'default' \| 'outline' \| 'quiet' \| 'primary' \| 'round' \| 'graytype' | false | default  | shape of the button |
| colorStyle | 'primary' \| 'secondary' \| 'negative' \| 'buy' \| 'sell'\| 'destructive' | false | primary | color style of the button |
| sz | 'dwarf' \| 'tiny' \| 'small' \| 'normal' \| 'large' \| 'giant' | false | m |  size of button |
| inactive | boolean | false | false |  whether inactive |
| onClick  | Function | false | noop | |

