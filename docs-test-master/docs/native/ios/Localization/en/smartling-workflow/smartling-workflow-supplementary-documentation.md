---
id: en-smartling-workflow-supplementary-documentation
title: Supplementary documentation
---

## About the files in the LanguageSources/en directory

| File name                                                  | note                                                         |
| :--------------------------------------------------------- | :----------------------------------------------------------- |
| ios-common.json                                            | If there is no special requirement, you can add new strings here |
| ios-c2c.json                                               | The source file used by the c2c team                         |
| ios-flutter.json                                           | source file used by flutter SDK                              |
| no-placeholder-formatting.json                             | The strings in this file will not be forced to format placeholders |
| no-translation.json                                        | This file is currently used to store strings that are hidden on the Crowdin system and no need translationThe strings in this file will not be translatedFor Strings that no need any language translation, please follow this chapter: "CMS vs TMS" |
| chinese-translation-only.json                              | This file is currently used to store strings that are hidden on the Crowdin system and only need simplified Chinese translationThe strings in this file will only be translated into simplified Chinese |
| finance-spot.json finance-margin.json finance-futures.json | Source file used by the Finance team                         |

## CMS vs TMS

**CMS：**Content management system

**TMS：**Translation management system

Smartling is TMS. All source files under our LanguageSources directory will upload Smartling. Please do not use TMS as CMS.

Therefore, for text that displays the same content in all language, for example: MACD, BOLL

These strings do not need to be uploaded in source files, just use "MACD" directly in the code.



If you really have strings that want to use our dynamic function, but do not need translation, you can put them in the no-translation.json file

## Bad cases about Localize library using

During our migration process, we found some bad cases, such as some verification code logic.

Because in Simplified Chinese / English, it is customary to use "s" instead of "seconds". Therefore, in the logic related to the verification code, "%ds remaining" is used. We expect its final display to be "10s remaining".

If you use the standard format of String, there will be no problem. But if you use the format of the Localize library (Localize library uses a single % as a placeholder), your source string will looks like this:

```
"%ds remaining"
```

This leads to two problems:

1. `%s` is one of the standard placeholders in iOS, Smartling will force conversion to `%1$s`
2. Even if "%s" is not converted, the translator does not know what `%s` means

In this case, you should:

1. Do not use any text that is the same as the iOS standard placeholder (If you need to use it anyway, such as `%Chg`, which can be put in no-placeholder-formatting.json)
2. About abbreviations:
   1. You can add a description in the note, such as "s means seconds"
   2. Inform PM and refuse it. For example, in English, you can use "%1$d seconds remaining" instead of "s"

## Documents strongly recommended

[String Format Specifiers](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Strings/Articles/formatSpecifiers.html)

[Creating a String Using Formats](https://developer.apple.com/documentation/swift/string)

[Smartling Json source file](https://help.smartling.com/hc/en-us/articles/360008000733)

