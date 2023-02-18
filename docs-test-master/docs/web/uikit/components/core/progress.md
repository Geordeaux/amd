---
id: progress
title: Progress
---

## Overview

Show the current progress of the operation

## Best practices

When an operation takes a long time to complete, show the user the current progress and status of the operation.

* When an operation will interrupt the current interface, or need to run in the background, and may take more than 2 seconds.
* When you need to display the percentage of an operation completed.

## Usage

```tsx live title=Basic enableSwitchTheme enableExportToCodePen enableHideEditor
<>
  <Flex alignItems='center'>
    <Progress percent={30} />
    <Text ml={2} width='60px'>30%</Text>
  </Flex>
  <Flex alignItems='center'>
    <Progress percent={50} />
    <Text ml={2} width='60px'>50%</Text>
  </Flex>
  <Flex alignItems='center'>
    <Progress percent={100} />
    <Text ml={2} width='60px'>100%</Text>
  </Flex>
</>
```

## Implementation

|  name   | type  | isRequired | default | description |
|  :----:  | :----:  | :----:  | :----:  | :----:  |
| percent | nmber | true | 0 | percentage |
