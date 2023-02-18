---
id: release_process
title: Release process

hide_title: true
---


# Release process

### Contracts ：shaojun@binance.com

### Currently released application market

1. Google Play
2. Huawei (overseas)
3. Transsion (currently not being updated)
4. Official website

### Release process

The regression package is packaged with the developer branch

When the Code Freeze period is reached, the code of the developer branch needs to be merged into the pre-release. Provide the official version of qa for testing.

Code merging process
The developer's code is merged into the prerelase branch, and the jenkins script under jenkins will be found from this branch.

During Code Freeze to gray release, if new code is submitted, you need to go through the PR application process and merge the code into the hotfix branch. The hotfix pr is provided to pre-release and dev, and packaged on pre-release.

If the version is released urgently (a minor version), at this time dev already has the next version of the code, first merge to the hotfix branch, and then merge to the pre-release branch after the test is passed, and pr is provided to pre-release and dev respectively. Then package from pre-release.

The pre-release after the release is merged with the master, and then tagged.

### Submit crowdin to CDN

Go to mono's PR search---------android-i18n

[https://git.toolsfdg.net/mono/mono/pulls](https://git.toolsfdg.net/mono/mono/pulls)

![img](https://confluence.toolsfdg.net/download/attachments/27875534/image2020-11-23_16-57-4.png?version=1&modificationDate=1606121800000&api=v2)

Click in to copy the PR address, 

such as [https://git.toolsfdg.net/mono/mono/pull/10800](https://git.toolsfdg.net/mono/mono/pull/10800)

![img](https://confluence.toolsfdg.net/download/attachments/27875534/image2020-11-23_16-58-4.png?version=1&modificationDate=1606121860000&api=v2)

QA can ask [caifeng.li@binance.com](mailto:caifeng.li@binance.com) to help publish

![img](https://confluence.toolsfdg.net/download/attachments/27875534/image2020-11-23_17-3-17.png?version=1&modificationDate=1606122172000&api=v2)

### Packaging process

1. Jenkins
    - [https://cicdjenkins.toolsfdg.net/job/fe/job/bnb-android/job/pre-release/](https://cicdjenkins.toolsfdg.net/job/fe/job/bnb-android/job/pre-release/)
    - 

    ![img](https://confluence.toolsfdg.net/download/attachments/27875534/Jietu20200707-200240@2x.jpg?version=1&modificationDate=1594123390000&api=v2)

    Reinforcement: Reinforcement must be checked in the release market

2. Github/Tekton
    - The developer's code is merged into the prerelase branch, and a pre-release is proposed to the master branch. 
    Merge after this PR is released. All packaging operations are completed in this PR.

    ![img](https://confluence.toolsfdg.net/download/attachments/27875534/image2020-10-10_11-43-30.png?version=1&modificationDate=1602301393000&api=v2)

    Comment on /package play or /package binance in PR for packaging (both are reinforcement packages)

    After the packaging is successful, the corresponding mapping file and apk package will be as follows:

    ![img](https://confluence.toolsfdg.net/download/attachments/27875534/image2020-12-28_13-6-39.png?version=1&modificationDate=1609131975000&api=v2)

    When releasing the regression package or releasing the Google market, only one channel is selected when packaging the GooglePlay channel, and reinforcement is selected, so that the packaging speed is fast.
    Flavor: Choose different channel packages: official website, Google Play, and Transsion.

    Remember to check the reinforcement package, build and download it.

    Release process
    After the master station is packaged, the downloaded zip is decompressed, and the directory structure inside is as follows:

    ![img](https://confluence.toolsfdg.net/download/attachments/27875534/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202020-02-10%20%E4%B8%8B%E5%8D%884.34.09.png?version=1&modificationDate=1581328109000&api=v2)

    The four directories correspond to the official website and gp installation package and the official website and gp mapping files respectively.

    Release to Google Play([https://play.google.com/apps/publish/?account=6050695873821566432#AppListPlace](https://play.google.com/apps/publish/?account=6050695873821566432#AppListPlace))

    ![img](https://confluence.toolsfdg.net/download/attachments/27875534/Jietu20200707-200617.jpg?version=2&modificationDate=1594123766000&api=v2)

    Select the application shown in the figure, other applications have been abandoned.

    ![img](https://confluence.toolsfdg.net/download/attachments/27875534/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202020-02-10%20%E4%B8%8B%E5%8D%884.17.37.png?version=1&modificationDate=1581328109000&api=v2)

    Select application version

    ![img](https://confluence.toolsfdg.net/download/attachments/27875534/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202020-02-10%20%E4%B8%8B%E5%8D%884.17.43.png?version=1&modificationDate=1581328109000&api=v2)

    Click Manage on the right

    ![img](https://confluence.toolsfdg.net/download/attachments/27875534/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202020-02-10%20%E4%B8%8B%E5%8D%884.18.31.png?version=1&modificationDate=1581328109000&api=v2)

    Click Create Version

    ![img](https://confluence.toolsfdg.net/download/attachments/27875534/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202020-02-10%20%E4%B8%8B%E5%8D%884.18.43.png?version=1&modificationDate=1581328109000&api=v2)

    Drag the encrypted apk in the play directory with "encrypted".

    ![img](https://confluence.toolsfdg.net/download/attachments/27875534/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202020-02-10%20%E4%B8%8B%E5%8D%884.18.49.png?version=1&modificationDate=1581328109000&api=v2)

    Fill in the release note (usually just fill in English). save.

    Then update the release phase.

    Release mapping

    ![img](https://confluence.toolsfdg.net/download/attachments/27875534/image2021-4-16_18-2-21.png?version=1&modificationDate=1618567341000&api=v2)

    Click the anti-obfuscation file, find the corresponding versioncode, and upload it. 

    Remember, the gp file in the mapping directory should be uploaded to the two versions. 

    The version code of the two apks differ by 1000, for example (armeabi: 3153 and arm64: 4153).

    **Gray rule**
    Generally, the gray scale starts at 3%, then 5%, 10%, 20%, 50%, 100%. According to the situation, the full amount is the same as ios within a week. 

    Generally increase the gray level once a day, Usually 5% of the time, there will be some new versions of crashes on firebase. 

    If there are many problems, you need to pause the grayscale and assign the corresponding bu solution (ie hotfix) according to the page where the problem is located. 

    Upgrade the versionCode after all are resolved, and then restart Package and release, repeat the above process.

    Generally, there will be more than 3 hotfixes, 5% and 10% are the easiest to find online problems, so after the hotfix, you still need to start the grayscale from 3%.

    Publish the official website (pay attention to many regions, such as China)

    1. Jenkins way:
    Generally, after the full Google play, you will find Damon Mao (Infra-Tech), but remember to send him the v7a package (compatibility package).Or enter jenkins [https://cicdjenkins.toolsfdg.net/job/fe/job/release/job/android-release/](https://cicdjenkins.toolsfdg.net/job/fe/job/release/job/android-release/)

    Fill in the serial number packaged by jenkins and start building.

    Note: After the construction starts, you need to click the construction progress bar and click the yes button.

    ![img](https://confluence.toolsfdg.net/download/attachments/27875534/image2020-7-9_17-34-20.png?version=1&modificationDate=1594287261000&api=v2)

    ![img](https://confluence.toolsfdg.net/download/thumbnails/27875534/image2020-7-9_17-35-39.png?version=1&modificationDate=1594287340000&api=v2)

    **After entering the channel number, click Build With parameters and then click the place pointed by the red arrow, a dialog appears, select yes.**

    ![img](https://confluence.toolsfdg.net/download/attachments/27875534/11_45_59%281%29.jpg?version=1&modificationDate=1594784789000&api=v2)

    2. Tekton way

    Branch a new branch (master-publishcdn) from the master branch, create a /tekton-configs/sync-binance-cdn-apk.txt file, and fill in the s3 download address of the binance channel package (must be the encrypted_binance of armeabi-v7a) .apk package, otherwise the task will fail), as follows:

    ![img](https://confluence.toolsfdg.net/download/attachments/27875534/image2020-12-28_13-8-15.png?version=1&modificationDate=1609132070000&api=v2)

    commit the content, initiate a PR

    Comment in PR /uploadCDNAndRefresh

    CI will synchronize the apk to the cdn on the official website.

    After the task is completed, you can close the PR, do not merge the PR

    1. Notify PM to update the background prompt language configuration [chen.fan@binance.com](mailto:chen.fan@binance.com)
    2. Notify background colleagues to update the version number [mingming.sheng@binance.com](mailto:mingming.sheng@binance.com)

    Release TRANSSION
    Generally, after the full amount of Google play, find Roven He (MKT-Preformance marketing associate) to give the v7a reinforcement package

    ![img](https://confluence.toolsfdg.net/download/attachments/27875534/02_18_54.jpg?version=1&modificationDate=1593757326000&api=v2)

    After the full amount, please contact [chen.fan@binance.com](mailto:chen.fan@binance.com) ((PM-Product)) as soon as possible to inform you that you can prompt to upgrade. There will be configuration, update prompts and whether to force the update in Chenchen. Test the style of the prompt dialog box several times and whether the copy is accurate.
    After the configuration is done in the morning and morning, the driver who leaves the car after installing an old version, clear the application cache, enter the app, and check the prompt update dialog box

    Release market channel package
    Portal: [Market Channel Package Process](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=57911105)