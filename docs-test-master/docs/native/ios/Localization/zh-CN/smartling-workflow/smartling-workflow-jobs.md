---
id: zh-CN-smartling-workflow-jobs
title: CICD jobs of localization
---

## 校验

- pull-ios-client-validate-i18n 
- pull-ios-cloud-futures-validate-i18n

### 描述

当 developer 创建 PR 给 `base branch` 时：

1. 校验是否修改了 localization files：本地化文件是不允许 developer 修改的，只能由 bot 每日从 Smartling 平台同步
2. 校验 Localization source files 的 json 正确
3. 校验 Localization source files 中是否存在重复的 key

|                                | Binance                               | Cloud                                              |
| ------------------------------ | ------------------------------------- | -------------------------------------------------- |
| base branch                    | bnbfly                                | X-CLOUD/bnbfly                                     |
| Localization files path        | /Binance/Resources/Languages/         | /Apps/X-Cloud/X-Cloud/Resources/Languages          |
| Localization source files path | /Binance/Resources/LanguageSources/en | /Apps/X-Cloud/X-Cloud/Resources/LanguageSources/en |



## Source files 上传 Smartling

- post-ios-cloud-futures-i18n-source-push
- post-ios-client-i18n-source-push 

### 描述

- 当 `目标分支` 的 Localization source files 发生变更后，自动触发此 job

- 此 job 将把 Localization source files 上传至 Smartling 平台，供翻译人员翻译

|          | Binance | Cloud          |
| -------- | ------- | -------------- |
| 目标分支 | bnbfly  | X-CLOUD/bnbfly |

## 生成器

- post-ios-client-i18n-generator
- post-ios-cloud-futures-i18n-generator

### 描述

- 由于 Smartling 平台不维护和生成 source language (English) 的最终文件，所以 `lang_en.json` 是由此 job 生成的

- 当 `目标分支` 的 Localization source files 发生变更后，自动触发此 job
- 此 job 会将所有的 Localization source files 整合成 `lang_en.json`

|          | Binance | Cloud          |
| -------- | ------- | -------------- |
| 目标分支 | bnbfly  | X-CLOUD/bnbfly |

## 发布 Dev

- post-ios-client-i18n-publish
- post-ios-cloud-futures-i18n-publish

### 描述

- 当 `目标分支` 的 Localization files 发生变更时，自动触发此 job

- 此 job 会将 Localization source files 发布到 Dev 环境的 CDN 中
- 此 job 最后会创建一个 mono 仓库的 PR，提供给之后发布 Product 环境使用

|                         | Binance                                                    | Cloud                                                        |
| ----------------------- | ---------------------------------------------------------- | ------------------------------------------------------------ |
| 目标分支                | bnbfly                                                     | X-CLOUD/bnbfly                                               |
| Localization files path | /Binance/Resources/Languages/                              | /Apps/X-Cloud/X-Cloud/Resources/Languages                    |
| Dev CDN file example    | https://static.devfdg.net/api/i18n/-/ios/lang_zh_Hans.json | https://cloudfutures-static.binancedev.com/cloud-futures/api/i18n/-/ios/lang_zh_Hans.json |

## 每日定时同步

- i18n-ios-periodics-sync
- i18n-ios-cloud-futures-periodics-sync

### 描述

- 此 job 每日 8:00 AM（GMT+8）中自动触发，也可以在 prow 系统手动触发
- 此 job 将会获取 Smartling 上最新的翻译内容，将变更创建一个 PR 到 `目标分支`

|               | Binance                                                      | Cloud                                                        |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 目标分支      | bnbfly                                                       | X-CLOUD/bnbfly                                               |
| Prow job link | https://prow.toolsfdg.net/?job=i18n-ios-periodics-sync       | https://prow.toolsfdg.net/?job=i18n-ios-cloud-futures-periodics-sync |
| PR name       | [Binance][i18n] chore(i18n): download translations from smartling | [Cloud][i18n] chore(i18n): download translations from smartling |
| PR Example    | https://git.toolsfdg.net/fe/ios-client/pull/12976            |                                                              |

## 发布 Product

- post-mono-deploy-prod-cn-ios-i18n
- post-mono-deploy-prod-ios-i18n
- post-mono-deploy-prod-ios-cloud-futures-i18n

### 描述

- 中 `发布 Dev 的 job` 中，会生成 mono 仓库的一个 PR
  - PR 中记录了在发布 Dev 环境时，保存在 CDN 上的 Localization files 的 zip 包的名称
- 当 上述 PR 被合并时，会触发此 job

-  会将 Dev CDN 上保存的 Localization source files 发布到 Product 环境的 CDN 中

|            | Binance                                          | Cloud                                                        |
| ---------- | ------------------------------------------------ | ------------------------------------------------------------ |
| PR name    | chore(release): update prod ios-i18n(ios-client) | chore(release): update prod ios-cloud-futures-i18n(ios-client) |
| PR Example | https://git.toolsfdg.net/mono/mono/pull/47645    | https://git.toolsfdg.net/mono/mono/pull/46074                |