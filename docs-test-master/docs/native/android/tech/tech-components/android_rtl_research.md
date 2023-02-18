---
id: android_rtl_research
title: Android RTL Investigation

hide_title: true
---

# Android RTL Investigation

## Current Situation

The Android system supports RTL layout. Turn on the supportsRtl option in the project, and you can see the current app’s adaptation to the RTL layout.
The 5 tabs of the homepage, the market details page, and the personal center are sampled and observed. Behave as follows:

### Main Page
The overall structure is normal, and some controls need to be adjusted:
1. Carousel control modification
2. Head novice guide page modification
3. Professional version item needs to be modified

![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_12-19-49.png?version=1&modificationDate=1595305190000&api=v2)
![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_14-12-24.png?version=1&modificationDate=1595311945000&api=v2)

### Market Page
The whole is normal, the details need to be adjusted:
1. Change value
2. Animation effect of tab switching (ViewPager switching animation)
3. Guidance prompt box display

![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_14-9-42.png?version=1&modificationDate=1595311783000&api=v2)
![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_14-10-26.png?version=1&modificationDate=1595311827000&api=v2)


### Trade Page
The whole is normal, the details need to be adjusted:

![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_14-16-33.png?version=1&modificationDate=1595312194000&api=v2)
![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_14-16-59.png?version=1&modificationDate=1595312220000&api=v2)
![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_14-17-18.png?version=1&modificationDate=1595312239000&api=v2)

### Futures Page
1. The perpetual page is generally normal, but the control on the left is abnormal and needs to be modified
2. The delivery page is similar to perpetual
3. The options page is relatively normal, and the details need to be modified
4. The RTL function of the line chart needs to be investigated

![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_14-37-42.png?version=1&modificationDate=1595313463000&api=v2)
![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_14-39-28.png?version=1&modificationDate=1595313569000&api=v2)
![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_14-53-15.png?version=1&modificationDate=1595314396000&api=v2)

### Fund Page
The whole is normal, the details need to be adjusted:

![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_14-42-50.png?version=1&modificationDate=1595313771000&api=v2)

### Mine Page
1. The overall entry page is normal, and the details need to be adjusted
2. Some sub-abnormal pages need to be confirmed one by one

![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_14-44-27.png?version=1&modificationDate=1595313868000&api=v2)
![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_14-44-43.png?version=1&modificationDate=1595313884000&api=v2)
![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_14-44-57.png?version=1&modificationDate=1595313898000&api=v2)

### Market Detail Page
The market graph cannot be adapted, and other controls need to be investigated in detail.

![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_14-57-17.png?version=1&modificationDate=1595314638000&api=v2)
![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_14-57-35.png?version=1&modificationDate=1595314656000&api=v2)

## About time estimation

- All page UI needs to be evaluated. If the current RTL effect does not meet expectations, the page may need to be redesigned.
- Troubleshoot by page and check out the icon resources that need to be flipped (dark night and day mode).
- Animation effects need to be adapted
- The RTL controls supported by the system require RTL support. (TextView, EditText, etc., compatibility issues may occur on different phones, need to be covered)
- Some controls do not support RTL and may need to be redesigned from the control implementation. There is a lot of uncertainty here. Take time to sort out
- The whole experience is received and the effect is fine-tuned.

## Amendment

1. Use Start/End instead of Left/Right

![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_15-13-58.png?version=1&modificationDate=1595315639000&api=v2)

2. Add RTL resource folders (pictures, layouts, animations and other resources)

![img](https://confluence.toolsfdg.net/download/attachments/42441610/image2020-7-21_15-17-9.png?version=1&modificationDate=1595315830000&api=v2)

3. Add RTL support to the control (especially the adaptation of EditText, TextView and other commonly used controls)
4. Look for adaptations or even alternatives for controls that do not support RTL (such as ViewPager)
5. RTL adaptation of animation effects
6. It is recommended to start with relatively simple modules, which can lay the foundation for the adaptation of complex pages (such as my page)

### Contacts

- [seal.w@binance.com](mailto:seal.w@binance.com)