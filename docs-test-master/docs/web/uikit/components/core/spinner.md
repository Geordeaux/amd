---
id: spinner
title: Spinner
---

## Overview

Used for the loading status of pages and blocks.

## Best practices

## Usage

```tsx live enableSwitchTheme enableExportToCodePen enableHideEditor
<>
  <Spinner label="Loading..." />
  <p />
  <Spinner itemsColor="#000" />
</>
```

## Implementation

|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  | :----:  |
| variant | string | false | default | see: [Box](/docs/web/uikit/components/box) |
| itemsColor | string  | false | primary | Spinner item's backgroundColor |
| label | string | false | - | Spinner's label |
