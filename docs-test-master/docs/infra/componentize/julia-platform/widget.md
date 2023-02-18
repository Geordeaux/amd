---
id: widget
title: widget
---

# Julia Widget

[中文版](widget-zh)

## Generate  widget template

use generate-jw  to generate  widget template.

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

> Note: The generated julia widget widget is stored in `widget-template` under src by default

## Widget attributes

### schema
Define the configuration items of the widget, [related document](https://rjsf-team.github.io/react-jsonschema-form/) 

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
The server-side cross component data sharing scheme sets the obtained data to the namespace of ssrStore and is used by components

* Get and set the shared data of action
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
* Use action to share data 
    ``` typescript
    import { useSsrStore } from '@binance/hooks'
    
    const JuliaWidget = ({ config }) => {
         const ssrStore  = useSsrStore()
         const demoValue = ssrStore[demoKey]
    }
    ```

> Note: During render, the collector will collect the actions of all components uniformly, and then perform a de duplication operation with the same name. Only one action with the same name can exist and be executed once

###  contexts
Client side data sharing scheme, react.context

* such as  `TestProvider`
    ``` typescript jsx
    import React, { useContext } from 'react'
    
    const Context = React.createContext({})
    
    export const useTest = () => useContext(Context)
    
    export const TestProvider = ({ lng, children }) => {
      return <Context.Provider value={{ lng }} children={children} />
    }
    ```
* Use `TestProvider`
    ``` typescript
    import  { useTest } from './TestrProvider'
    
    const JuliaWidget = ({ config }) => {
        const testData = useTest()
    }
    ```
    
> Note: During render, the collector will collect the context of all components uniformly, and then perform a de duplication operation with the same name. Nested in the outermost layer of all components to complete data sharing

##  Debug widget
To debug widgets locally, you need to start the julia-ui project
``` bash
mono start -p julia-ui -w julia-widget
```
and then enter the Widget Vim page ,find the widget you need to debug in the widget list on the left

![Widget Vim](https://static.devfdg.net/image/julia/julia-doc/widget-vim.png)
