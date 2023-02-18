---
id: android-emulator
title: Android Emulator
---
> How to use the Android Emulator  
> How to debug web page with Android Emulator

### Step 1. Download & Install Android Studio

https://developer.android.com/studio.


### Step 2. Create the Android Emulator

- Open the Android Studio.

![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/emulator/001.png)

- Click the **Configure** and select the **AVD Manager**

![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/emulator/002.png)


- Click the **Create Virtual Device** button.

![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/emulator/003.png)


- Choose a device definition(Pixel 4 is recommended), and click the **Next** button.

![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/emulator/004.png)


- Select a system image(Android 10 is recommended), and click the **Next** button.

  > If the corresponding version has not been downloaded, you need to click the **Download** button in the selected version to download.

![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/emulator/011.jpg)


- You can set a name for the emulator, and then click **Finish** button.

![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/emulator/006.png)


- Select the virtual device and click the launch button in **Actions**.

![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/emulator/007.png)


- Start the Android emulator.

![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/emulator/008.png)

- If your emulator can not access network, please read this article: 

 https://stackoverflow.com/questions/42736038/android-emulator-not-able-to-access-the-internet

### Step 3. Configure the `adb` in environment variables.

```
export ANDROID_HOME=/Users/xxx/sdk
export PATH=${PATH}:${ANDROID_HOME}/tools
export PATH=${PATH}:${ANDROID_HOME}/platform-tools
```

> You can click the "Configuration" on the Android Studio startup page, and then select **SDK Manager** to enter the Android SDK page. On the page, **Android SDK Location** is the Android SDK file path (ie ANDROID_HOME). If the SDK version displayed on the current page is relatively old, you can choose the latest version of **SDK Platform** to download (Android 10 is recommended).



### Step 4. Install apk

- After starting the emulator, execute the command in the terminal to install the apk.

```groovy
adb install /Users/xxxx/xxx/binance.apk
```

> **How to get the apk for debug？**
>
> You can tell android developer ask for apk, or open **App Host** https://app.toolsfdg.net/apps/4/plats/13 to download(select  the tab of dev).



### Step 5. Debug with Chrome

- Start App and open a web page,  then open chrome and input `chrome://inspect/#devices`.

![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/emulator/009.png)


- Click the **inspect** and enter the DevTools page.

![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/emulator/010.png)


**Enjoy it.**
