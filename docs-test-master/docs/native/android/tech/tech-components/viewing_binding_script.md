---
id: view_binding_script
title: Use script to replace Kotlin Android Extension with View Binding
hide_title: true
---

# Context

The kotlin-android-extension plugin has been deprecated, and the official gives the [replacement scheme](https://developer.android.com/topic/libraries/view-binding/migration) of View Binding. We also met another problem during the component splitting, that the kotlin synthetic properties [cannot support crossing module access](https://youtrack.jetbrains.com/issue/KT-22430#focus=Comments-27-3684849.0-0). And the problem will occur at compile-time instead of code-time. This problem will have a great impact on us; especially we are doing component splitting. However, there are many boring and tedious works during the replacement, so I developed a script to mitigate the load.


Share Video Link : https://binance.webex.com/recordingservice/sites/binance/recording/playback/a7363819c28c44758d75d923723de09c

Video Password : Kcxn2HhD

## How to use

1. Please Clone [Scripts](https://git.toolsfdg.net/fe/Scripts) to your local machine. (Please contact Jerome Ran if you don't have the permission.)
2. Please cd to the directory where viewbind.py is located.
3. Please run python3 viewbind.py {your module path}.
4. Resync the project and check the changes. (I'll show you how to check the changes later.)

The module path is the directory where your module build.gradle.kts is located. If you have multiple modules sharing some layouts, you should replace them together, use a comma to separate multiple module paths.

## What the script did

- Modify your build.gradle.kts automatically, including:
    - Remove the kotlin-android-extension plugin.
    - Open view binding.
    - Add the kotlin-parcelize plugin.
    - Add view binding property delegate library.
- Modify imports on class files, including:
    - Remove kotlin synthetic properties imports.
    - Add binding class imports.
    - Replace the new parcelize import.
- Initialize the variables of binding classes, including the dealing with include and merge labels. (Use view binding property delegate library)
- Replace variables in kotlin synthetic property style with view binding class variables.

The whole process is like the following:

![ViewBinding Replacement Process](https://confluence.toolsfdg.net/download/attachments/66436610/viewbinding.png)

## How to check the changes

This script cannot free your hands in 100%. The script does not conveniently handle some special cases, so we must have an all-around check.

At first, open local changes in Android Studio (Command + 9):

![Command + 9](https://confluence.toolsfdg.net/download/attachments/66436610/%E6%88%AA%E5%B1%8F2021-04-12%20%E4%B8%8A%E5%8D%8811.03.35.png)

Second, you only need to pay attention to kt classes.

### Extension functions on view

![Extention Functions](https://confluence.toolsfdg.net/download/attachments/66436610/%E6%88%AA%E5%B1%8F2021-04-12%20%E4%B8%8A%E5%8D%8810.59.59.png)

The gone and visible are extension functions on the view. But the historyTipView is a binding class variable actually, so you need to change it to historyTipView.root.

### The binding class variable is initialized in the wrong place

![Binding Class](https://confluence.toolsfdg.net/download/attachments/66436610/%E6%88%AA%E5%B1%8F2021-04-12%20%E4%B8%8B%E5%8D%883.59.26.png)

There is an inner class PosHistoryViewHolder in class PosHistoryFragment, and the variable itemPosHistoryBinding should be initialized in PosHistoryViewHolder class, so you need to move it.

![Inner Class](https://confluence.toolsfdg.net/download/attachments/66436610/%E6%88%AA%E5%B1%8F2021-04-12%20%E4%B8%8A%E5%8D%8811.07.43.png)

This is another case.

### PopupWindow doesn't support view binding property delegate in current

![PopupWindow](https://confluence.toolsfdg.net/download/attachments/66436610/%E6%88%AA%E5%B1%8F2021-04-12%20%E4%B8%8A%E5%8D%8811.01.09.png)

The view binding property delegate doesn't support PopupWindow in current, so you need to change back to the original way like the following:

```kotlin
private var itemFutureExchangeDecimalBinding: ItemFutureExchangeDecimalBinding? = null
 
itemFutureExchangeDecimalBinding = ItemFutureExchangeDecimalBinding.inflate(LayoutInflater.from(context), null, false)
// or you can use bind method
// itemFutureExchangeDecimalBinding = ItemFutureExchangeDecimalBinding.bind(view)
 
// release the binding class on dismiss callback
itemFutureExchangeDecimalBinding = null
```

### The view binding property delegate library has a bug on DialogFragment
There is a [bug](https://github.com/kirich1409/ViewBindingPropertyDelegate/issues/55) on DialogFragment, so you need to abandon it and change back to the original way, just like PopupWindow.