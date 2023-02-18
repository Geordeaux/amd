---
id: en-smartling-workflow-precautions
title: ⚠️Precautions
---

## 1 Do not use the placeholder scheme provided by the Localize third party library

1. Please use Swift standard String format method
2. The placeholder scheme provided by [Localize](https://git.toolsfdg.net/fe/Localize-iOS) is unstable and non-universal

## 2 Use more standard placeholder: "%1$@"

1. In the source files, although `%@` can be written, Smartling will still convert it to a more standard placeholder: `%1$@`
2. `1$` is a position marker supported by iOS, for example, when there are two placeholders:
   1. ![](https://static.devfdg.net/static/mono-static/docs-ui/img/smartling-workflow-08.png)



To learn more about format and placeholders please check [apple doc](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Strings/Articles/formatSpecifiers.html)

## 3 no-placeholder-formatting.json

no-placeholder-formatting.json is a special source file, the string in this source file will not be forced to convert the placeholder.

This file is to protect some special strings. For example, `%Chg` is a special word, where %C is not a placeholder. If you don’t add it to no-placeholder-formatting.json, the Smarlting system will recognize %C as a placeholder and change it to `%1$Chg`.

