---
id: en-localization-add-a-language-support
title: Add a language support
---

# 1

bot will submit new PRs (including the latest changes in the TMS system) daily to our [repo](https://git.toolsfdg.net/fe/ios-client) **/Resources/Languages** directory , Which may contain new language files

If we need to add this language, first, determine whether the language file is linked to our project. If there is no need to manually add a link (drag and drop the file to the workspace) to our project

![](https://static.devfdg.net/static/mono-static/docs-ui/img/Localization-01.png)

# 2

Next, you need to add the code of the new language in the dev apollo **ios.crowdinConfiguration** field (ie the code part in lang_{% code %}.json)

![](https://static.devfdg.net/static/mono-static/docs-ui/img/Localization-02.png)

# 3 (Optional)

- If the front-end and back-end have different Languages Code mappings, you need to configure the mapping in the dev apollo **ios.languageMapping** field
- Binance has unified the rules of language codes used internally, and iOS, android, web, and backend can all correspond to unified language codes.
- For Binance languages that are already online or about to go online, please read to [this document](https://docs.google.com/spreadsheets/d/1zpSz2scY83E4v8oLoKAo558cY_1BqW1wzradAc7GMec/edit#gid=0)

![](https://static.devfdg.net/static/mono-static/docs-ui/img/Localization-03.png)

# 4

Next, you need to add a new language code in `Tasks.swift` -> `setGlobalLocalizationWithoutNetwork` -> `initialSupportedLanguages`:

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

- This step is for a better user experience, e.g. If the user's system language is German, the App will automatically set the App to German for the first time. The following is the key code:

- ```swift
  var language: String = ""
  if let storedLanguage = UserDefaults.standard.string(forKey: localizeStorageKey) {
      language = storedLanguage
  } else if let preferredLanguage = Bundle.preferredLocalizations(from: initialSupportedLanguages).first {
      language = preferredLanguage
  }
  ```
  
- ⚠️RTL language should not added to initialSupportedLanguages, About this part, qingxu.chong@binance.com is being optimized

# 5 (Optional) 

Add a new Language case to `enum Language`, and handle the corresponding switch and init methods of Language enum

![](https://static.devfdg.net/static/mono-static/docs-ui/img/Localization-04.png)

  - This step is convenient for judging the current language in some cases, so it's optional. e.g:

```swift
static var binanceAcademyP2P: URL? {
    switch LocalizationConverter.currentLanguage {
    case .chinese:
        return appendingWebView(path: "/cn/support/articles/360039384951")
    default:
        return appendingWebView(path: "/en/support/articles/360039384951")
    }
}
```

## 6 Project settings

Make sure your Xcode -> Project -> Info -> Localizations has added support for the target language.
