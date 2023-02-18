---
id: zh-CN-localization-manager-guidance
title: 管理者手册
---

## 周期与节点 

[Time points](https://docs.fe.devfdg.net/docs/native/ios/Localization/zh-CN/smartling-workflow/zh-CN-smartling-workflow-workflow-instructions#time-point)

## 日常管理内容

1. 定期检查所有的 job 是否正常运转 - [CICD jobs of localization](https://docs.fe.devfdg.net/docs/native/ios/Localization/zh-CN/smartling-workflow/zh-CN-smartling-workflow-jobs)
2. 每天 8:00 (GMT+8) 的定时任务（[i18n-ios-periodics-sync](https://prow.toolsfdg.net/?job=i18n-ios-periodics-sync) | [i18n-ios-cloud-futures-periodics-sync](https://prow.toolsfdg.net/?job=i18n-ios-cloud-futures-periodics-sync)），会将 smartling 的翻译内容通过 PR 形式同步给 repo，管理者可以每天检查 PR changes 是否合理（例如：[占位符的使用](https://docs.fe.devfdg.net/docs/native/ios/Localization/zh-CN/smartling-workflow/zh-CN-smartling-workflow-precautions)是否合理、是否在不合理的时间更改了 strings 的翻译），并帮助合并 PR
3. 在发布节点，将 mono 的 PR 发送给 QA，让 QA 测试并发布生产环境（目前我们在 app-i18n-cdn-publish-notification 群中来通知所有人）。管理者在 QA 合并 PR 后，需要简单的验证发布是成功的
4. 管理 apollo 配置，apollo 配置会涉及到：
   1. **语种支持：**`ios.crowdinConfiguration` -> `supportedLanguages / rtlSupportedLanguages`
   2. **CDN path:** `ios.crowdinConfiguration` -> `cnCDN / commonCDN`
   3. **语言名称展示（Optional）：** `ios.crowdinConfiguration` -> `languageDisplayNameMapping`
   4. **Language Code 映射：**` ios.languageMapping.2.26.0` -> `backendLanguageCodeMap / frontendLanguageCodeMap`
      1. iOS 在 2.26.0 之后，mapping 根据 binance 统一 language code 来配置
      2. 所有的语言和即将上线的语言 language code 均可以在[这里查看](https://docs.google.com/spreadsheets/d/1zpSz2scY83E4v8oLoKAo558cY_1BqW1wzradAc7GMec/edit#gid=0)

4. 新语言管理
   1. 在工程中（Binance 和 Cloud 均有）有 `localemap.json` 文件，该文件可以控制每种语言文件从 smartling 平台导出文件，和最终到我们的工程中的文件名（即 `Resources/Languages` 目录下的`lang_ko.json` 一系列文件），之间的映射关系
   2. 帮助 PM 和 QA 在 dev / product apollo 配置新的语言，参考 [https://docs.fe.devfdg.net/docs/native/ios/Localization/zh-CN/zh-CN-localization-add-a-language-support](https://docs.fe.devfdg.net/docs/native/ios/Localization/zh-CN/zh-CN-localization-add-a-language-support)

