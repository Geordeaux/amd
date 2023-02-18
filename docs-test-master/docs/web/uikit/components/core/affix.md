---
id: affix
title: Affix
---

## Overview

Pin page elements in the visible range

## Best practices

When the content area is relatively long and the page needs to be scrolled, the operations or navigation corresponding to this part of the content must always be displayed within the scrolling range. Often used for side menus and button combinations.

If the viewable range of the page is too small, use this function with caution to avoid obstructing the content of the page.

## Usage

```tsx live title=Basic,Test enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const [top, setTop] = React.useState(60);
  const [bottom, setBottom] = React.useState(10);

  return (
    <>
      <Affix offsetTop={top}>
        <Button sz='l' onClick={() => setTop(top + 10)}>Affix top</Button>
      </Affix>
      <p/>
      <Affix offsetBottom={bottom}>
        <Button sz='l' onClick={() => setBottom(bottom + 10)}>Affix bottom</Button>
      </Affix>
    </>
  );
}
```

:::note Operation tips
Open the developer tools, scroll the window to view the effect
:::

<!-- ### With Container

```tsx live title="test" enableSwitchTheme enableExportToCodePen enableHideEditor

() => {
  const [container, setContainer] = useState(null);

  return (
    <div className='demo-checkerboard-wrap' ref={setContainer}>
      <div className='demo-checkerboard-inner '>
        <Affix containerRef={container}>
          <Button sz="l">Fixed at top of the container</Button>
        </Affix>
      </div>
    </div>
  );
}
``` -->

## Implementation

|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  | :----:  |
| offsetTop | number | false | 0 | Triggered when the specified offset is reached from the top of the window. |
| offsetBottom | number | false | 0 | Triggered when the specified offset is reached from the bottom of the window. |
