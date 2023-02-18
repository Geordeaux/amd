---
id: dynamic-widget
title: dynamic-widget
---

# Dynamic Widget

[中文版](dynamic-widget-zh)



## Overview
  Dynamic components are a portable way to use components, which allows the business to dynamically pull the packaged component js from remote. For component upgrades, the business side does not need to re-issue the version, which can greatly reduce the business side’s component js The degree of dependence.
## System architecture diagram

![System architecture diagram](https://static.devfdg.net/image/julia/julia-doc/dy.png)


## Example
    
### document.js


```
       documentProps.mainTags.push({
               tagName: 'script',
               attrs: {
                   src:"TestWidget.js"
               },
       })
```

 ### TestWidget

 ```
 import {isServer } from '@binance/julia-utils'
 import { useEffect, useState } from 'react'

 export default () =>{

   const [Widget,setWidget] = useState(null)

   useEffect(()=>{
       if(window.TestWidget){
           setWidget(window.TestWidget)
       }
   },[])

   if(isServer){
       return "___TESTWIDGET"
   }

  return  <Widget/>

 }
 

 ```

 ## Ssr support

Need to use shuvi plugin  `shuvi-plugin-federation`

### config

```
      todo

```