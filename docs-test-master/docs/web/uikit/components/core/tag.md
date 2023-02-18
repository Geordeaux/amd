---
id: tag
title: Tag
---

## Overview

Small tags for marking and sorting.

## Best practices

* Used to mark the attributes and dimensions of things.
* As a classification mark.

## Usage

```tsx live title=Basic enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const theme = ['default', 'blue', 'green', 'red', 'gray'];

  return (
    <>
      {
        theme.map((item, index) => (
          <Tag key={item} ml={index ? 2 : 0} variant={item}>{ item }</Tag>
        ))
      }
    </>
  );
}
```

```tsx live title=With,Icons enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const theme = ['blue', 'green', 'red', 'gray'];

  return (
    <>
      <Tag bg='#55acee' color='white' display='inline-flex' alignItems='center'>
        <SocialTwitterS24 mr='1' size={16}/>
        Twitter
      </Tag>
      <Tag bg='#cd201f' color='white' display='inline-flex' alignItems='center' ml={2}>
        <SocialYoutubeS24 mr='1' size={16}/>
        Youtube
      </Tag>
      <Tag bg='#3b5999' color='white' display='inline-flex' alignItems='center' ml={2}>
        <SocialFacebookS24 mr='1' size={16}/>
        Facebook
      </Tag>
      <Tag bg='#55acee' color='white' display='inline-flex' alignItems='center' ml={2}>
        <SocialLinkedinS24 mr='1' size={16}/>
        LinkedIn
      </Tag>
    </>
  );
}
```

## Implementation

> See: [Box](/docs/web/uikit/components/box), for more Apis.
