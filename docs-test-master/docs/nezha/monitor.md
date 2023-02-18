---
id: monitor
title: Native Monitor
sidebar_label: Native Monitor
---

## Reference

https://confluence.toolsfdg.net/pages/viewpage.action?pageId=12589175

https://confluence.toolsfdg.net/pages/viewpage.action?pageId=45719567

## Basic Data

| key | type | default value | required | desc |
|---------|----|----|----|----|
| t     |  String  |   |  √| monitor type  |
| d     | long |  -  | √|  timestamp |
| id   | String |  -  | √|  appid |
| v   | String |  -  | √|  version |
| rv   | String |  -  | √|  revision |
| cv   | String |  -  | √|  common sdk version |

## Error

### Page does not exist

| key | type | default value | required | desc |
|---------|----|----|----|----|
| t     |  String  |   NEZHA_ERR_PAGE_MISS |  √|   |
| p     | String |  -  | √|  page name |
| e     | String |  -  | √|  error information  |

### WebView Error

| key | type | default value | required | desc |
|---------|----|----|----|----|
| t     |  String  |   NEZHA_ERR_WEBVIEW|  √|   |
| f     | String |  work/render  | √|  from |
| p   | String |  -  | -|  page name |
| e     | String |  -  | √|  error information  |

### Plugin call error

| key | type | default value | required | desc |
|---------|----|----|----|----|
| t     |  String  |  NEZHA_ERR_PLUGIN |  √ |   |
| n   | String |  -  | √|  plugin name |
| e     | String |  -  | √|  error information  |


### Resource download failed (including timeout)

| key | type | default value | required | desc |
|---------|----|----|----|----|
| t     |  String  |  NEZHA_ERR_DOWNLOAD |  √|   |
| e     | String |  -  | √|  error information  |

### Resource unarchiving failed

| key | type | default value | required | desc |
|---------|----|----|----|----|
| t     |  String  | NEZHA_ERR_UNARCHIVE |  √|   |
| e     | String |  -  | √|  error information  |


## Performance


### Mini program resource download time

| key | type | default value | required | desc |
|---------|----|----|----|----|
| t     |  String  | NEZHA_DOWNLOAD |  √|   |
| dt     |  String  | block/background |  √|  downloadType |
| tt     | long |  -  | √|  totalTime  |
| req   | long |  -  | √|  requestTime |
| dow   | long |  -  | √|  downloadTime |
| un  | long |  -  | √|  unarchiveTime |


### Page View (PV)

| key | type | default value | required | desc |
|---------|----|----|----|----|
| t     |  String  | NEZHA_PAGE_PV |  √ |   |
| p     | String |  -  | √|  page name |
