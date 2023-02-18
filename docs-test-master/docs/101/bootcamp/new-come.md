---
id: new-come
title: Web FE 入职指南
---


## 常用网站

* [okta](https://binance.okta.com)
* [Github](https://git.toolsfdg.net/)
* [APP HOST](https://app.toolsfdg.net/)
* [测试工具](https://qcenter.k8s.qa1fdg.net/)
* [前端组件库](https://libs.fe.devfdg.net/uikit-core/getting-started/)
* [shuvi](https://shuvijs.github.io/)
* [日志管理系统](https://kiali.devfdg.net/)
* [小程序管理平台](https://developers.qa1fdg.net/en/mpp)

## 新人速查文档
* [内部开发工具指南](/docs/101/bootcamp/tools/)
* [内部权限开通指南](/docs/101/bootcamp/permission/)
## first day todo list
第一天,请完成以下任务
### 1. 权限申请
- [ ]  [Jira 项目管理工具权限申请 ](/docs/101/bootcamp/permission/#jira-项目管理工具)
- [ ]  [Jira 添加Exchange(EX)项目](/docs/101/bootcamp/permission/#exchangeex项目)
- [ ]  [Jira 添加COM 项目](/docs/101/bootcamp/permission/#com-项目)
- [ ]  [注册 GitHub Enterprise 账号](/docs/101/bootcamp/permission/#注册github-enterprise)
- [ ]  [GitHub Enterprise 加入fe organization](/docs/101/bootcamp/permission/#github-enterprise-加入-organization)
- [ ]  [GitHub Enterprise 加入 mono organization](/docs/101/bootcamp/permission/#github-enterprise-加入-organization)
- [ ]  [figma 加入组织](/docs/101/bootcamp/permission/#figma)

### 2. 本地开发环境配置

公司电脑不允许随便安装应用.如果需要安装,请在该网站进行确认.

[[MacOS]允许軟件类别 与 禁用软件类别](https://docs.google.com/spreadsheets/d/1X2GUeCFFyDFxUy1x5osgv86m-3NYWAV6vjBuiSa37zk/edit#gid=1345848083)


- [ ]  [完成本地Git或者客户端(Sourcetree)账号配置](/docs/101/bootcamp/tools/#关于git-cli以及git-管理客户端鉴权问题)
- [ ]  [安装必要的开发软件](/docs/101/bootcamp/tools/#开发工具)
- [ ]  [认识开发,测试,灰度,生产环境](/docs/101/bootcamp/tools/#环境)
- [ ]  [通过Header插件切换不同环境](/docs/101/bootcamp/tools/#feheader-浏览器插件访问指定环境)
- [ ]  [注册你的测试账号](/docs/101/bootcamp/tools/#使用测试系统创建测试账号)
- [ ]  [mono项目在本地运行](https://libs.fe.devfdg.net/)
- [ ]  [学会通过fake login登陆你的本地环境](/docs/101/bootcamp/tools/#fake-login)

## first week todo list
### 1. 尝试提交一个pr且发布

- [ ]  [启动本地开发环境,修改代码](https://libs.fe.devfdg.net/#dev--doc)
- [ ]  [提交代码,并且写一个规范的 commit meaasge](https://www.npmjs.com/package/@commitlint/config-angular)
- [ ]  [提交pr到指定仓库](https://docs.github.com/en/github/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)
- [ ]  [发布新代码到开发与测试环境](https://libs.fe.devfdg.net/ci-cd/test-pr/)
- [ ]  [查看CICD状态,等待发布完成](https://libs.fe.devfdg.net/ci-cd/test-pr/#manual-running-jobs)


### 2. mono进阶
- [ ]  [在mono中创建一个项目](https://git.toolsfdg.net/mono/mono#generator)
- [ ]  [项目环境变量的配置](/docs/101/bootcamp/tools/#mono项目中的环境变量配置)
- [ ]  [添加一个页面,配置一条路由](https://shuvijs.github.io/shuvijs.org/docs/config#routes)
- [ ]  [使用一个Uikit组件,并且兼容 PC, Pad, Phone](https://libs.fe.devfdg.net/uikit-core/props#responsive-styles)
- [ ]  [在项目中调试一个Lib](https://libs.fe.devfdg.net/#dev--doc)
- [ ]  [了解基于prow的CI/CD架构](https://libs.fe.devfdg.net/ci-cd/architecture/)


### 3. 配置一个i18n多语言词条

现在i18n的词条统一由该需求的产品经理录入系统.

代码中i18n词条对应的 `Key` 由产品经理给出.

### 4. 小程序开发
- [ ]  [下载Binance小程序开发者工具](/docs/101/bootcamp/tools/#)
- [ ]  [申请小程序开发权限](/docs/101/bootcamp/permission/#小程序开发权限)
- [ ]  [本机安装安卓模拟器 (没有测试机的时候,可方便本机调试)](/docs/native/android/tech/infra/android-emulator)

### 5. 接一个需求,小试牛刀
- [ ]  开始任务时将 jira 状态调整至 `In progress`
- [ ]  [确认页面属于哪一个App](/docs/101/bootcamp/tools/#如何知道当前页面属于哪一个mono子项目)
- [ ]  需求完成之后,将状态切换为`In Test`，
- [ ]  在该任务回复 你提交的`PR Link`，并将该 jira 状态指派给对应测试


## 其他

* [如何将静态文件上传到CDN](/docs/101/bootcamp/tools/#静态文件上传到cdn)
* [如何使用安卓模拟器调试小程序](/docs/101/bootcamp/tools/#小程序篇)
* [mono开发问题解决技巧](/docs/101/bootcamp/tools/#mono篇)

