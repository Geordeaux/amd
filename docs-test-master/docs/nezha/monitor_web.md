---
id: web-monitor
title: Web Monitor
sidebar_label: Web Monitor
---

## Document

Action document: https://docs.fe.devfdg.net/docs/nezha/rpc-action#monitor-1

## Basic Data

the datas below are added by native, except event name(t) and timestamp(d).

| key | type   | default value | required | desc               |
| --- | ------ | ------------- | -------- | ------------------ |
| t   | String |               | √        | event name         |
| d   | long   | -             | √        | timestamp          |
| id  | String | -             | √        | appid              |
| v   | String | -             | √        | version            |
| rv  | String | -             | √        | revision           |
| cv  | String | -             | √        | common sdk version |

## Interface

Data list below are additional fields for each event.

#### app launch time

event name: `appLaunch`

data:

| key | type | default value | required | desc      |
| --- | ---- | ------------- | -------- | --------- |
| st  | long |               | √        | startTime |

#### page route time

event name: `route`

data:

| key | type   | default value | required | desc                                                       |
| --- | ------ | ------------- | -------- | ---------------------------------------------------------- |
| p   | String |               | √        | page name                                                  |
| nt  | String | -             | √        | navigation type (`navigateTo`,`navigateBack`, `switchTab`) |
| st  | long   |               | √        | startTime                                                  |

#### page inject script time

event name: `evaluateScript`

data:

| key | type   | default value | required | desc        |
| --- | ------ | ------------- | -------- | ----------- |
| p   | String |               | √        | page name   |
| st  | long   |               | √        | startTime   |

#### page render time

event name: `firstRender`

data:

| key | type   | default value | required | desc        |
| --- | ------ | ------------- | -------- | ----------- |
| p   | String |               | √        | page name   |
| st  | long   |               | √        | startTime   |

#### runtime Error

event name: `NEZHA_ERR_WEB_RUNTIME`

data:

| key | type   | default value | required | desc              |
| --- | ------ | ------------- | -------- | ----------------- |
| f   | String | work/render   | √        | from              |
| p   | String | -             | -        | page name         |
| e   | String | -             | √        | error information |
