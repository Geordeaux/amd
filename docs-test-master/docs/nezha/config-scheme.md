---
id: config-scheme
title: Schema
sidebar_label: Schema
---

#### Schema
Use different schema headers for different style:

• mp/web

• mp/app

#### Query

appId={appId}&startPagePath={base64URL(string)}&rev={bundleMD5}&channel={enum}

| Name  | Type  | Desc |
|---|---|---|
|  appId | string  |  app id |
|  data | string  |  data sent to the worker directly|
| rev | string | revision, used to download the specified version of the App during debugging| 
| channel| int | channel |
| startPagePath| string | first page path(urlencoded) |
| startPageQuery| string | first page query data(urlencoded) |

channel: 
1. 真机调试：仅限开发者使用
2. 体验版本：对应一个appId； 体验者<=60（白名单）；path；
3. 审核版本：审核白名单可以打开
4. 正式版本：通过appId打开
5. 分享DeepLink

#### Navigation bar style:

| schema| loading page| home page | other page |
| :----: | :----: | :----: | :----: | 
| /mp/web|  back | back| back|
| /mp/app| close| close | close & back| 

#### Loading page
When starting the Mini Program, first display the loading page, request the detail API, download resources and load resources in the loading page

#### Animation
All pages are from right to left

