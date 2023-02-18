---
id: zh-CN-smartling-workflow-workflow-instructions
title: 使用流程
---

## Source files 格式

https://help.smartling.com/hc/en-us/articles/360008000733

## Source 目录结构

![](https://static.devfdg.net/static/mono-static/docs-ui/img/smartling-workflow-01.png)

1. Binance/Resources/LanguageSources：目录下文件为 Smartling 源文件和配置文件
2. Binance/Resources/Language：目录下文件为自动化生成，不允许 Developer 更改

## 权责分配

### Source strings

- 增删改全部由 Developer 管理
- 在 ios-client 项目下管理

### 中文翻译

- 在 Smartling 系统，由 PM 和 Localization 团队管理
- Developer 目前无需感知

### 截图管理

- 与中文翻译的管理相同

## 提交申请

按照国际化组的要求，所有的翻译需求都需要提交表单，请 Developers 更改 source string 后，请联系 PM 提交翻译需求表单

## 时间节点与重要信息

e.g. 以主站规则为例

- 主站每 3 周一个 app 版本
- 假设图中 23 日是当前版本的发版日
- **5日 10:00～9日 20:00**： 发版日前 2 周为 strings 新增/改动的截止时间
- **12日～16日 20:00：**Localization team 翻译的时间
- **19日～23日 10:00**：发布 CDN 的周期，由于该时间节点，Localization team 承诺的翻译已经完成，该时间段内可以不断发布新内容到 product CDN
- 默认情况下，管理者会在发版日前一周和 Code freeze 当天，通过 QA 同学各发布一次 product CDN



![](https://static.devfdg.net/static/mono-static/docs-ui/img/smartling-workflow-time-point.png)

**一件重要的事：**

- 我们的翻译系统有一个规则：如果 source string(english) 被改动，则所有语种对应的翻译，将被重置为英文
- **所以如果你不能确保 strings 在当前版本能被翻译完成，就不要改动 source string**
- 例如，现在是 0729 版本，请在7月29日之后，再添加 0819 的改动，否则，这些改动将会跟随 0729 CDN 发布，被带入到 prod 环境

## Workflow of developers

### 新增 source string

1. 基于 bnbfly 新开分支

2. 更改 Source files（Resources/LanguageSources/en/xxx.json），添加你需要的 strings

3. 提交 PR **给 bnbfly branch** ([Demo PR](https://git.toolsfdg.net/fe/ios-client/pull/6384))

4. 合并 PR

5. 合并 PR 后会触发一系列的自动化 Job，可以通过 github 的

    

   commit 列表

    查看

   1. ![](https://static.devfdg.net/static/mono-static/docs-ui/img/smartling-workflow-03.png)
   2. 第一个 Job - **post-ios-client-i18n-source-push -** 将 changes上传 Smartling 
   3. 第二个 Job - **post-ios-client-i18n-generator -** 通过 changes 生成新的 lang_en.json
   4. **post-ios-client-i18n-generator** 执行结束后，会自动给 bnbfly 提交一个 commit（*commit message: generate from LanguageSources*），将新生成的 lang_en.json 推到 bnbfly 上
   5. ![](https://static.devfdg.net/static/mono-static/docs-ui/img/smartling-workflow-04.png)
   6. 这个 commit 会继续自动触发一个新 Job：**post-ios-client-i18n-publish**
   7. **post-ios-client-i18n-publish** 和以往一样，是发布 dev CDN 的 job，这个 job 成功后，就可以正常使用新增的 en strings 了

### 获取/同步翻译

每天 8 点中会自动执行一个 Prow Job - [i18n-ios-periodics-sync](https://prow.toolsfdg.net/?job=i18n-ios-periodics-sync)，你也可以在 [Prow Job 面板](https://prow.toolsfdg.net/?job=i18n-ios-periodics-sync)手动 rerun 这个 Job 的方式来进行同步

该 Job 会同步 Smartling 最新的内容，并**给 bnbfly 分支**提交 PR（[Demo PR](https://git.toolsfdg.net/fe/ios-client/pull/6139)）

![](https://static.devfdg.net/static/mono-static/docs-ui/img/smartling-workflow-05.png)

在合并这个 PR 后：

1. 将会自动触发 Dev CDN 的发布
2. 将会自动为 mono repo 一个 PR（该 PR 由 QA approve 和 merge，merge 后将会发布变更到 product CDN）

### 修改 source string

修改和新增的流程基本相同。

不同的是，

1. 若修改了 source string 的 value，Smartling 系统会清空该 string 所有语言的翻译
2. 更改了 key 的内容 = 删除一个 source string + 增加一个 source string（[Demo PR 1](https://git.toolsfdg.net/fe/ios-client/pull/6152) ｜ [Demo PR 2](https://git.toolsfdg.net/fe/ios-client/pull/6154)）



如果更新了 source string 的 value，翻译人员只会按照默认的 ETA（根据版本计划表） 来完成翻译，如果有紧急需要，让 PM 给 Localization 团队提交紧急翻译表单

### 参考时序图

![](https://static.devfdg.net/static/mono-static/docs-ui/img/smartling-workflow-06.png)
