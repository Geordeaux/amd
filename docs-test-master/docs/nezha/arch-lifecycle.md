---
id: arch-lifecycle
title: Lifecycle
sidebar_label: Lifecycle
---

## Lifecycle

## Trigger by web side

#### beforeMount
Before the web page mounted

#### mounted
When the web page mounted

#### beforeUpdate
Before the web page update
  
#### updated
When web page update

#### unmounted
When the web page unmounted


## Trigger by native side

#### App.onLaunch
When load the app

#### App.onShow
When the app go to frontdesk

#### App.onHide
Hide the app to the background mode or kill the whole app

#### Page.onLoad
When load the page

#### Page.onShow
When the page go to frontdesk

#### Page.onHide
Hide the page to the background mode

#### Page.onUnload
When unload page


### Lifecycle Flow
![lifecycle](https://static.devfdg.net/static/mono-static/docs-ui/img/lifecycle_graph.png)

![lifecycle-detail](https://static.devfdg.net/static/mono-static/docs-ui/img/page-lifecycle_detail.png)

### Lifecycle Events

- [App lifecycle events](./rpc-event#app-lifecycle-events)
- [Page lifecycle events](./rpc-event#page-lifecycle-events)


## Reference
* https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html
* https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page-life-cycle.html
* https://vuejs.org/v2/guide/instance.html#Lifecycle-Diagram
