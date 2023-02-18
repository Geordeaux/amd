---
id: ws-http-sdk-sdk
title: sdk
---

import Mermaid from '@theme/Mermaid'

## Overview

This project aimed to reduce the connection and the amount of data in transmission.

- use[`websocket`](#reduce-connection) to reduce connection
- use [`protobuf`](#protobuf) to compress

## Reduce connection

### Before

<Mermaid
  chart={
    `graph LR
    User1/api1 --> GateWay
    User1/api2 --> GateWay
    User1/api3 --> GateWay
    User2/api1 --> GateWay
    User2/api2 --> GateWay`
  }
/>

### After

<Mermaid
  chart={
    `graph LR
    User1/api1 --> Websocket1
    User1/api2 --> Websocket1
    User1/api3 --> Websocket1
    User2/api1 --> Websocket2
    User2/api2 --> Websocket2
    Websocket1 --> GateWay
    Websocket2 --> GateWay`
  }
/>

## Protobuf

A compression algorithm，using the same schema from backend to frontend, we can replace the key(which is fixed) information with position.(eg. Id is always in position 1, so we get id from position 1, and assign it to key id.)

![compress](https://static.devfdg.net/static/mono-static/docs-ui/img/ws-http-sdk/compress.png)

## Concerns

### Keep the connection alive -- Ping! Pong! 🏓

Websocket is a transmission protocol, it uses TCP building connection. TCP can't notify disconnection. Common solution is sending heartbeat to the opposite party to confirm the connection is alive or not.

Currently, client need to send a ping frame 3minutes - 2 seconds - 2 seconds, if there's no response, reconnect.

### Timeout

Websocket is a queue of request, no guarantee that when server would get the request. So the client need to deal with timeout logic.like 10s without response, just reject this request.

### Websocket match request and response

Every request and response need to have a wsReqId to match. Since we have a multiple to multiple connection using websocket.

### Sync cookie (token, uuid, lang)

#### Sync overview

Websocket have no access to cookie. We need to sync some common cookie.

- token
- bnc-uuid
- lang

#### Token

Login status change can be detected by cookie `cr00`. Get token from server with restful api and sync with websocket server.

##### Schedule

RefreshToken is only needed when user need to call private api.

So we check the login status every time when user's called the private api and decide wether or not refreshToken.

<Mermaid
  chart={
    `graph LR
    A(call private api) --> B{check if login status changed}
    B --> C(yes)
    C --> D(fire the private api)
    B --> E(no)
    E --> F(refreshToken)
    F --> G(fire the private api)`
  }
/>

1. User fire a private api request
2. Check if the login status changes

##### Restful api for token

`/gateway-api/v1/private/market/ws-token`

##### Tell the login status (Web developer only⚠️)

Frontend use the cookie cr00 to tell the difference.

- `cr00` is exist when the user is logged in.(logged In ✅)

- `cr00` is different for every login.(same user ✅)

Based on `cr00` we can use refreshToken to refresh the token or clearToken to clear the token.

#### bnc-uuid

`bnc-uuid` is used to identify the device，we can sync this information to tracing some error.

#### lang

`lang` is used to specify the lang for the response.

### Reconnect websocket

If the connection is broken

- Closed from client side

- server not response for ping pong

We can reconnect the websocket with 0s - 5s - 10s - 20s....

### Refresh token

If user need to call private api and find out the token is not latest

We can reconnect the websocket with 0s - 1s - 2s - 4s....

### Fallback

The restful api using websocket is not stable right now, business developer might want to fallback to the old restful api.

## Workflow

![workflow](https://static.devfdg.net/static/mono-static/docs-ui/img/ws-http-sdk/workflow.png)


## API Wrapper

Check api.md for more information: `mono\libs\ws-http-sdk\api.md`

## Business developer

Checkout README.md for more information: `mono\libs\ws-http-sdk\README.md`


### Resource

1. protobuf schema: `mono\protos\schema`
2. backend support: `https://git.toolsfdg.net/jack-jiang/websocket-server`

### Todo

- [ ] unit test

