---
id: onboarding-overview
title: Overview
---

欢迎加入 Binance 前端大家庭！🥳 接下来你会加入 **Finance Futures** 业务组。为了让你更快地进入角色，请你仔细阅读这份入职指南并完成目标～

**Week 目标**

* 了解[公司文化](https://docs.google.com/document/d/1xuASZfxaPSpYOj7Aoax5XaJWc5iVcFd-rAlP7HU9oDc/edit#heading=h.xtjrl0ezau16)
* okta 开通各应用程式权限
* clone repo 并启动项目
* 熟悉开发环境，开通 QA 测试账号
* 在开发环境完成登录、下单
* 熟悉 Futures 业务

目标指南

* 开通权限
  登陆 [okta](https://binance.okta.com/) (https://binance.okta.com) 查看自己是否能够看到 JIRA、GitHub Enterprise。如没有，找联系人 Jack Ding (jack.ding@binance.com) 开通。
  * Confluence & JIRA
    * APP [james.he@binance.com](http://confluence.toolsfdg.net/display/~james.he@binance.com)
    * Futures [julia.chen@binance.com](http://confluence.toolsfdg.net/display/~julia.chen@binance.com)
      查看是否有一下链接的权限：
      * https://jira.toolsfdg.net/browse/FUT-9522?src=confmacro
      * https://confluence.toolsfdg.net/display/FUT/App+release+0819
    * Github
      * 提供下 github-id，联系下 [james.he@binance.com](http://confluence.toolsfdg.net/display/~james.he@binance.com)
      * 开发者权限
        * 提供下邮箱，找下 [james.he@binance.com](http://confluence.toolsfdg.net/display/~james.he@binance.com)
* 关于 Git cli 或 Git 客户端
  我们的 GHE 不允许使用 SSH key，只能用 https 的方式拉取代码仓库。使用 GHE 用户名（注意，用户名不是 Binance 邮箱）作为账号，使用 access token 作为密码，通过【setting -> developer setting -> Personal access tokens】来获取 access token。成功之后，需要 clone [ https://git.toolsfdg.net/fe/ios-client](https://git.toolsfdg.net/fe/ios-client)，尝试在本地编译，有问题联系 Johnson (nan.jiang@binance.com)。
* 关于测试环境
  测试账号系统：http://py-user.k8s.qa1fdg.net/users/ ，使用、登录下单咨询 Johnson。
  接口文档：
  http://internal-tk-dev-backend-forward-alb-1658290603.ap-northeast-1.elb.amazonaws.com:9024/swagger-ui.html# (线上接口)
  http://eureka1.qa1fdg.net:8761/ （灰度中的接口: 1.搜索 BINANCE-MGS-LENDING 2.根据后台提供的灰度标签，如 dev 点击链接进入）

**First Week 目标**

* 完成环境配置，熟悉项目

* 阅读 [teams 文档](https://git.toolsfdg.net/fe/iOS-Team-Portal)

目标指南

* 环境配置
  * Homebrew 下载： https://brew.sh/
  * CocoaPods 相关流程
    * 查看 ruby 环境 ruby -v
    * 查看源 gem sources -l
    * 源地址为 https://rubygems.org/
    * 删除指令：gem sources --remove[ https://rubygems.org/](https://rubygems.org/)
    * 新增指令：gem sources -a[ https://gems.ruby-china.com/](https://gems.ruby-china.com/)
    * sudo gem install cocoapods
  * Git Large File Storage 安装
    * 由于特定 SDK 大小问题，部分 SDK 集成到私有仓库，需要配置 GLFS: https://git-lfs.github.com/
    * 指令：
      * brew install git-lfs
      * git lfs install
      * 把工程目录下 Podfile.lock Pods 这两个删了
      * pod cache clean --all
      * pod install
  * 目前项目做了编译优化，切分支后用如下命令更新
    * bundle install
      bundle exec pod install --repo-update

**First Month 目标**

* 独立完成一个 JIRA Task
* 独立找到一个性能问题并修复
* 给项目提一个优化的建议

目标指南

在第一个月后找 James He 设立自己的半年规划。

* 测试机申请， 真机调试文档
* 组织架构 BU职责范围
* CICD流程
* 每一期发版流程 组件开发branch 说明
* 专业知识文档 QA环境验证码 111111
* 灰度标签说明
* 多啦A梦介绍
* charles key
* code review 文档
* PM QA 负责人
* VPN 权限
* 多语言文档
* 处理流程code owener 文档化
