---
id: dynamic-widget-zh
title: 动态组件
---

[English Version](dynamic-widget)



## 背景
   动态组件是一种轻便式的组件使用方式，可以让业务动态的从远程拉去已经打包好的组件js，对于组件的升级改动，业务方无需重新发版，能极大的减轻业务方对组件的依赖程度。
## 系统架构图(System architecture diagram)

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

 ## Ssr 支持

需要使用shuvi 插件  `shuvi-plugin-federation`

### config

```
      todo

```