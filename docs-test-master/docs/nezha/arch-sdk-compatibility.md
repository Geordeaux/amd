---
id: arch-sdk-compatibility
title: SDK Compatibility
sidebar_label: SDK compatibility
---

# Overview & Objectives


## SDK and mini-program capabilities

- SDK provides a set of APIs to help mini-programs to use certain functionalities of native side via [plugins](https://docs.fe.devfdg.net/docs/nezha/arch-overview#plugin). 
- Changes in these capabilities (e.g: new capabilities are added, existing capabilities are enhanced/optimized) will result in the changes in the SDK implementation.

**Objective:** SDK will be updated in cycles: A new version of the SDK will be published every 2 weeks.



## SDK ↔ Host app Compatibility

1. Because The APIs that a SDK provides relies on the actual capabilities of the host app,  each SDK version can only run on a corresponding version of  Host app. 
* Example: Assuming **Host 1.0.0** has a feature *Google login*, and **SDK 0.1.0** is implemented to support this feature. Then the next version **Host 2.0.0** support a new feature *Facebook login*, and the corresponding **SDK 0.2.0** is likewise implemented to support *Facebook login*. If we try to use  **SDK 0.2.0** for **Host 1.0.0**, it will cause compatibility issues because **Host 1.0.0** does not support *Facebook login* yet.

2. Some times when there are issues with a SDK version, we will need to deliver a patch version, ost app should be able to update to the new version without having to upgrade to a new version via AppStore/Google Play.  

**Objective:**
- We will need to properly manage the versions of the SDK and their corresponding host app versions.

- When there is a SDK patch version, host app should be able to access this patch without having to update via AppStore/GooglePlay

  

## SDK ↔ Mini-program compatibility

Ideally when there are a new SDK version, mini-program developers are required to adapt to the changes. Usually mini-program developers will have access to the new SDK in advance in order to update their mini-programs in a timely manner.  
However there could be the situation that mini-program developers doesn't want (or are not yet able) to update their apps, in such case we should allow the possiblity to let a mini-program to run on a lower version of the SDK.

**Objective:**
- Mini-program developers should be able to access the new SDK version before it is officially released on production
- Mini-programs should be able to specify the version of SDK that they needs to run on.



# SDK Release cycle

## Semantic versioning

In order to maintain a consistent relationship between SDK and host app versions. We will use [Semantic Versioning](https://semver.org/) for SDK versioning, and define a mapping rule between Host app and its corresponding SDK versions.
 * Update of the primary (major) and secondary (minor) version numbers of SDK depends on the version of host app. In short, the major or minor number of SDK can only be increased when Host app version number(s) is increased accordingly. For example: **SDK 0.1.0** can run on **Host 1.0.0**, but **SDK 0.2.0** will require **Host 1.1.0**
 * Update of the tertiary (patch) version number of SDK does not depend on the version of host app. In short, a host app version can support a range of patches. For example: **Host 1.0.0** can run SDK versions ranging from **0.1.0** to **0.1.x** (x >= 0)



## Release cycles

In order to avoid the unknown impact of a new SDK version on the online Mini-programs, Host app will always be shipped with the **latest stable SDK** version (the SDK resources are packaged within the bundle of the Host app).

After a new Host app version is released, the development of a new SDK version will start. During the development process, SDK team will constantly release `dev` SDK versions to mini-programs developer via `preview channel` so that mini-program developers can adapt to changes in time. 
* Example: When **Host 1.0.0** (shipped with **SDK 0.1.3** - the stable version) is released on production. **SDK 0.2.0-dev.x** (intended to be shipped with **Host 1.1.0**) will be developed based on **SDK 0.1.3**. As **SDK 0.2.0-dev.x** is being developed, mini-program will have access to this SDK version via `preview channel`



# Native's SDK management mechanism

## Definitions

### Active SDK

An `active SDK` is a SDK version to be installed in the Host app, and will be used to run mini-program under normal circumstances. Host app will keep **one and only one** `active SDK` at any time. if there is a new SDK version available for Host app, Host app can install this new version via **Background Update** mechanism and this new version will become the `active SDK` in place of the older one. 

* Example: **Host 1.0.0** currently has **SDK 0.1.0** as it's `active SDK`, when **SDK 0.1.1** is available, **Host** **1.0.0** can download and install **SDK 0.1.1**, from here on **SDK 0.1.1** will become the `active SDK` and **SDK 0.1.0** will be deemed obsolete and be removed later. 

  

### Default SDK

When a new Host app is released on production, a **stable SDK version** (namely sdk.zip) will be shipped within the Host app's bundle - This SDK version is called `default SDK`. When Host app version is launched, native will check for an installed `active SDK` version, if there is no `active SDK`, host app will install `default SDK` and use it as the `active SDK`.


## Multiple SDK mechanism

### Overview

In order to manage the SDK effectively, we use Multiple sdk strategy the workflow is described in this 
[Multiple SDK Diagram](https://www.processon.com/view/link/60d071451e085301d0b71bcc) . In general, there are 2 main workflows:

* **Background update**: To ensure that the `active SDK` always has the latest version
* **SDK Loading**: The details of how SDK will be loaded under 3 scenarios:
  1. When the mini-program requires a specified SDK version to run. 
  2. When the mini-program requires to be run on `preview`  channel.
  3. When the mini-program is run on `published` channel, and it doesn't require any specific SDK version



### Technical details

#### Background Update

Diagram: 

<img src="https://static.devfdg.net/static/docs-ui/nezha/resources/arch-sdk-compatibility/background-update.png" width="800"></img>

When Host app is launched, native will make an API request to [BE API](https://confluence.toolsfdg.net/pages/viewpage.action?spaceKey=FEG&title=MP+SDK+API) (see **Senario 3: Upgrade LatestVersion**) to check if there is any new SDK version available. If a new version is available, native will download this version and install it, this new version will become the `active SDK` and the previous `active SDK` will be marked to be deleted when at next Host app launch.

#### SDK Loading

To open a mini-program, a SDK is required for the mini-program to run. Depending on how the mini-program is opened, the SDK will be loaded differently. The following will describe how SDK will be loaded (according to priority)

1. When mini-program requires a specific SDK to run, regardless of `active SDK`. This is usually applied when the mini-program strictly rely on that SDK version (e.g: MP developer haven't adapt to the newer SDK yet). 
Native will check with BE via [BE API](https://confluence.toolsfdg.net/pages/viewpage.action?spaceKey=FEG&title=MP+SDK+API) (see **Senario 1: Request Version**) for that specified SDK version, if that version is available, native will download and install it, then use it to open the mini-program. 

<img src="https://static.devfdg.net/static/docs-ui/nezha/resources/arch-sdk-compatibility/specified-sdk-loading.jpg" width="800"></img>

2. When mini-program needs to be run on `preview` channel. This is usually applied for `grayscale release` period of a new SDK version. MP developer can also use `preview` channel. If there are any problem during this period, we will be able to rollback the SDK. 

If mini-program is opened on `preview` channel, native will check with BE via [BE API](https://confluence.toolsfdg.net/pages/viewpage.action?spaceKey=FEG&title=MP+SDK+API) (see **Senario 3: Upgrade LatestVersion**) for the `grayscale SDK version`. If grayscale version is available, native will download and install this version (see **Appendix 1: SDK installation workflow**) and use it to open the mini-program. 
**Note**: If the response from BE API has *updateDefaultSDK==true*, the grayscale will be set as a new `active SDK` in place of the current one. 

<img src="https://static.devfdg.net/static/docs-ui/nezha/resources/arch-sdk-compatibility/preview-sdk-loading.jpg" width="800"></img>


3. When the mini-program is run on `published` channel. This is the most common case when end-user opens mini-program on production app. 
If `active SDK` satisfies the minimum SDK requirement of mini-program. Native will use `active SDK` to open mini-program . Otherwise native will check with BE for newer SDK. If a newer version is available, native will download and install this version and use it to open the mini-program. 
**Note**: If the response from BE API has *updateDefaultSDK==true*, the grayscale will be set as a new `active SDK` in place of the current one. 

<img src="https://static.devfdg.net/static/docs-ui/nezha/resources/arch-sdk-compatibility/active-sdk-loading.jpg" width="800"></img>

#### Appendix 1: SDK installation workflow

When native needs to install a new SDK version, the steps are as follow: 
1. Native download the package `.zip` file via provided url from [BE API](https://confluence.toolsfdg.net/pages/viewpage.action?spaceKey=FEG&title=MP+SDK+API) response
2. After downloading, native will check for the signature of the package (also provided from the API response)
3. If the signature is valid, native will unzip the package contenst to `../frameworks/${version}/` directory inside `/Cache` directory of the file system. 
4. SDK is ready to use. 

<img src="https://static.devfdg.net/static/docs-ui/nezha/resources/arch-sdk-compatibility/appendixone-sdk-install.jpg" width="800"></img>

## References

[WeChat documents](https://developers.weixin.qq.com/miniprogram/en/dev/framework/client-lib/)
[SDK API](https://confluence.toolsfdg.net/pages/viewpage.action?spaceKey=FEG&title=MP+SDK+API)
