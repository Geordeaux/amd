---
id: internationalization
title: 多國語言開發
---

## 多國語言翻譯時程

請參考文件 [Translation Process (App - Smartling)](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=56307637)，並留意兩個時間點：

* 須在發布前兩週完成錄入多國語言
* 發布前一週不可再錄入多國語言，須在 `finance APP 國際化` 群裡，找 POC Elly 協助

## 多國語言開發流程

1. 在文件 [App文案 for Finance](https://docs.google.com/spreadsheets/d/1W6Sy7hZxJiLxcbrnCP7fWvJyHTtAyKhh75uHIaySmvw/edit) 上找到當期的 sheet，並錄入簡中、繁中及英文語言文案，以及對應的 iOS keys
2. 在最新的 `bnbfly` 分支上新增分支
3. 在 Binance 專案上，找到合適的多國文件 (.json)，並錄入英文文案與 keys
   * 資料夾位置：Binance --> Resources --> LanguageSource --> en
   * 常用檔案為 finance-*.json (e.g. finance-spot.json)，若為跨模組的常用文案，則錄入 ios-common.json 檔案中
4. 發布 PR，並確保在審核後完成 PR 合併

## 最佳實例

請參考文件 [i18n Best Practices](https://confluence.toolsfdg.net/display/LW/i18n+Best+Practices)

## Instructions/guides

- [如何新增一种语言](../Localization/zh-CN/zh-CN-localization-add-a-language-support)

- [Localization 调试工具](../Localization/zh-CN/localization-debug-tool/zh-CN-localization-debug-tool)

- [Submit a Translation Request (JIRA)](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=51993431)（Please submit your request at least two working days in advance）

- [Submit a Localization Bug](https://confluence.toolsfdg.net/display/LW/Submit+a+Localization+Bug)（Except Source English）

- [How to input Simplified Chinese](https://confluence.toolsfdg.net/display/LW/How+to+input+Simplified+Chinese) (Translating + proofreading)

- [Updating and maintaining translations in Smartling (for PMs and linguists)](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=57930541)（PMs are allowed to maintain the Simplified Chinese only）

## 其他資訊

- [Meet the Localization team](https://confluence.toolsfdg.net/display/LW/Meet+the+Localization+team)
- [BUs and Project Ownership](https://confluence.toolsfdg.net/display/LW/BUs+and+Project+Ownership)
- [Localization Country/Language Support](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=51993366)

## 緊急需求

若有緊急需求並聯繫不上 POC，可在 Webex 上請求支援機器人 `LocalizationSupportBot` 協助，我們將會盡快解決您的問題。
