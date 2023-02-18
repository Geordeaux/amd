---
id: en-localization-debug-tool
title: Localization Debug Tool
---

## Features

- [x] Multi-language support: Chinese and English
- [x] Usage guide
- [x] Dark mode support
- [x] Current localization basic information view
- [x] Apollo related data view
- [x] CDN information display and switching
- [x] Language Code View
- [x] Batch strings localization verification

## Entrance

DoraemonKit -> Localization

## Information panel

The information panel is the homepage of the Localization Debug Tool

![](https://static.devfdg.net/static/mono-static/docs-ui/img/localization-debug-tool-01.png)

The First part:

- Display current language information, current localization file reading source
- Click to enter `String Localization Testing`

The second part:

- Click to enter the localization-related Apollo configuration view

The third part:

- Show the currently requested CDN environment
- Click to switch CDN to request localization file again

The fourth part:

- Display the Language Code information of the current language

## Localization Apollo configuration

Click here to enter the localization-related Apollo configuration view

![](https://static.devfdg.net/static/mono-static/docs-ui/img/localization-debug-tool-02.png) -> 

![](https://static.devfdg.net/static/mono-static/docs-ui/img/localization-debug-tool-03.png)

## CDN environment

You can click `Switch CDN Environment`, select the CDN environment you want to request the localization file again

![](https://static.devfdg.net/static/mono-static/docs-ui/img/localization-debug-tool-04.png) ->

![](https://static.devfdg.net/static/mono-static/docs-ui/img/localization-debug-tool-05.png)

## Strings Localization Testing

Click here to enter `String Localization Testing`, which is used to check the correctness of strings localize in batches

![](https://static.devfdg.net/static/mono-static/docs-ui/img/localization-debug-tool-06.png)

1. In the input box, fill in all string.key, separated by carriage return
2. After completing the input, click the `Localize` button in the upper right corner
3. The following shows the value of string.key after executing `localized` in the current language
4. The `From Downloaded` means that the read strings are read from the file downloaded by the CDN. If is `From Bundle`, it means that it is read from the main bundle (this indicates that the CDN access may failed)

![](https://static.devfdg.net/static/mono-static/docs-ui/img/localization-debug-tool-07.png)
