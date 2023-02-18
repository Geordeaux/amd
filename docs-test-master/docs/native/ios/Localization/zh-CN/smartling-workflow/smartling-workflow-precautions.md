---
id: zh-CN-smartling-workflow-precautions
title: ⚠️国际化使用的注意事项
---

## 1 不要使用 Localize 三方库提供的占位符方案

1. 请使用 Swift 标准的 String format 方法
2. Localize 提供的占位符方案是不稳定且非通用的

## 2 使用更标准的占位符： “%1$@”

1. Source file 中，虽然可以写入 `%@`，但 Smartling 仍会将其转换为更标准的占位符： `%1$@`
2. `1$` 是 iOS 支持的位置标记，例如，当有两个占位符时:
   1. ![](https://static.devfdg.net/static/mono-static/docs-ui/img/smartling-workflow-08.png)



更多的系统 format 方案，请参考 [apple doc](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Strings/Articles/formatSpecifiers.html)

## 3 no-placeholder-formatting.json

no-placeholder-formatting.json 是特殊源文件，该源文件中的 string，将不会被强制性转换占位符

这个文件是为了保护一些特殊 string。例如 `%Chg` 是一个特殊用词，其中的 %C 并不是占位符。如果不将其加在 no-placeholder-formatting.json 中，Smarlting 系统将把 `%C` 识别成 iOS 系统中的占位符，更改为 `%1$Chg`

