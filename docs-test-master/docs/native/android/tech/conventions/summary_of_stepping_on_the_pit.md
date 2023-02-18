---
id: summary_of_stepping_on_the_pit
title: Summary of stepping on the pit
hide_title: true
---

# Summary of stepping on the pit

Contracts ：ruotong.zhou@binance.com

### Question:

1. Init SelectPop 

    ```kotlin

    marginPositionList.apply {
                   ......
                   add(getString(R.string.margin_cross_margin))
                   add(getString(R.string.margin_isolated_margin))
                   ......
               }

    ```

2. onItemSelect

    ```kotlin
    if (mTvMarginMode?.text != item) {
                        mTvMarginMode?.text = item
                        when (item) {
                            appContext.getString(R.string.margin_cross_margin) -> {
                               ......
                            }
                            appContext.getString(R.string.margin_isolated_margin) -> {
                              .....
                            }
                        }
                    .....
                    }

    ```

The difference between getting string from the above two code blocks is as follows:

`getString(R.string.margin_cross_margin)`

`appContext.getString(R.string.margin_cross_margin)`

Due to the inconsistency of the context of the obtained String, when the system language and App language are inconsistent, the String obtained from the same Key is ultimately inconsistent.

Caused the conditional judgment statement error in SelectPop onItemSelect.

The complete code is as follows:

```kotlin
private fun showMarginPositionPopup(context: Context) {
        if (selectPositionPopup == null) {
            marginPositionList.apply {
                clear()
                add(getString(R.string.margin_cross_margin))
                add(getString(R.string.margin_isolated_margin))
            }
            selectPositionPopup = SelectPop(context, marginPositionList).apply {
                textSize = dimen(R.dimen.text_size_14)
                mTvMarginMode?.measuredWidth?.let { width = it }
                setBackgroundResource(R.drawable.bg_select_pop_no_border)
                selectPosition = MarginSymbolManager.instance.getPositionIndex()
            }
 
            selectPositionPopup?.onItemSelect = { position ->
                val item = marginPositionList.getOrNull(position)
                selectPositionPopup?.selectPosition = position
                if (mTvMarginMode?.text != item) {
                    mTvMarginMode?.text = item
                    when (item) {
                        appContext.getString(R.string.margin_cross_margin) -> {
                            MarginSymbolManager.instance.setPositionType(MarginSymbolManager.MARGIN_CROSS)
                            SharedPreferencesManager.get().setLastModifyMarginPosition(MarginSymbolManager.MARGIN_CROSS)
                            refreshUIAfterPositionTypeChanged(false)
                        }
                        appContext.getString(R.string.margin_isolated_margin) -> {
                            MarginSymbolManager.instance.setPositionType(MarginSymbolManager.MARGIN_ISOLATED)
                            SharedPreferencesManager.get().setLastModifyMarginPosition(MarginSymbolManager.MARGIN_ISOLATED)
                            refreshUIAfterPositionTypeChanged(true)
                        }
                    }
                    openOrderFragment?.doSwitchMarginPositionMode()
                    findMarginExchangeFragment()?.resetOrderType()
                }
            }
            selectPositionPopup?.setOnDismissListener {
                mIvMarginMode?.rotateDown()
            }
        }
        mTvMarginMode?.let {
            selectPositionPopup?.showAsDropDown(it)
        }
        mIvMarginMode?.rotateUp()
    }
```

Contracts ：ruotong.zhou@binance.com