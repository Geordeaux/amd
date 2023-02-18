---
id: config-global
title: Global Config
sidebar_label: Global Config
---

## Configs

| Property  | Type     | Default Value | Required | Description                                                                     |
| --------- | -------- | ------------- | -------- | ------------------------------------------------------------------------------- |
| entryPage | `string` | -             | ✓        | index page name                                                                 |
| window    | `Object` | -             | -        | Global default window behavior                                                  |
| tabBar    | `Object` | -             | -        | Behavior of the tab bar at the bottom of the page.                              |
| page      | `Object` | -             | -        | Page configuration                                                              |
| devtool   | `Object` | -             | -        | Development environment configuration, only available in the `preview` channel. |

#### window

| Property                     | Type       | Default Value | Required | Description                                                          |
| ---------------------------- | ---------- | ------------- | -------- | -------------------------------------------------------------------- |
| navigationBarBackgroundColor | `HexColor` | #000000       | -        | The navigation bar background color.                                 |
| navigationBarButtonColor     | `HexColor` | #707A8A       | -        | The button color in the navigation.                                  |
| navigationBarTitleText       | `string`   | -             | ✓        | Title text of the navigation bar.                                    |
| navigationBarTextStyle       | `string`   | white         | -        | The title color of the navigation bar, only supports black or white. |
| backgroundTextStyle          | `string`   | dark          | -        | The pull-down loading style, only dark and light are supported.      |
| backgroundColor              | `HexColor` | #ffffff       | -        | The background color of the window.                                  |
| enablePullDownRefresh        | `bool`     | false         | -        | Enables pull-down-to-refresh control on the app page.                |

#### tabBar

If the Mini Program is a multi-tab app (which uses a tab bar at the bottom or top of the app window to switch between tabs), you can use the tabBar configuration item to specify the behavior of the tab bar and the pages displayed when the user switches between tabs.

| Property        | Type       | Default Value | Required | Description                                                                       |
| --------------- | ---------- | ------------- | -------- | --------------------------------------------------------------------------------- |
| color           | `HexColor` |               | ✓        | The default color of the tab bar text, only supports hex color codes.             |
| selectedColor   | `HexColor` |               | ✓        | The color of the tab bar text when being selected, only supports hex color codes. |
| backgroundColor | `HexColor` |               |          | The color of the tab bar background, only supports hex color codes.               |
| borderStyle     | `string`   | `"black"`     | ✓        | The border color of the tab bar, only supports `black` and `white`.               |
| list            | `Array`    |               | ✓        | The tab list (see the list property description), supports 2 to 5 tabs.           |
| position        | `string`   | `"bottom"`    |          | The tabBar position, only supports bottom and top.                                |
| custom          | `boolean`  | `false`       |          | A custom tabBar                                                                   |

The `list` property accepts an array, which can only configure 2 to 5 tabs. The tabs are listed in the order of the array. Each item is an object with the following properties:

| Property                                                | Type     | Default Value | Required | Description                                                                                                                                          |
| ------------------------------------------------------- | -------- | ------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| pagePath                                                | `string` |               | ✓        | The page path, which must be defined first in pages.                                                                                                 |
| text                                                    | `string` |               | ✓        | The text of tab buttons.                                                                                                                             |
| iconPath                                                | `string` |               |          | The image path. The size of the icon is limited to 40 KB, suggested dimensions: 81px by 81px, online images are not supported.                       |
| When position is set to top, the icon is not displayed. |
| selectedIconPath                                        | `string` |               |          | The image path for the selected icon. The size of the icon is limited to 40 KB, suggested dimensions: 81px by 81px, online images are not supported. |
| When position is set to top, the icon is not displayed. |

#### page

> In the page confirmation, you can only set the corresponding window configuration items to determine the window behavior of this page.

#### devtool

| Property        | Type       | Default Value | Required | Description                     |
| --------------- | ---------- | ------------- | -------- | ------------------------------- |
| debug           | `string`   | -             | -        | Enable debug mode               |
| domainWhitelist | `string[]` | -             | -        | Add extra white list for debug. |

#### Example

```
{
  "entryPage": "pages/index2",
  "pages": [
    "pages/index2",
    "pages/success"
  ],
  "window": {
    "backgroundTextStyle": "light",
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "WeChat",
    "navigationBarTextStyle": "black"
  },
  "page": {
    "pages/index2": {
      "navigationBarTitleText": "首页 Test",
      "usingComponents": {
        "comp": "../comp"
      }
    },
    "pages/success": {
      "usingComponents": {
        "comp": "../comp"
      }
    }
  }
}
```
