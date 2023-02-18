---
id: checkbox
title: Checkbox
---

## Overview
Accessible and themeable form checkbox input component.


## Usage

```tsx live enableExportToCodePen enableSwitchTheme enableHideEditor
<Flex>
  <Label>
    <Checkbox  defaultChecked={false} />
    <Text variant="formLabel">Label</Text>
  </Label>
  <Label>
    <CheckboxV1 defaultChecked={true} />
    <Text variant="formLabel">Label</Text>
  </Label>
  <Label>
    <Checkbox  defaultChecked={false} indeterminate={true} />
    <Text variant="formLabel">Label</Text>
  </Label>
  <Label>
    <CheckboxV1 defaultChecked={true} indeterminate={true} />
    <Text variant="formLabel">Label</Text>
  </Label>
  <Label>
    <Checkbox  defaultChecked={false} disabled />
    <Text variant="formLabelDisabled">Label</Text>
  </Label>
  <Label>
    <Checkbox  defaultChecked={true} disabled />
    <Text variant="formLabelDisabled">Label</Text>
  </Label>
  <Label>
    <Checkbox  defaultChecked={true} indeterminate={true} disabled />
    <Text variant="formLabelDisabled">Label</Text>
  </Label>
</Flex>
```

```tsx live enableExportToCodePen enableSwitchTheme enableHideEditor
() => {
  const [checked, setChecked] = React.useState(false)

  const onChange = (e) => {
    console.log(`Checkbox change: ${e.currentTarget.checked}`)
    setChecked(e.currentTarget.checked)
  }

  return (
    <Label>
      <Checkbox
        
        defaultChecked={true}
        checked={checked}
        onChange={onChange}
      />
      <Text variant="formLabel">Remember me</Text>
    </Label>
  )
}
```


```tsx live enableExportToCodePen enableSwitchTheme enableHideEditor
<Box>
  <Box mb="lg">
    <Label mb="16px">
      <Checkbox  defaultChecked={false} />
      <Text variant="formLabel">Checkbox1</Text>
    </Label>
    <Label mb="16px">
      <Checkbox  defaultChecked={true} />
      <Text variant="formLabel">Checkbox2</Text>
    </Label>
    <Label>
      <Checkbox  defaultChecked={true}/>
      <Text variant="formLabel">Checkbox3</Text>
    </Label>
  </Box>
  <Flex mb="lg">
    <Label sx={{width: 'auto', mr: '24px'}}>
      <Checkbox defaultChecked={false} />
      <Text variant="formLabel">Checkbox1</Text>
    </Label>  
    <Label sx={{width: 'auto', mr: '24px'}}>
      <Checkbox defaultChecked={false} />
      <Text variant="formLabel">Checkbox2</Text>
    </Label>  
    <Label sx={{width: 'auto', mr: '24px'}}>
      <Checkbox defaultChecked={false} />
      <Text variant="formLabel">Checkbox3</Text>
    </Label>
  </Flex>
</Box>
```

## Implementation
|  name   | type  | isRequired | default |
|  :----:  | :----:  | :----:  | :----:  |
| defaultChecked | boolean  | false | false |
| checked | boolean  | false | false |
| size | number | false |  |
| disabled |  boolean | false | false |
| indeterminate |  boolean | false | false |
| onChange |  (event: React.FormEvent&lt;HTMLInputElement>) => void  | false |  |
| sx | Object (the container sx) | false |  |
| ref | HTMLInputElement (checkbox) | false |  |
