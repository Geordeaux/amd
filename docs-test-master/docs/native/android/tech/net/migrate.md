---
id: migrate
title: The Public API is migrated to OkHttp
hide_title: true
---
# The Public API is migrated to OkHttp

## why

At present, the data request mode of Public API in the project is too old and nobody maintains it, and the business of requesting API is scattered everywhere, which is not conducive to maintenance and later componentization.

1.Existing business Public API data requests are implemented in Java code and the underlying libraries are unmaintained

2.The request code is scattered all over the place, which is not conducive to late componentization splitting

3.There are two formats for server-side Reponse data return

![img](https://confluence.toolsfdg.net/download/attachments/51977072/image2021-1-15_15-56-56.png?version=1&modificationDate=1610697417000&api=v2)![img](https://confluence.toolsfdg.net/download/attachments/51977072/image2021-1-15_15-58-31.png?version=1&modificationDate=1610697511000&api=v2)

## How

1.Using the Kotlin language, the BNCHTTPClient underlying library in the project is used to implement the API data request and encapsulation

![img](https://confluence.toolsfdg.net/download/attachments/51977072/image2021-1-15_16-40-1.png?version=1&modificationDate=1610700002000&api=v2)

2.In the public API development stage, the interface is classified and organized.Regulating rules: Divided by business modules. For example, the spot interface is structured into the SpotrepositoryImpl

3.Server-side Reponse data return provides the corresponding extension method.

![img](https://confluence.toolsfdg.net/download/attachments/51977072/image2021-1-15_16-34-36.png?version=1&modificationDate=1610699677000&api=v2)

The extension method returned by Figure 1Response

![img](https://confluence.toolsfdg.net/download/attachments/51977072/image2021-1-15_16-35-0.png?version=1&modificationDate=1610699700000&api=v2)

The extension method returned by Figure 2Response

## Gudline

1.**/api/v1/depth** :Currency of depth

Response:

![img](https://confluence.toolsfdg.net/download/attachments/51977072/image2021-1-15_15-56-56.png?version=1&modificationDate=1610697417000&api=v2)

Method:

![img](https://confluence.toolsfdg.net/download/attachments/51977072/image2021-1-15_15-54-53.png?version=1&modificationDate=1610697293000&api=v2)



2.**/api/v1/aggTrades**:The currency pair was recently traded

Response:

![img](https://confluence.toolsfdg.net/download/thumbnails/51977072/image2021-1-15_15-58-31.png?version=1&modificationDate=1610697511000&api=v2)

Method:

![img](https://confluence.toolsfdg.net/download/attachments/51977072/image2021-1-15_16-0-12.png?version=1&modificationDate=1610697612000&api=v2)

## Plan

### First  Phase

- Implement data request call of BNCHTTPClient
- Support flexible configuration of Response data
- Classified API request interface is regular (PS: in each phase)
- [0128 接口迁移](https://confluence.toolsfdg.net/pages/viewpage.action?spaceKey=FUT&title=Android+Tech-driven+items)

### Second Phase

- The Future Public API is partially migrated
- Spot ordering interface migration and sorting
- [0225 技术需求PRD](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=57923176)

### Third Phase

- The rest of the API migration for Future Public

### Four Phase

- Delivery Public API migration