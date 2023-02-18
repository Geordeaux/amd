---
id: update_in_app
title: Update In-App

hide_title: true
---
# Update In-App

### Contracts ：richard.cao@binance.com

### Background:

- Android users can receive update notification, then let them update app as soon as possible in order to increasing the rate of update, decreasing bugs and enhancing user experience.
- Original strategy was weak update, however it's not the same as Google play's principle.
ref: [https://play.google.com/intl/zh-CN_ALL/about/privacy-security-deception/malicious-behavior/](https://play.google.com/intl/zh-CN_ALL/about/privacy-security-deception/malicious-behavior/)
- There're some issues following code:
    1. Country could be changed.
    2. If the user is from china who downloads app from google play, It will update and download new app directly instead of download via google play
    3. AppChecker.isGooglePlayInstalled can't ensure whether here is google play within device.

    Therefore, in version 1.6.0.0 (3094), we refactored the update strategy based on the existing channel distribution logic, circumventing the Google Play in-app update terms, and can safely give users weak update prompts and guide users when there is a new version.

```kotlin
val county = SharedPreferencesManager.get().getSystemCountry()
when (county) {
    Locale.CHINA.country -> {
        downloadApk(downloadUrl, versionName)
    }
    else -> {
        if (AppChecker.isGooglePlayInstalled(this@MainActivity)) {
            //安装了 Google Play，则跳转 Google Play 更新
            jumpToGooglePlay()
        } else {
            //没有安装 自动下载 apk 文件
            downloadApk(downloadUrl, versionName)
        }
    }
}
```

### Strategy:

**How to determine whether a user has installed Google Play**

Based on the existing channel distribution, we have a channel dedicated to publishing to Google Play. The user who downloads this channel package must have installed Google Play (the special case of downloading the apk package on Google Play through a third-party channel is not This column). In this package, the isGooglePlayInstalled method can directly return true. On the contrary, in channel packages of non-Google Play channels, the isGooglePlayInstalled method can directly return false.

**How to guide users to update**

Every time the application is started, an update interface will be requested in MainActivity. When there is a new version, the interface will be updated. At this time, different strategies are adopted for packages of different channels:

- Google Play channel: direct users to jump to Google Play update
- Non-Google Play channels: directly download the latest version of the apk package

### Conclusion:

The UpgradeUtil code is newly added, and different implementations are made for different channels. MainActivity directly calls the upgradeApp method.

In version 1.6.0.0 (3094) and later versions, every time a new version is released, users can be prompted to update. The update behavior is a weak update, which can increase the user update rate, especially for users who are not on Google Play channels.