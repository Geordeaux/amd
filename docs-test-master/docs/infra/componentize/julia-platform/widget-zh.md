---
id: widget-zh
title: 组件
---

[English Version](widget)

## Generate widget template

使用 `generate-jw` 去创建组件.

### usage
``` bash
mono generate --type jw
```

### Enter widget name
![generate-jw](https://static.devfdg.net/image/julia/docs/mini-program/julia-two.jpg)

### Enter widget path
![generate-jw](https://static.devfdg.net/image/julia/docs/mini-program/julia-one.jpg)


### Template code
```typescript jsx
import React from 'react'
import { Box, Flex } from '@binance/uikit-core'
import { ComLayout } from '@binance/julia-share'
import { JuliaWidget, JuliaProps } from '@binance/julia-types'

const JuliaWidget: JuliaWidget = ({ py, py1, py2 }: JuliaProps) => {}

JuliaWidget.actions = {}
JuliaWidget.displayName = ''
JuliaWidget.contexts = {}
if (process.env.julia_env !== 'production') {
  JuliaWidget.schema = {
    type: 'object',
    title: 'JuliaWidget',
    properties: {},
  }
}

export default React.memo(JuliaWidget)
```

> Note: 默认情况下，生成的组件是在`src/widget-template`目录下


## Widget attributes

### schema
schema的数据结构使用的是 [react-jsonschema-form](https://rjsf-team.github.io/react-jsonschema-form/) 

``` typescript
import { BASE_CONFIG } from '../../constants'

const JuliaWidget = ({ config,py,py1,py2 }) => {
  // py  py1 py2 is BASE_CONFIG
}
JuliaWidget.schema = {
  type: 'object',
  title: 'footer Community props',
  properties: {
    ...BASE_CONFIG, // julia widget common properties
    config: {
       // todo -- julia widget custom properties
    },
  },
  }
```

###  actions
> server端跨组件共享数据方案，将获取的数据设置到ssrStore的namespace上，被组件使用

* 获取并设置action的共享数据
    ``` typescript
    const demoAction = async ({ ctx, config }) => {
        const { dispatch } = ctx.store 
        const {  } = config || {}  // Data defined by the schema of action
        // ...
        const data = await demoFetch() // Logic to get data. Business customization, here is just a simple demonstration
        // ...
        dispatch.ssrStore.updateState({ 
            [demoKey]: data  //Set data to ssrStore.
        })
    }
    ```
* 使用action共享数据
    ``` typescript
    import { useSsrStore } from '@binance/hooks'
    
    const JuliaWidget = ({ config }) => {
         const ssrStore  = useSsrStore()
         const demoValue = ssrStore[demoKey]
    }
    ```

> Note: render的过程中，收集器会统一收集所有组件的action，然后进行一次同名去重操作。同名的action只会存在一个且只执行一次

###  contexts
> client端共享数据方案，React.Context

* context的定义，例如  `TestProvider`
    ``` typescript jsx
    import React, { useContext } from 'react'
    
    const Context = React.createContext({})
    
    export const useTest = () => useContext(Context)
    
    export const TestProvider = ({ lng, children }) => {
      return <Context.Provider value={{ lng }} children={children} />
    }
    ```
* 使用 `TestProvider`
    ``` typescript
    import  { useTest } from './TestrProvider'
    
    const JuliaWidget = ({ config }) => {
        const testData = useTest()
    }
    ```

> Note: render的过程中，收集器会统一收集所有组件的context，然后进行一次同名去重操作。嵌套在所有组件最外层，完成数据共享

##  Debug widget
本地调试，你需要启动julia-ui project,
``` bash
mono start -p julia-ui -w julia-widget
```
然后进入Widget Vim页面，在左边的Widget列表中找到需要调试的Widget

![Widget Vim](https://static.devfdg.net/image/julia/julia-doc/widget-vim.png)
