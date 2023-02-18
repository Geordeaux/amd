---
id: layout
title: Layout
---

## Overview
Themeable layout components

## Usage


```tsx live enableExportToCodePen enableSwitchTheme enableHideEditor
<Tiles width={[96, null, 128]}>
  <Box bg='primary' height='50px'/>
  <Box bg='primary' height='50px'/>
  <Box bg='primary' height='50px'/>
  <Box bg='primary' height='50px'/>
</Tiles>
```

```tsx live enableExportToCodePen enableSwitchTheme enableHideEditor
<Tiles columns={[2, null, 4]}>
  <Box bg='primary' height='50px'/>
  <Box bg='primary' height='50px'/>
  <Box bg='primary' height='50px'/>
  <Box bg='primary' height='50px'/>
</Tiles>
```

```tsx live enableExportToCodePen enableSwitchTheme enableHideEditor
<Tiles gap='4px' columns={[2, null, 4]}>
  <Box bg='primary' height='50px'/>
  <Box bg='primary' height='50px'/>
  <Box bg='primary' height='50px'/>
  <Box bg='primary' height='50px'/>
</Tiles>
```

```tsx live enableExportToCodePen enableSwitchTheme enableHideEditor
<Tiles gridTemplateColumnsWidth='3fr 2fr 2fr 3fr' gap='8px'>
  <Box bg='primary' height='200px' />
  <Box bg='primary' height='200px' />
  <Box bg='primary' height='200px' />
  <Box bg='primary' height='200px' />
  <Box bg='primary' height='200px' />
  <Box bg='primary' height='200px' />
  <Box bg='primary' height='200px' />
  <Box bg='primary' height='200px' />
  <Box bg='primary' height='200px' />
  <Box bg='primary' height='200px' />
</Tiles>
```


## Props & API
|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  | :----:  |
| gap | number | false | 3  | gap between children |
| columns | Array<number \| string \| null> \| number | false | undefined  | how many columns |
| width | Array<number \| string \| null> \| number | false | undefined  | width for children |
| gridTemplateColumnsWidth | Array<number \| string> | false | undefined  | css grid regulations |

