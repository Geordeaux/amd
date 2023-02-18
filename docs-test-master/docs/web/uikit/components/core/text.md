---
id: text
title: Text
---

## Overview
Frequently used Text

## Usage

```tsx live title=Overview enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
  <Text variant="extraHeadline" as="h1">Headline 0</Text>
  // extraHeadline is extra title for the homepage.
  <Text variant="headline1" as="h1">Headline 1</Text>
  <Text variant="headline2" as="h2">Headline 2</Text>
  <Text variant="headline3" as="h3">Headline 3</Text>
  <Text variant="headline4" as="h4">Headline 4</Text>
  <Text variant="headline5" as="h5">Headline 5</Text>
  <Text variant="headline6" as="h6">Headline 6</Text>
  <Text variant="largeBody">Large Body</Text>
  <Text variant="subtitle1">Subtitle 1</Text>
  <Text variant="subtitle2">Subtitle 2</Text>
  <Text variant="body1">Body 1</Text>
  <Text variant="body2">Body 2</Text>
  <Text variant="captionSub">Caption Sub</Text>
  <Text variant="caption">Caption</Text>
  <Text variant="linkBody">Link Body</Text>
  <Text variant="linkCaption">Link Caption</Text>
  <Text variant="largeLink">Large Link</Text>
</Box>
```


```tsx live title=labels enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
  <Text variant="formLabel">Label</Text>
  <Text variant="formLabelSmall">Small label</Text>
  <Text variant="formLabelDisabled">Disabled label</Text>
  <Text variant="infoHelperText">Info helper message</Text>
  <Text variant="errorHelperText">Error helper message</Text>
</Box>
```

```tsx live title=Responsive enableExportToCodePen enableSwitchTheme enableHideEditor
<Text
  fontSize={[ 3, 4, 5 ]}
  fontWeight='bold'
  color='t.primary'>
  Hello World
</Text>
```