---
id: openApi
title: openApi
---

## **Domain**

Prod： [api.saasexch.com](https://api.saasexch.com/)

Dev：  [www.devfdg.net](https://www.devfdg.net/)



## The Explanation Of  Response



- Json struct of response

```json
{
  "code":0,  
  "msg":"ok", //error info
  "data": {
     ...
    }
}
```



- Enumeration of code

| code | info                      |
| :--- | :------------------------ |
| 0    | Success                   |
| 1001 | Parameter error           |
| 1007 | appid found               |
| 1000 | unknow error              |
| 1004 | auth failed               |
| 1005 | auth failed,token expired |







## **Api List**



### **1. /bapi/fe/usd/login?appid=xxxxx**



**Desc**:   Login to get  token

**Method** : GET

**QueryString:**

| Parameter | Desc             | Explore |
| :-------- | :--------------- | :------ |
| appid     | the buesiness id | true    |

**Response:**

```json
{
  "code":0,
  "msg":"ok",
  "data": {
      "appid":"xxxx",
      "token":"xxxx" (expired after 30 days)
    }
}
```



### **2. /bapi/fe/usd/report/upload?batch=true**

**Authentication :**

- Basic Auth (User: appid Password: token) ( you can find info in https://en.wikipedia.org/wiki/Basic_access_authentication)

  if the response code is 1004 ,that means auth failed ,please  check appid and token again  , or relogin (/bapi/report/login) to get token.

- QueryString , you can  pass parameter like this ` https://{{host}}/bapi/fe/usd/report/upload?appid={{appid}}&token={{token}}  `

You can use one of above ,  recommend Base Auth，if you used both , the server will judge by  query string first .



**Desc: **  Report the log

**Method :**  POST

**QueryString：**

| Parameter | Desc                                                         | Example |
| --------- | :----------------------------------------------------------- | :------ |
| batch     | report batch or not（default false）<br />true：report batchly  ( max number < 100, the max  size of body < 1M )<br />false：report singlely | true    |

**Body:** 

| Type   | json struct in body     |
| :----- | :---------------------- |
| Single | `{...}`                 |
| Batch  | `{"data_list":[{...}]}` |

**Response:**

```
{
  "code":0,
  "msg":"ok"
  "data":null
}
```



### 3. /bapi/fe/usd/report/poupload

**Authentication** :

- Basic Auth (User: appid Password: token) ( you can find info in https://en.wikipedia.org/wiki/Basic_access_authentication)

  if the response code is 1004 ,that means auth failed ,please  check appid and token again  , or relogin (/bapi/report/login) to get token.

- QueryString , you can  pass parameter like this ` https://{{host}}/bapi/fe/usd/report/upload?appid={{appid}}&token={{token}}  `

You can use one of above ,  recommend Base Auth，if you used both , the server will judge by  query string first .



**Desc:** `report log for Polaris`

**Method:** `POST`

**Body:**  ` json farmat  {...}`

**Response:**

```
{
  "code":0,
  "msg":"ok"
  "data":null
}
```



### 4. /bapi/fe/usd/sensor

The api for proxying  the data to sensors, and sending  to  Kafka  of the our sidde meanwhile . You must keep the header and data struct  same   with requesting  sensors api

The doc for sensors  is https://manual.sensorsdata.cn/sa/latest/tech_sdk_server_golang-17569150.html



| Buesiness    | Kafka-topic |
| :----------- | :---------- |
| 神策数据转发 | fe-sensors  |
