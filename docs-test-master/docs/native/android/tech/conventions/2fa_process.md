---
id: new_2fa_process
title: New 2FA process
hide_title: true
---

# New 2FA process

### Contracts ：klay.wei@binance.com

In version 1.23.0, due to security, the previous 2FA process was modified. 

When 2FA was performed in the previous version, only one method was required to verify. 

If two are bound, the verification method can be switched. As shown below:

![img](https://confluence.toolsfdg.net/download/thumbnails/33119717/image2020-5-1_9-51-38.png?version=1&modificationDate=1588297898000&api=v2)

After clicking Submit, the selected verification method authType and verification code code will be returned to the previous interface in the form of activityResult, and the 2FA verification interface will be closed. 

Therefore, if the verification code expires or is wrong, it is inconvenient to modify and cannot be restored. Said not very friendly.

### New Verification Process

The new version of the process will pull the verification configuration from the backend according to the event scenario bizScene (see the bottom note) and flowId. 

The returned data model is as follows:

The page displays the verification method according to the data in the needCheckVerifyList, and the verification methods given are not optional but all need to be verified. 

Therefore, the previous interface for calling 2FA data has changed.

The new communication method requires the caller to register for the broadcast:

registersBroadCast(BroadCastConstants.NOTIFY_TWOFA)

![img](https://confluence.toolsfdg.net/download/attachments/33119717/image2020-5-1_13-17-30.png?version=1&modificationDate=1588310251000&api=v2)

Note that : 
It is necessary to determine whether the type is the currently needed scene Code, which is similar to the resultCode in onActivityResult:

![img](https://confluence.toolsfdg.net/download/attachments/33119717/image2020-5-22_15-43-36.png?version=1&modificationDate=1590133416000&api=v2)

### 1. If the user clicks back and exits the 2FA page.

2FA will send a cancellation broadcast, as follows:

The caller can perform some rollback processing after receiving the data

![img](https://confluence.toolsfdg.net/download/attachments/33119717/image2020-5-1_13-17-47.png?version=1&modificationDate=1588310268000&api=v2)

### 2. If there is a way to verify, the user fills in the verification code and clicks the submit button.

2FA will return the filled data to the caller in the form of broadcast, as follows:

```kotlin
private fun authComplete() {
        // 通过本地广播传送表单数据到调用方
        val resultIntent = Intent(BroadCastConstants.NOTIFY_TWOFA)
        resultIntent.putExtra(BundleConstants.CODE, 1)
        resultIntent.putExtra(BundleConstants.DATA, TwoFaResult(
                mViewModel.email, emailEt.text?.toString(), gauthEt.text?.toString(), mViewModel.mobile, mViewModel.mobileCode, smsEt.text?.toString()
        ))
        broadCast(resultIntent)
    }
```

TwoFaResult represents the form data of the entire 2FA page, the model is as follows:

![img](https://confluence.toolsfdg.net/download/attachments/33119717/image2020-5-1_11-55-10.png?version=1&modificationDate=1588305311000&api=v2)

The caller can take the required fields and put them in the interface for use.

![img](https://confluence.toolsfdg.net/download/attachments/33119717/image2020-5-1_11-58-23.png?version=1&modificationDate=1588305504000&api=v2)

If you need to call the loading box or return error information to the 2FA page when requesting the network, you can handle it by operating TwoFaDataBlock, which is essentially a LiveData and will be observed on the 2FA page.

![img](https://confluence.toolsfdg.net/download/attachments/33119717/image2020-5-1_12-2-7.png?version=1&modificationDate=1588305728000&api=v2)

The caller uses the 2FA form data to make an interface request:

![img](https://confluence.toolsfdg.net/download/attachments/33119717/image2020-5-1_12-4-9.png?version=1&modificationDate=1588305850000&api=v2)

### 3. If the 2FA page load configuration shows that no 2 verifications are required.

Then it will also enter the above process, but the data in the TwoFaResult passed to the caller are all empty characters: "".

### 4. Jump to 2FA page

```kotlin
Router.Builder().path(ROUTE_TWOFA_V2)
                .withSerializable(BundleConstants.DATA, TwoFaType.WITHDRAW_WHITE_SWITCH)
                .navigation(this)
 
 
//TwoFaType见 https://docs.google.com/spreadsheets/d/1u-2ZG_cwKvZzNG-QWX_takfvE_dOLnZ0jEbEv0ixxcs/edit#gid=287291240
```

### Current TwoFaType:

![img](https://confluence.toolsfdg.net/download/attachments/33119717/image2020-5-22_16-43-51.png?version=1&modificationDate=1590137031000&api=v2)

###