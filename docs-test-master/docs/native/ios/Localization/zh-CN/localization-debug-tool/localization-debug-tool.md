---
id: zh-CN-localization-debug-tool
title: Localization 调试工具
---

## 功能

- [x] 中英双语显示
- [x] 使用引导
- [x] 暗黑模式支持
- [x] 当前本地化基本信息查看
- [x] Apollo 相关数据查看
- [x] CDN 信息与切换
- [x] Language Code 查看
- [x] 批量 string Localize 校验

## 入口

哆啦A梦 -> Localization

## 信息面板

信息面板是 Localization Debug Tool 的首页

![](https://static.devfdg.net/static/mono-static/docs-ui/img/localization-debug-tool-01.png)

第一部分:

- 展示当前语言信息、当前本地化文件读取来源
- 点击可以进入 `String Localization Testing`

第二部分：

- 点击可以进入本地化相关的 Apollo 配置阅览

第三部分：

- 展示当前所请求的 CDN 环境
- 点击可以切换 CDN 重新请求本地化文件

第四部分：

- 展示当前语言的 Language Code 信息

## Localization Apollo 配置

点击这里，可以进入本地化相关的 Apollo 配置阅览

![](https://static.devfdg.net/static/mono-static/docs-ui/img/localization-debug-tool-02.png) -> 

![](https://static.devfdg.net/static/mono-static/docs-ui/img/localization-debug-tool-03.png)

## CDN 环境

可以点击 `切换 CDN 环境`，选择你想要重新请求本地化文件的 CDN 环境

![](https://static.devfdg.net/static/mono-static/docs-ui/img/localization-debug-tool-04.png) ->

![](https://static.devfdg.net/static/mono-static/docs-ui/img/localization-debug-tool-05.png)

## Strings Localization Testing

点击这里，可以进入 `String Localization Testing`，用于批量检测 strings localize 的正确性

![](https://static.devfdg.net/static/mono-static/docs-ui/img/localization-debug-tool-06.png)

1. 在输入框中，填入所有的 string.key，以回车分割
2. 输入完成，点击右上角 `Localize` 按钮
3. 下方展示『当前语言下』string.key 执行 `localized` 后的值
4. 中间的 `From Downloaded` 表示读取 strings 是从 CDN 下载的文件中读取，若显示 `From Bundle` 则表示从 main bundle 读取（这提示 CDN 访问可能失败）

![](https://static.devfdg.net/static/mono-static/docs-ui/img/localization-debug-tool-07.png)
