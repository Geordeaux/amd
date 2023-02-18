---
id: android-build-package
title: Compile installation package
---

> How to compile installation package  

### Step 1. Open the link
https://git.toolsfdg.net/fe/bnb-android/pull/10844
(If this link fails, please contact dongming.zhang@binance.com through Webex)


### Step 2. Create package job

Commenting on the following in PR triggers the build, and listing the build APK and links in PR after the build.

debug
* `/publishw`          binance-v7a-debug
* `/publishw v7`       binance-v7a-debug
* `/publishw v8`       binance-v8a-debug
* `/publishw all`      binance-v7a-debug, binance-v8a-debug

release
* `/releasew`          binance-v7a-release
* `/releasew v7`       binance-v7a-release
* `/releasew v8`       binance-v8a-release
* `/releasew all`      binance-v7a-release, binance-v8a-release

After a while (About twenty minutes), it will be compiled
