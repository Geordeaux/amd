---
id: dropdown
title: Dropdown
---

## Overview

Pop-up list

## Best practices

When there are too many operation commands on the page, use this component to store operation elements. Click or move into the contact, and a drop-down menu will appear. You can select from the list and execute the corresponding command.

* Used to collect a set of command operations
* `Select` is used for selection, and `Dropdown` is a collection of commands

## Usage

```tsx live title=Basic enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const handleClick = value => {
    console.log(`You clicked: ${value}`);
  }

  const [disabled, setDisabled] = React.useState(false);

  const menuOptions = ['1st menu item', '2st menu item'];

  const Menu = (
    <Box bg='primary' mt='2'>
      {
        menuOptions.map(item => (
          <Box key={item} px='plus' py='xs' onClick={() => { handleClick(item) }}>
            <Text>{item}</Text>
          </Box>
        ))
      }
    </Box>
  );
  
  return (
    <>
      <Dropdown
        disabled={disabled}
        overlay={Menu}>
        <Button sz='l'>Click me <ChevronDownD ml='1' size={16}/></Button>
      </Dropdown>
      <Button ml="ls" sz='l' onClick={() => setDisabled(!disabled)}>Dropdown {disabled ? 'enable' : 'disable' }</Button>
    </>
  );
}
```

```tsx live title=Trigger,Ways enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const menuOptions = ['1st menu item', '2st menu item'];

  const handleClick = value => {
    console.log(`You clicked: ${value}`);
  }

  const Menu = (
    <Box bg='primary' mt='2'>
      {
        menuOptions.map(item => (
          <Box key={item} px='plus' py='xs' onClick={() => { handleClick(item) }}>
            <Text>{item}</Text>
          </Box>
        ))
      }
    </Box>
  );

  return (
    <>
      <Dropdown
        trigger='click'
        overlay={Menu}>
        <Button sz='l'>Click me <ChevronDownD ml='1' size={16}/></Button>
      </Dropdown>
      <Dropdown
        trigger='hover'
        overlay={Menu}>
        <Button ml="ls" sz='l'>Hover me <ChevronDownD ml='1' size={16}/></Button>
      </Dropdown>
    </>
  );
}
```

```tsx live title=Pop-up,position enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const menuOptions = ['1st menu item', '2st menu item'];

  const handleClick = value => {
    console.log(`You clicked: ${value}`);
  }

  const Menu = (
    <Box bg='primary' mt='2'>
      {
        menuOptions.map(item => (
          <Box key={item} px='plus' py='xs' onClick={() => { handleClick(item) }}>
            <Text>{item}</Text>
          </Box>
        ))
      }
    </Box>
  );

  const topPlacementOptions = [
    'top-start',
    'top',
    'top-end'
  ];
  
  const rightPlacementOptions = [
    'right-start',
    'right',
    'right-end'
  ];

  const bottomPlacementOptions = [
    'bottom-start',
    'bottom',
    'bottom-end'
  ];

  const leftPlacementOptions = [
    'left-start',
    'left',
    'left-end'
  ];

  return (
    <>
      {
        topPlacementOptions.map(placement => (
          <Dropdown
            key={placement}
            trigger='hover'
            placement={placement}
            overlay={Menu}>
            <Button sz='l'>{placement}</Button>
            <Box pl={2} pr={2} display='inline-block'></Box>
          </Dropdown>
        ))
      }
      <p/>
      {
        rightPlacementOptions.map(placement => (
          <Dropdown
            key={placement}
            trigger='hover'
            placement={placement}
            overlay={Menu}>
            <Button sz='l'>{placement}</Button>
            <Box pl={2} pr={2} display='inline-block'></Box>
          </Dropdown>
        ))
      }
      <p/>
      {
        bottomPlacementOptions.map(placement => (
          <Dropdown
            key={placement}
            trigger='hover'
            placement={placement}
            overlay={Menu}>
            <Button sz='l'>{placement}</Button>
            <Box pl={2} pr={2} display='inline-block'></Box>
          </Dropdown>
        ))
      }
      <p/>
      {
        leftPlacementOptions.map(placement => (
          <Dropdown
            key={placement}
            trigger='hover'
            placement={placement}
            overlay={Menu}>
            <Button sz='l'>{placement}</Button>
            <Box pl={2} pr={2} display='inline-block'></Box>
          </Dropdown>
        ))
      }
    </>
  );
}
```

```tsx live title=Fixed enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const menuOptions = ['1st menu item', '2st menu item'];

  const handleClick = value => {
    console.log(`You clicked: ${value}`);
  }

  const Menu = (
    <Box bg='primary' mt='2'>
      {
        menuOptions.map(item => (
          <Box key={item} px='plus' py='xs' onClick={() => { handleClick(item) }}>
            <Text>{item}</Text>
          </Box>
        ))
      }
    </Box>
  );

  return (
    <>
      <Dropdown
        trigger='click'
        isFixed={true}
        overlay={Menu}>
        <Button ml='2' sz='l'>Click me <ChevronDownD ml='1' size={16}/></Button>
      </Dropdown>
    </>
  );
}
```

:::note Operation tips
Click to show and fixed the dropdown (scroll page and see the magic).

It's not a bug. It's a feature.
:::

```tsx live title=EnablePortal enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const handleClick = value => {
    console.log(`You clicked: ${value}`);
  }

  const menuOptions = ['1st menu item', '2st menu item'];

  const Menu = (
    <Box bg='primary' mt='2'>
      {
        menuOptions.map(item => (
          <Box key={item} px='plus' py='xs' onClick={() => { handleClick(item) }}>
            <Text>{item}</Text>
          </Box>
        ))
      }
    </Box>
  );

  return (
    <Box width='200px' height='50px' __css={{willChange: 'transform',overflow: 'hidden'}}>
      <Dropdown
        trigger="hover"
        enablePortal
        overlay={Menu}>
        <Button sz='l'>Hover me <ChevronDownD ml='1' size={16}/></Button>
      </Dropdown>
      </Box>
  );
}
```

:::note
If one of its ancestors has a transform, perspective, or filter property set to something other than none (see the CSS Transforms Spec), in which case that ancestor behaves as the containing block. So we should set enablePortal. 

see: [https://developer.mozilla.org/en-US/docs/Web/CSS/position](https://developer.mozilla.org/en-US/docs/Web/CSS/position)
:::

```tsx live title=ForceTop enableSwitchTheme enableExportToCodePen enableHideEditor
() => {
  const handleClick = value => {
    console.log(`You clicked: ${value}`);
  }

  const menuOptions = ['1st menu item', '2st menu item'];

  const Menu = (
    <Box bg='primary' mt='2'>
      {
        menuOptions.map(item => (
          <Box key={item} px='plus' py='xs' onClick={() => { handleClick(item) }}>
            <Text>{item}</Text>
          </Box>
        ))
      }
    </Box>
  );

  return (
    <Box width='200px' height='50px' __css={{overflow: 'hidden'}}>
      <Dropdown
        trigger="hover"
        forceTop
        overlay={Menu}>
        <Button sz='l'>Hover me <ChevronDownD ml='1' size={16}/></Button>
      </Dropdown>
      </Box>
  );
}
```

## Implementation

|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  | :----:  |
| enablePortal | boolean | false | undefined | see: [demo](/docs/web/uikit/components/dropdown#enableportal) |
| trigger | 'hover' \| 'click' | false | hover | Trigger way |
| children | React.ReactFragment \| React.ReactChild | true | - | Dropdown internal elements |
| disabled | boolean | fasle | undefined | Whether the Dropdown is disabled |
| isFixed | boolean | false | false | Fixed the Dropdown on the screen |
| forceTop| boolean | false | false | see: [demo](/docs/web/uikit/components/dropdown#enableportal) |
| onVisibleChange | (visible: boolean) => void | false | - | Called when the menu display status changes |
| open    | boolean | false | - | show Dropdown |
| overlay | string \| number  \| JSX.Element \| React.ReactFragment \| React.ReactChild | true | - | Dropdown items |
| overlayProps | BoxProps | false | {} | - |
| placement | 'auto'  \| 'top'  \| 'right'  \| 'left'  \| 'bottom'  \| 'top-start'  \| 'right-start'  \| 'left-start'  \| 'bottom-start'  \| 'top-end'  \| 'right-end'  \| 'left-end'  \| 'bottom-end' | fasle | 'auto'| Menu pop-up position |
