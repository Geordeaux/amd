---
id: android_git_codeowners
title: Android-Git-CODEOWNERS

hide_title: true
---

# Android-Git-CODEOWNERS

Contracts ：zhen.lou@binance.com

[Github official guidelines](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/about-code-owners)

### Code permissions principle:

The permissions principle

1. The owner is as detailed as possible to the individual, the principle of minimum.
2. The number of owners for each rule must be greater than 2, which can be individual + team (number of team> 2) or team (number of team> 2) or individual + individual.

Note that : CODEOWNERS files are matched in order from top to bottom, and only the last matched owner will take effect. Therefore, for matching rules with overlapping parts, please carefully evaluate the impact.

### Groups

Currently, there're 3 groups

|Group |Description                                                               |Group name                                |
|------|--------------------------------------------------------------------------|------------------------------------------|
|A-Team|It's biggest team, permissions should be "write"                          |Android                                   |
|B-Team|The permissions in every single team should be "Write "                   |Android-com Finance C2C Fiat Architecture |
|C-Team|Committee(It's group of BU TL and seldom senior members) should be "Admin"|Android-captain                           |


### Configuration about Git branch

1. Corresponding to the project setting-Collaborators & teams, each team has been added, currently a total of 6 (5 BU team + 1 committee team), and the permissions have been changed.
2. Corresponding project setting-Branches If you need to configure owner permissions, select add rule.Select these configurations as shown below.

![https://confluence.toolsfdg.net/download/attachments/38206990/image2020-7-8_22-54-49.png?version=1&modificationDate=1594220089000&api=v2](https://confluence.toolsfdg.net/download/attachments/38206990/image2020-7-8_22-54-49.png?version=1&modificationDate=1594220089000&api=v2)

## bnb-android engineering:

app-belongs to C-team by default
activities directory: split from BU TL to individual or B-team
Other class file directories:
--- If it is an independent module, it belongs to the corresponding individual
--- If it is a shared module, it belongs to B-team (architecture team & main site team)
--- Under the root directory (such as App.kt), it belongs to B-team (architecture team & main site team)
layout & drawable & string & others: attributable to A-team
gradle & manifest & .gitignore: belongs to C-team

Each module-xxx: belongs to B-team, detailed to individual later
Each lib-xxx:
--- If it is an independent module, similar to lib-hybrid, the relevant own classmate will be responsible for it
--- If it is a shared module, similar to lib-base, it belongs to B-team (architecture team & main site team)

CODEOWNERS file permissions: C-team is responsible

|Module|Desc                                                                      |Belong                                    |
|------|--------------------------------------------------------------------------|------------------------------------------|
|app   |All                                                                   |ALL                                       |
|lib-base|All                                                                       |ALL                                       |
|lib-data|All                                                                   |ALL                                       |
|lib-hybrid|hybrid container andplugin                                                           |.com                                      |
|lib-idcard| identification                                                                      |.com                                      |
|lib-liveness|Live detection                                                                      |.com                                      |
|lib-modules|Componentized service related                                                                   |Fiat                                      |
|lib-zeus|Risk collecting data                                                                |.com                                      |
|module-c2c|c2c BU                                                                  |c2c                                       |
|module-convert|Fiat BU                                                                    |Fiat                                      |
|module-launcher|.com BU                                                                    |.com                                      |
|module-ocbs|Fiat BU                                                                    |Fiat                                      |
|module-market|Finance BU                                                                    |Finance                                   |


### Other engineering:

|Repo  |Desc                                                                        |Belong                                        |
|------|--------------------------------------------------------------------------|------------------------------------------|
|https://git.toolsfdg.net/fe/android-hydrogen|tools and widget                                                             |Arch                                      |
|https://git.toolsfdg.net/fe/android-method-monitor|Method monitoring                                                                      |Arch                                      |
|https://git.toolsfdg.net/fe/android-device-fingerprint|                                                                          |Fiat                                      |
|https://git.toolsfdg.net/fe/android-elasticexecutor|Thread Pool                                                                       |Arch                                      |
|https://git.toolsfdg.net/fe/Skylinef|k-line flutter version                                                              |Fiat                                      |
|https://git.toolsfdg.net/fe/binance-push|push notification                                                                        |.com                                      |
|https://git.toolsfdg.net/fe/android-opensdk|android-opensdk                                                                   |Arch                                      |
|https://git.toolsfdg.net/fe/payment-sdk|payment-sdk                                                                     |Fiat                                      |
|https://git.toolsfdg.net/fe/android-aquarius|android-aquarius                                                                       |Arch                                      |
|https://git.toolsfdg.net/fe/android-crowdin|android-crowdin                                                                    |Arch                                      |
|https://git.toolsfdg.net/fe/Heimerdinger|performance monitor                                                                      |Arch                                      |
|https://git.toolsfdg.net/fe/Sylas-android-binding|dart to aar, deprecated                                                          |Fiat                                      |
|https://git.toolsfdg.net/fe/Sylas-core|flutter relatived, deprecated                                                            |Fiat                                      |
|https://git.toolsfdg.net/fe/Android-DataCentral|Data Central                                                                      |none                                        |
|https://git.toolsfdg.net/fe/android-DoraemonKit|Doraemon Kit                                                                    |Arch                                      |
|https://git.toolsfdg.net/fe/Ashe|Network probe                                                                     |Arch                                      |
|https://git.toolsfdg.net/fe/android-skyLine|k-line kotlin version                                                               |Finance                                   |
|https://git.toolsfdg.net/fe/DataCentral-QBC|DataCentral QBC                                                                 |none                                        |
|HappyWss|websocket                                                                 |Finance                                   |



|Module|Desc                                                                      |Belong                                    |
|------|--------------------------------------------------------------------------|------------------------------------------|
|https://git.toolsfdg.net/fe/fiat-app|U.S. app                                                                  |fiat                                      |


### Risk point：

It's working on modularization, then the working directory has changed.

### Template：

```markdown
#
# FE Android bnb-android project CODEOWNERS file.
#
##########################################################################
# tips:

# Each line is a file pattern followed by one or more owners.
# The last matching pattern takes the most precedence.

##########################################################################
##########################################################################
# These owners will be the default owners for everything in the repo

* @fe/C-team

##########################################################################
##########################################################################
# The most important files and dirs to be reviewed.

/CODEOWNERS @fe/C-team
/.gitignore @fe/C-team
*.gradle @fe/C-team
AndroidManifest.xml @fe/C-team

##########################################################################
##########################################################################
# Resource files (include layout, string, drawable, etc).

/app/src/main/res/ @fe/android

##########################################################################
##########################################################################
# .com business team.

/module-launcher/ @fe/android-com-team
/app/src/main/java/com/binance/dev/activities/main/ @fe/android-com-team
/app/src/main/java/com/binance/dev/activities/main/account/ @zhen-lou @klay-wei
/app/src/main/java/com/binance/dev/activities/redpacket/ @song-yi @zhen-lou

##########################################################################
##########################################################################
# Finance business team.

##########################################################################
##########################################################################
# Fiat business team.

/module-convert/ @fe/android-fiat-team
/module-ocbs/ @fe/android-fiat-team

##########################################################################
##########################################################################
# c2c business team.

/module-c2c/ @fe/android-c2c-team

##########################################################################
##########################################################################
# architecture team.

##########################################################################
##########################################################################
# The common libraries or modules.

/lib-base/ @fe/android-arch-team @fe/android-com-team
/lib-data/ @fe/android-arch-team @fe/android-com-team
/lib-liveness/ @zhifeng-li @fe/android-com-team
/lib-hybrid/ @zhenqiao-li 
/lib-idcard/ @zhifeng-li @fe/android-com-team
/lib-modules/ @zhenqiao-li
/app/src/debug/ @fe/android
/app/src/main/java/com/binance/dev/base/ @fe/android-arch-team @fe/android-com-team
```

### Refer to the bnb-android directory structure:

ps: xx%-represents the proportion of code engineering, roughly estimated

![img](https://confluence.toolsfdg.net/download/attachments/38206990/image2020-7-8_22-44-20.png?version=1&modificationDate=1594219460000&api=v2)