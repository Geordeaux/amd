---
id: zh-CN-localization-add-a-language-support
title: 如何新增一种语言
---

# 1

bot 每日会提新 PR（包含 TMS 系统最新的变更内容） 到我们的 [repo](https://git.toolsfdg.net/fe/ios-client) 的 **/Resources/Languages** 目录中，其中可能会包含新增的语言文件

如果我们需要新增这种语言，先确定语言文件是否链接到我们的工程中了。如果没有需要手动添加链接 （拖拽文件至 workspace）到我们的工程里

![](https://static.devfdg.net/static/mono-static/docs-ui/img/Localization-01.png)

# 2 

接着，需要在 dev apollo **ios.crowdinConfiguration** 字段中配置新增语言的 code（即 lang_{% code %}.json 中 code 部分）

![](https://static.devfdg.net/static/mono-static/docs-ui/img/Localization-02.png)

# 3 （可选）

- 若前端、后端分别有不同 Languages Code 映射，需要在 dev apollo **ios.languageMapping** 字段中配置映射

- 目前，Binance 已经统一了内部使用的语言代码的规则，iOS、android、web、backend 均可以相应统一语言代码

- 主站已上线、和即将上线的语言，可以参考[此文档](https://docs.google.com/spreadsheets/d/1zpSz2scY83E4v8oLoKAo558cY_1BqW1wzradAc7GMec/edit#gid=0)

![](https://static.devfdg.net/static/mono-static/docs-ui/img/Localization-03.png)

# 4 

在 `Tasks.swift` -> `setGlobalLocalizationWithoutNetwork` -> `initialSupportedLanguage` 添加新的语言 code
```swift
func setGlobalLocalizationWithoutNetwork() {
    let initialSupportedLanguages = ["ko", "en", "zh_Hans",
                                     "vi", "tr", "es",
                                     "ru", "pt_BR", "ja",
                                     "zh_Hant", "pl", "uk",
                                     "fr", "fil", "id",
                                     "nl", "th", "ro",
                                     "de", "es_EM", "it",
                                     "cs", "bg", "lv",
                                     "sv", "pt-PT", "bn"]
    LocalizationKit.default.configureGlobalLanguageWithoutNetwork(initialSupportedLanguages: initialSupportedLanguages)
}
```

  - 该步骤是为了更好的用户体验，如果用户系统语言是德语，当用户初次 App 会自动设置 App 为德语，以下是 key code：

  - ```swift
    var language: String = ""
    if let storedLanguage = UserDefaults.standard.string(forKey: localizeStorageKey) {
        language = storedLanguage
    } else if let preferredLanguage = Bundle.preferredLocalizations(from: initialSupportedLanguages).first {
        language = preferredLanguage
    }
    ```

- ⚠️RTL 语言暂时不要加到 initialSupportedLanguages，该部分 qingxu.chong@binance.com 正在优化

# 5（可选）

（可选）enum Language 中补充新的 Language case，并处理好相应的 switch 和 Language 枚举的 init 方法

![](https://static.devfdg.net/static/mono-static/docs-ui/img/Localization-04.png)

  - 该步骤是一些情况下方便做当前语言的判断，是可选步骤。e.g:

  - ```swift
    static var binanceAcademyP2P: URL? {
        switch LocalizationConverter.currentLanguage {
        case .chinese:
            return appendingWebView(path: "/cn/support/articles/360039384951")
        default:
            return appendingWebView(path: "/en/support/articles/360039384951")
        }
    }
    ```

## 6 工程设置

确保你的 Xcode -> Project -> Info -> Localizations 里添加了对目标语言的支持
