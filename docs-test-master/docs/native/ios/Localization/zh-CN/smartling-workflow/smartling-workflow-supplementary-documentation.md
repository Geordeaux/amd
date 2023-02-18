---
id: zh-CN-smartling-workflow-supplementary-documentation
title: 补充文档
---

## 关于 LanguageSources/en 目录下的文件

| File name                                                  | note                                                         |
| :--------------------------------------------------------- | :----------------------------------------------------------- |
| ios-common.json                                            | 如果没有特殊需求，可以将新文案加在这里                       |
| ios-c2c.json                                               | c2c 团队使用的 source file                                   |
| ios-flutter.json                                           | flutter SDK 使用的 source file                               |
| no-placeholder-formatting.json                             | 此文件中的 strings 不会被强制格式化 placeholder              |
| no-translation.json                                        | 该文件目前是为了存放 Crowdin 系统上被 hidden 且不需要翻译的 strings此文件中的 strings 不会被翻译对于不需要任何语言翻译的 String，请遵循此篇章：**《关于 CMS 和 TMS 的问题》** |
| chinese-translation-only.json                              | 该文件是为了存放 Crowdin 系统上被 hidden 且仅需要简体中文翻译的 strings此文件中的 strings 只会有简体中文会被的翻译 |
| finance-spot.json finance-margin.json finance-futures.json | Finance 团队使用的 source file                               |

## 关于 CMS 和 TMS 的问题

**CMS：**Content management system

**TMS：**Translation management system

Smartling 是一个 TMS，我们的 LanguageSources 目录下的 source files 都会上传 Smartling。请不要将 TMS 当作 CMS 使用。

所以语言下都显示相同内容的文案，例如：MACD，BOLL

这些文案其实不需要放到 source files 上传，在代码中直接使用 “MACD”即可。



如果真的有需要，想借助我们整个动态流程达到更改线上文案的需求，但又不需要翻译，可以放到 no-translation.json 文件中

该文件的内容，翻译人员不会进行翻译

## 关于 Localize 三方库的不良案例补充

在我们迁移过程中发现一些不良 case，例如一些验证码逻辑。

因为在简体中文中，有习惯用 "s" 代表 "seconds"。所以在验证码相关的逻辑中，用到了：“剩余%ds”，我们期望它的最终展示是“剩余10s”。

如果使用 String 的标准 format 是不会出现问题。但是如果使用了 Localize 库的 format（Localize 使用单个 % 作为占位符），你的 source string 是这样的：

```
“剩余%s”
```

这样导致了两个问题：

1. `%s` 是 iOS 的标准占位符之一，Smartling 会强制转换成 `%1$s`
2. 即使 `%s` 没有被强制转换，翻译人员也不知道是 `%s` 什么意思

对于这种情况，请做到：

1. 不要使用任何与 iOS 标准占位符相同的文案（无论如何都需要使用的，如 `%Chg`，可以放到 no-placeholder-formatting.json 中）
2. 对于缩写
   1. 可以在 note 加上说明，比如 "s 代表 seconds"
   2. 告知 PM 相关问题，并拒绝缩写，比如这里，中文可以填写“剩余 %1$d 秒”，而不是用 s

## 强烈建议阅读的文档

[String Format Specifiers](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Strings/Articles/formatSpecifiers.html)

[Creating a String Using Formats](https://developer.apple.com/documentation/swift/string)

[Smartling Json source file](https://help.smartling.com/hc/en-us/articles/360008000733)

