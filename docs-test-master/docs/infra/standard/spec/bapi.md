---
id: bapi
title: bapi
---

## bapi全链路业务域隔离

### 背景

目前在币安的流量请求模型是：

CDN->Nginx->Gateway

在CDN入口流量上不支持正则表达式，而把业务域做为请求的URL前缀能够让CDN动态的进行切分，而最终的效果是：

CDN->future Nginx->future Gateway
CDN->fiat Nginx->fiat Gateway
…..

这样的效果是保证了所有业务域都是隔离的，互相不影响，而慢接口也不会影响快接口，真正做到快速车道走快车，慢速车道走慢车；

### 步骤

后端gateway支持`/bapi`为前缀的url请求，并按照业务域进行拆分

### 注意

整个bapi只不过是把原来的`/gateway-api/exchange-api`转化为`/bapi/`业务域来划分，从原有的整个URL就可以大概推测该URL属于那个业务域，请大家灵活判断;

### 业务域

```
/bapi/accounts
/bapi/futures
/bapi/margin 
/bapi/lending
/bapi/earn
/bapi/fiat     
/bapi/asset 
/bapi/c2c
/bapi/download
/bapi/mbx
/bapi/capital
/bapi/card
/bapi/haodesk
/bapi/composite
/bapi/kyc
/bapi/slow
/bapi/hooks (外网有个工具 服务 -> 需要和内部服务通信)
```
